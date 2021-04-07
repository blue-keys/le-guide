# How Netflix Scales its API with GraphQL Federation \(Part 2\)

[Netflix Technology Blog](https://medium.com/@NetflixTechBlog?source=post_page-----bbe71aaec44a--------------------------------)Follow[Dec 11](https://netflixtechblog.com/how-netflix-scales-its-api-with-graphql-federation-part-2-bbe71aaec44a?source=post_page-----bbe71aaec44a--------------------------------) · 11 min read

![](https://miro.medium.com/fit/c/96/96/1*BJWRqfSMf9Da9vsXG9EBRQ.jpeg)

In our [previous post](https://netflixtechblog.com/how-netflix-scales-its-api-with-graphql-federation-part-1-ae3557c187e2) and [QConPlus talk](https://www.infoq.com/presentations/netflix-api-graphql-federation/), we discussed GraphQL Federation as a solution for distributing our GraphQL schema and implementation. In this post, we shift our attention to what is needed to run a federated GraphQL platform successfully — from our journey implementing it to lessons learned.![Netflix GraphQL Federation](https://miro.medium.com/max/60/1*tHy8v5LnTwqLcvLglyIErw.png?q=20)

![Netflix GraphQL Federation](https://miro.medium.com/max/1958/1*tHy8v5LnTwqLcvLglyIErw.png)

## Our Journey so Far <a id="c739"></a>

Over the past year, we’ve implemented the core infrastructure pieces necessary for a federated GraphQL architecture as described in our previous post:![Studio Edge Architecture Diagram](https://miro.medium.com/max/60/1*nRWGvrsrrhbx2vPXcbOsqw.png?q=20)**Studio Edge Architecture**

![Studio Edge Architecture Diagram](https://miro.medium.com/max/2880/1*nRWGvrsrrhbx2vPXcbOsqw.png)

The first Domain Graph Service \(DGS\) on the platform was the former GraphQL monolith that we discussed in our first post \(Studio API\). Next, we worked with a few other application teams to make DGSs that would expose their APIs alongside the former monolith. We had our first Studio applications consuming the federated graph, without any performance degradation, by the end of the 2019. Once we knew that the architecture was feasible, we focused on readying it for broader usage. Our goal was to open up the Studio Edge platform for self-service in April 2020.

April 2020 was a turbulent time with the pandemic and overnight transition to working remotely. Nevertheless, teams started to jump into the graph in droves. Soon we had hundreds of engineers contributing directly to the API on a daily basis. And what about that Studio API monolith that used to be a bottleneck? We migrated the fields exposed by Studio API to individually owned DGSs without breaking the API for consumers. The original monolith is slated to be completely deprecated by the end of 2020.

This journey hasn’t been without its challenges. The biggest challenge was aligning on this strategy across the organization. Initially, there was a lot of skepticism and dissent; the concept was fairly new and would require high alignment across the organization to be successful. Our team spent a lot of time addressing dissenting points and making adjustments to the architecture based on feedback from developers. Through our prototype development and proactive partnership with some key critical voices, we were able to instill confidence and close crucial gaps.

Once we achieved broad alignment on the idea, we needed to ensure that adoption was seamless. This required building robust core infrastructure, ensuring a great developer experience, and solving for key cross-cutting concerns.

## Core Infrastructure <a id="4779"></a>

Our GraphQL Gateway is based on Apollo’s reference implementation and is written in [Kotlin](https://kotlinlang.org/). This gives us access to Netflix’s Java ecosystem, while also giving us the robust language features such as [coroutines](https://kotlinlang.org/docs/reference/coroutines-overview.html) for efficient parallel fetches, and an expressive type system with null safety.

The schema registry is developed in-house, also in Kotlin. For storing schema changes, we use an [internal library](https://netflixtechblog.com/scaling-event-sourcing-for-netflix-downloads-episode-2-ce1b54d46eec) that implements the [event sourcing pattern](https://microservices.io/patterns/data/event-sourcing.html) on top of the [Cassandra](https://cassandra.apache.org/) database. Using event sourcing allows us to implement new developer experience features such as the Schema History view. The schema registry also integrates with our CI/CD systems like [Spinnaker](https://spinnaker.io/) to automatically setup cloud networking for DGSs.

## Developer Education & Experience <a id="23dd"></a>

In the previous architecture, only the monolith Studio API team needed to learn GraphQL. In Studio Edge, every DGS team needs to build expertise in GraphQL. GraphQL has its own learning curve and can get especially tricky for complex cases like [batching](https://www.graphql-java.com/documentation/v15/batching/) & [lookahead](https://www.graphql-java.com/blog/deep-dive-data-fetcher-results/). Also, as discussed in the previous post, understanding GraphQL Federation and implementing entity resolvers is not trivial either.

We partnered with Netflix’s Developer Experience \(DevEx\) team to build out documentation, training materials, and tutorials for developers. For general GraphQL questions, we lean on the open source community plus cultivate an internal GraphQL community to discuss hot topics like pagination, error handling, nullability, and naming conventions.

### DGS Framework & Developer Tools <a id="8830"></a>

To make it easy for backend engineers to build a GraphQL DGS, the DevEx team built a “DGS Framework” on top of GraphQL Java and Spring Boot. The framework takes care of all the cross-cutting concerns of running a GraphQL service in production while also making it easier for developers to write GraphQL resolvers. In addition, DevEx built robust tooling for pushing schemas to the Schema Registry and a Self Service UI for browsing the various DGS’s schemas. Check out their [conference talk](https://plus.qconferences.com/plus2020/presentation/using-devex-accelerate-graphql-federation-adoption-netflix) and expect a future blog post from our colleagues. The DGS framework is planned to be open-sourced in early 2021.

## Schema Governance <a id="3555"></a>

Netflix’s studio data is extremely rich and complex. Early on, we anticipated that active schema management would be crucial for schema evolution and overall health. We had a Studio Data Architect already in the org who was focused on data modeling and alignment across Studio. We engaged with them to determine graph schema best practices to best suit the needs of Studio Engineering.

Our goal was to design a GraphQL schema that was reflective of the domain itself, not the database model. UI developers should not have to build Backends For Frontends \(BFF\) to massage the data for their needs, rather, they should help shape the schema so that it satisfies their needs. Embracing a collaborative schema design approach was essential to achieving this goal.![Schema Design Workflow Diagram](https://miro.medium.com/max/60/1*0TKgbUeZLcX6MB4o2zvFlw.png?q=20)**Schema Design Workflow**

![Schema Design Workflow Diagram](https://miro.medium.com/max/2546/1*0TKgbUeZLcX6MB4o2zvFlw.png)

The collaborative design process involves feedback and reviews across team boundaries. To streamline schema design and review, we formed a schema working group and a managed technical program for on-boarding to the federated architecture. While reviews add overhead to the product development process, we believe that prioritizing the quality of the graph model will reduce the amount of future changes and reworking needed. The level of review varies based on the entities affected; for the core federated types, more rigor is required \(though tooling helps streamline that flow\).

We have a deprecation workflow in place for evolving the schema. We’ve leveraged GraphQL’s [deprecation](https://spec.graphql.org/June2018/#sec--deprecated) feature and also track usage stats for every field in the schema. Once the stats show that a deprecated field is no longer used, we can make a backward incompatible change to remove the field from the schema.![Clients with Deprecated Field Usage](https://miro.medium.com/max/60/1*f1y5s4bNunDJYMmE9lZJ8A.png?q=20)**Clients with Deprecated Field Usage**

![Clients with Deprecated Field Usage](https://miro.medium.com/max/2391/1*f1y5s4bNunDJYMmE9lZJ8A.png)

We embraced a schema-first approach instead of generating our schema from existing models such as the [Protobuf](https://developers.google.com/protocol-buffers) objects in our [gRPC](https://grpc.io/docs/what-is-grpc/introduction/) APIs. While Protobufs and gRPC are excellent solutions for building service APIs, we prefer decoupling our GraphQL schema from those layers to enable cleaner graph design and independent evolvability. In some scenarios, we implement generic mapping code from GraphQL resolvers to gRPC calls, but the extra boilerplate is worth the long-term flexibility of the GraphQL API.

Underlying our approach is a foundation of “context over control”, which is a [key tenet of Netflix’s culture](https://jobs.netflix.com/culture). Instead of trying to hold tight control of the entire graph, we give guidance and context to product teams so that they can apply their domain knowledge to make a flexible API for their domain. As this architecture matures, we will continue to monitor schema health and develop new tooling, processes, and best practices where needed.

## Observability <a id="30ce"></a>

In our previous architecture, observability was achieved through manual analysis and routing via the API team, which scaled poorly. For our federated architecture, we prioritized solving observability needs in a more scalable manner. We prioritized three areas:

* _Alerting_ — report **when** something goes awry
* _Discovery_ — easily determine **what** isn’t working
* _Diagnosis_ — debug **why** something isn’t working

Our guiding metrics in this space are mean time to resolution \(MTTR\) and service level objectives and indicators \(SLO/SLI\).

We teamed up with experts from Netflix’s Telemetry team. We integrated the **Gateway** and **DGS** architectural components with [Zipkin](https://zipkin.io/), the internal distributed tracing tool [Edgar](https://netflixtechblog.com/edgar-solving-mysteries-faster-with-observability-e1a76302c71f), and application monitoring tool [TellTale](https://netflixtechblog.com/telltale-netflix-application-monitoring-simplified-5c08bfa780ba). In GraphQL, almost every response is a 200 with custom errors in the error block. We introspect these custom error codes from the response and emit them to our metrics server, [Atlas](https://netflixtechblog.com/introducing-atlas-netflixs-primary-telemetry-platform-bd31f4d8ed9a). These integrations created a great foundation of rich visibility and insights for the consumers and developers of the GraphQL API.![Trace for a Federated Request Lifecycle](https://miro.medium.com/max/48/1*shwDi-2MMXmlbmM9VI7e9A.png?q=20)**Edgar Trace for a Federated Request Lifecycle**![Timeline View for a Federated Request lifecycle](https://miro.medium.com/max/60/1*d2BlJ7wlEE2Q7smW6zFMWA.png?q=20)**Timeline View for a Federated Request**

![Timeline View for a Federated Request lifecycle](https://miro.medium.com/max/5350/1*d2BlJ7wlEE2Q7smW6zFMWA.png)

![Trace for a Federated Request Lifecycle](https://miro.medium.com/max/3424/1*shwDi-2MMXmlbmM9VI7e9A.png)

Distributed Log Correlation helps with debugging more complex server issues. By surfacing the application level logging details for all systems involved in processing a request, we gain deeper insights into what happened across the stack. Developers can easily see what was happening around the same time as a given request, to inspect surrounding factors that might have impacted an interaction.![Log correlation across multiple services for a request lifecycle](https://miro.medium.com/max/60/1*6_45mJHYesw0AMTgvQc7MA.png?q=20)**Logs across multiple services for a Federated Request**

![Log correlation across multiple services for a request lifecycle](https://miro.medium.com/max/2698/1*6_45mJHYesw0AMTgvQc7MA.png)

To solve the “**who** do I ask about…” routing problem, we integrated deep linking from GraphQL types and fields to their owning team’s support channels. Finding support is now as simple as clicking a link from a trace, which helps shorten MTTR and reduce the number of times the gateway team needs to get involved.

## Securing the Federated Graph <a id="3e89"></a>

Our goal is to enable robust and consistent security practices across the federated architecture. To achieve this, we partnered with the security experts at Netflix to build security into the graph. Let’s look at two essential parts of our security solution: AuthN and AuthZ.

### Authentication <a id="57af"></a>

All of our product experiences in the Studio space require an authenticated account, so we restrict the GraphQL Gateway access to only trusted authenticated callers. Additionally, [Graph Introspection](https://graphql.org/learn/introspection/) is restricted to Netflix internal developers.

### Authorization <a id="776c"></a>

Before Studio Edge, authorization logic was fragmented across teams. Some teams implemented authorization in their BFFs, some in microservices, and others did both for good measure. The result was often a different authorization story for a given piece of data depending on which UI a user was accessing it through. UI teams also found themselves needing to implement \(and re-implement\) authorization checks with each new frontend.

In Studio Edge, we delegated the authorization responsibility to DGS owners. This resulted in consistent authorization for the same user across different applications. Plus, Product Managers, Engineers and the Security team can easily get a bird’s eye view of who has access to each data type and how.

We have multiple authorization offerings within Netflix: from a simple system that grants access based on user identity to a more granular system that brings in the concept of roles and capabilities. DGS developers can choose a solution based on their needs. Then they simply annotate their resolvers with [@Secured](https://docs.spring.io/spring-security/site/docs/3.2.8.RELEASE/apidocs/org/springframework/security/access/annotation/Secured.html) annotation and configure that to use one of the available systems. If needed, more complex authorization can be implemented in the resolver or in downstream systems.

### Future of Authorization <a id="48d0"></a>

We are currently prototyping a GraphQL-aware authorization solution. The Schema Registry automatically generates Access Control Groups \(ACGs\) for each field and its corresponding type when its schema is registered. Product managers & DGS Engineers decide membership and rules for these generated ACGs. Since the ACGs map to a field in GraphQL, the DGS framework then automatically applies the rules associated with the ACG during execution.

## Architecting for Failure <a id="5737"></a>

The GraphQL Gateway is the single entry point for all requests; a failure on the gateway can cause significant disruptions. Following Netflix engineering best practices, we assume failures will happen and design ways to mitigate the impact of those failures. These are our design principles for ensuring the gateway layer is resilient:

1. Single purpose
2. Stateless service
3. Demand controlled
4. Multi-region
5. Sharded by functionality

First, we focus the responsibilities of the gateway layer on a single purpose: parse client queries, then build and execute query plans. By reducing the scope, we limit the range of problems that can occur. We aim to perform any additional resource-intensive operations off-box with the exception of logging and metrics. Taking on additional unrelated logic in the gateway layer could increase surface area for failures in this critical tier.

Second, we run multiple stateless instances of the gateway service. Any gateway instance is able to generate and execute a query plan for any request. When we do code changes to the gateway layer, we rigorously test them before rolling out to production.

Third, we seek to balance the resources each request consumes through applying demand control. We rate-limit callers to avoid overloading the underlying databases that are the source of most of our domain elements. We also run a static query cost calculation on all incoming queries and reject expensive queries to avoid gridlock in gateway and DGS resources. Our partners understand these tradeoffs and work with us to meet these requirements, reworking expensive queries and reducing high volume callers.

Fourth, we deploy our gateway layer to multiple AWS regions around the world. This allows us to limit the blast radius for problems that inevitably arise. When problems happen, we can fail over to another region to ensure our clients are minimally impacted.

Last, we deploy multiple functional shards of our gateway layer. The code is the same in each shard and incoming requests are routed based on category. For example, GraphQL subscriptions generally result in long-lived connections while Queries & Mutations are short-lived. We use a separate fleet of instances for Subscriptions so “running out of connections” does not affect the availability of Queries and Mutations.

There is more we can do to improve resilience. We have plans to do [canary deployments and analysis](https://netflixtechblog.com/automated-canary-analysis-at-netflix-with-kayenta-3260bc7acc69) for gateway deployments and, eventually, schema changes. Today, our gateway dynamically updates its schema by polling the schema registry. We are in the process of decoupling these by storing the federation config in a versioned S3 bucket, making the gateway resilient to schema registry failures.

## Closing Thoughts <a id="1756"></a>

GraphQL and Federation have been a productivity multiplier for Studio applications. Motivated by this, we’ve recently prototyped using GraphQL Federation for the Netflix consumer app search page on iOS & Android. To do this, we created three DGSs to provide the data for a minimal portion of the consumer graph. We are sending a small subset of users to this alternative stack and measuring high-level metrics. We are excited to see the results and explore further applicability in the Netflix consumer space.

Despite our positive experience, GraphQL Federation is early in its maturity lifecycle and may not be the best fit for every team or organization. Learning GraphQL and DGS development, running a federation layer, and doing a migration requires high commitment from partner teams and seamless cross-functional collaboration. If you’re considering going in this direction, we recommend checking out Apollo’s [SaaS offering](https://www.apollographql.com/docs/federation/) for Federation and the many online resources for learning GraphQL. For ecosystems like ours with a large swath of microservices that need to be aggregated together, the development velocity and improved operability has made the transition worth it.

In closing, we want to hear from you! If you have already implemented federation or tried to solve this problem with another approach, we would love to learn more. Sharing knowledge is one of the ways our industry learns and improves rapidly. Finally, if you’d like to be a part of solving complex and interesting problems like this at Netflix scale, check out [our jobs page](https://jobs.netflix.com/) or reach out to us directly.

_By_ [_Tejas Shikhare_](https://www.linkedin.com/in/tejas-shikhare-81027b19/)_, Edited by_ [_Philip Fisher-Ogden_](https://www.linkedin.com/in/philfish)

_Additional Credits:_ [_Stephen Spalding_](https://twitter.com/stephenspalding)_,_ [_Jennifer Shin_](https://www.linkedin.com/in/jennifer-shin-0019a516/)_,_ [_Robert Reta_](https://www.linkedin.com/in/robert-reta-a17b918/)_,_ [_Antoine Boyer_](https://www.linkedin.com/in/antoineboyer/)_,_ [_Bruce Wang_](https://www.linkedin.com/in/batmany13/)_,_ [_David Simmer_](https://www.linkedin.com/in/simmerer/)

_Source :_ [_https://docs.digitall.zone/kecharrm/nouveautes/netflix-technologies/how-netflix-scales-its-api-with-graphql-federation-part-2_](https://docs.digitall.zone/kecharrm/nouveautes/netflix-technologies/how-netflix-scales-its-api-with-graphql-federation-part-2)\_\_

