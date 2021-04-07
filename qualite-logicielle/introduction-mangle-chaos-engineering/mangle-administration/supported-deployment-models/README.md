# Supported Deployment Models

## Single Node Deployments <a id="single-node-deployments"></a>

### Deploying the Mangle Virtual Appliance <a id="deploying-the-mangle-virtual-appliance"></a>

For a quick POC we recommend that you deploy a single node instance of Mangle using the OVA file that we have made available [here](https://bintray.com/vmware/mangle/MangleAppliance).

Using the OVA is a fast and easy way to create a Mangle VM on VMware vSphere.

After you have downloaded the OVA, log in to your vSphere environment and perform the following steps:

1. Start the Import Process

   From the Actions pull-down menu for a datacenter, choose **Deploy OVF Template**.

   ​![Create/Register VM](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Le_gjgWKixKeQjdCCh9%2F-Le_jKHjCV-SAsgBGSTp%2FStep1_Deploy%20OVF%20Template.png?alt=media&token=ecda4cc9-56c7-4a57-a18d-0dab0a85f6fe)​

   Provide the location of the downloaded ova file.

   Choose **Next**.

2. Specify the name and location of virtual machine

   Enter a name for the virtual machine and select a location for the virtual machine.

   ​![OVA file](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Le_gjgWKixKeQjdCCh9%2F-Le_k2ZMHxj1wBuCt-qD%2FStep2_Select%20Name.png?alt=media&token=55c24986-cb73-4b27-b593-b99d24e71aad)​

   Choose **Next**.

3. Specify the compute resource

   Select a cluster, host or resource pool where the virtual machine needs to be deployed.

   ​![Target datastore](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Le_gjgWKixKeQjdCCh9%2F-Le_lhNspXKBzdrUy_kg%2FStep3_Select%20Compute%20Resource.png?alt=media&token=52da60c5-c0c3-4106-a9d0-6cbcc791f2fa)​

   Choose **Next**.

4. Review details

   ​![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Le_rmWyU-B7kolINBpg%2F-Le_sCd7un_ZYcJjQY01%2FStep4_Review%20Details.png?alt=media&token=13d4af4a-2a08-47ca-925c-1e8d09b7590d)

   Choose **Next**.

5. Accept License Agreement

   Read through the Mangle License Agreement, and then choose **I accept all license agreements**.

   ​![License](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Le_gjgWKixKeQjdCCh9%2F-Le_ni0wG2Ni0-foiQVg%2FStep5_Accept%20License.png?alt=media&token=fae02f53-88bb-4799-a09b-4f1d7a264b33)​

   Choose **Next**.

6. Select Storage

   Mangle is provisioned with a maximum disk size. By default, Mangle uses only the portion of disk space that it needs, usually much less that the entire disk size \( **Thin** client\). If you want to pre-allocate the entire disk size \(reserving it entirely for Mangle instead\), select **Thick** instead.

   ​![Deployment Options](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Le_gjgWKixKeQjdCCh9%2F-Le_oDZkncnQNKqqA3pM%2FStep6_Select%20Storage.png?alt=media&token=3225933b-ae41-450f-8881-c17ba1f38b15)​

   Choose **Next**.

7. Select Network

   Provide a static or dhcp IP for Mangle after choosing an appropriate destination network. ![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Le_gjgWKixKeQjdCCh9%2F-Le_p0vfBXTVTVa361y6%2FStep7_Select%20Network.png?alt=media&token=7cb17fe0-9d78-4827-b554-37a64c98fb24)

   ​![](https://firebasestorage.googleapis.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LcVKiIEQZ_SDz8uqA0g%2F-Le_gjgWKixKeQjdCCh9%2F-Le_pjjb7V76Wm-Mk5Cw%2FStep8_Customize%20Template.png?alt=media&token=13857aa7-8be0-4ee0-9a52-ea2637053d2e)

   Choose **Next**.

8. Verify Deployment Settings and click **Finish** to start creating the virtual machine. Depending on bandwidth, this operation might take a while. When finished, vSphere powers up the Mangle VM based on your selections.

After the VM is boots up successfully, open the command window. Press Alt+F2 to log into the VM.

The default account credentials are:

* Username: `root`
* Password: `vmware`

Because of limitations within OVA support on vSphere, it was necessary to specify a default password for the OVA option. However, for security reasons, we would recommend that you modify the password after importing the appliance.

It takes a couple of minutes for the containers to run. Once the Mangle application and DB containers are running, the Mangle application should be available at the following URL:

You will be prompted to change the admin password to continue.

* Default Mangle Username: [`[email protected]`](https://vmware-1.gitbook.io/cdn-cgi/l/email-protection)
* Password: `admin`

Export the VM as a Template \(Optional\)

Consider converting this imported VM into a template \(from the Actions menu, choose **Export** \) so that you have a master Mangle instance that can be combined with vSphere Guest Customization to enable rapid provisioning of Mangle instances.

Now you can move on to the [Mangle Users Guide](../../mangle-users-guide.md).

### Deploying the Mangle Containers <a id="deploying-the-mangle-containers"></a>

#### Prerequisites <a id="prerequisites"></a>

Before creating the Mangle container a Cassandra DB container should be made available on a Docker host. You can choose to deploy the DB and the Application container on the same Docker host or on different Docker hosts. However, we recommend that you use a separate Docker host for each of these. You can setup a Docker host by following the instructions [here](https://docs.docker.com/install/).

To deploy Cassandra, you can either use the authentication enabled image tested and verified with Mangle available on the Mangle Docker repo or use the default public Cassandra image hosted on Dockerhub.

**If you chose to use the Cassandra image from Mangle Docker Repo:**

```text
docker run --name mangle-cassandradb -v /cassandra/storage/:/var/lib/cassandra -p 9042:9042 -d -e CASSANDRA_CLUSTER_NAME="manglecassandracluster" -e CASSANDRA_DC="DC1" -e CASSANDRA_RACK="rack1" -e CASSANDRA_ENDPOINT_SNITCH="GossipingPropertyFileSnitch"  mangleuser/mangle_cassandradb:
```

**If you chose to use the Cassandra image from** [**Dockerhub**](https://hub.docker.com/_/cassandra/)**:**

```text
docker run --name mangle-cassandradb -v /cassandra/storage/:/var/lib/cassandra -p 9042:9042 -d -e CASSANDRA_CLUSTER_NAME="manglecassandracluster" -e CASSANDRA_DC="DC1" -e CASSANDRA_RACK="rack1" -e CASSANDRA_ENDPOINT_SNITCH="GossipingPropertyFileSnitch" cassandra:3.11
```

#### **Deploying the Mangle application container** <a id="deploying-the-mangle-application-container"></a>

To deploy the Mangle container using a Cassandra DB deployed using the image from Mangle Docker repo or with DB authentication and ssl enabled, run the docker command below on the docker host after substituting the values in angle braces &lt;&gt; with actual values.

```text
docker run --name mangle -d -e DB_OPTIONS="-DcassandraContactPoints= -DcassandraSslEnabled=true -DcassandraUsername=cassandra -DcassandraPassword=cassandra" -e CLUSTER_OPTIONS="-DclusterValidationToken=mangle -DpublicAddress=" -p 8080:8080 -p 8443:8443 mangleuser/mangle:
```

To deploy the Mangle container using a Cassandra DB deployed using the image from Dockerhub or with DB authentication and ssl disabled, run the docker command below on the docker host after substituting the values in angle braces &lt;&gt; with actual values.

```text
docker run --name mangle -d -e DB_OPTIONS="-DcassandraContactPoints= -DcassandraSslEnabled=false" -e CLUSTER_OPTIONS="-DclusterValidationToken=mangle -DpublicAddress=" -p 8080:8080 -p 8443:8443 mangleuser/mangle:
```

The Mangle docker container takes two environmental variables

"**DB\_OPTIONS**", which can take a list of java arguments identifying the properties of the database cluster

"**CLUSTER\_OPTIONS**", which can take a list of java arguments identifying the properties of the Mangle application cluster

Although the docker run commands above lists only a few DB\_OPTIONS and CLUSTER\_OPTIONS parameters, Mangle supports a lot more for further customization.

**Supported DB\_OPTIONS**

```text
-DcassandraContactPoints : IP Address of Cassandra DB (mandatory, default value is "localhost" and works only if the Mangle application and DB are on the same Docker host)-DcassandraClusterName : Cassandra cluster name (mandatory, value should be the one passed during cassandra db creation as an environmental variable CASSANDRA_CLUSTER_NAME)-DcassandraKeyspaceName : Cassandra keyspace name (optional, default value is "mangledb" if you are using the Cassandra image file from Mangle Docker repo)-DcassandraPorts : Cassandra DB port used (optional, default value is "9042")-DcassandraSslEnabled : Cassandra DB ssl configuration (optional, default value is "false"...mandatory and should be set to true if ssl is enabled)-DcassandraUsername : Cassandra DB username (mandatory only if the Cassandra DB is created with authentication enabled)-DcassandraPassword : Cassandra DB password (mandatory only if the Cassandra DB is created with authentication enabled)-DcassandraSchemaAction : Cassandra DB schema action (optional, default value is "create_if_not_exists")-DcassandraConsistencyLevel : Cassandra DB Consistency level (optional, default value is "local-quorum")-DcassandraSerialConsistencyLevel : Cassandra DB serial consistency level (optional, default value is "local-serial")-DcassandraDCName : Cassandra DB DC name (optional, value should be the one passed during cassandra db creation as an environmental variable CASSANDRA_DC or if it is set a value other than "DC1")-DcassandraNoOfReplicas : Cassandra DB replicas numbers (optional, default value is "1"...mandatory only if multiple nodes are available in the DB cluster)
```

**Supported CLUSTER\_OPTIONS**

```text
-DpublicAddress : IP address of the mangle node (mandatory)-DclusterValidationToken : Any string token name for mangle cluster (mandatory and should be kept in mind if more nodes need to be added to the cluster.)-DclusterName : Any string cluster name for mangle (optional, default value is "mangle")-DclusterMembers : Members in the mangle cluster (optional)
```

#### **Deploying the Mangle vCenter adapter container** <a id="deploying-the-mangle-vcenter-adapter-container"></a>

Mangle vCenter Adapter is a fault injection adapter for injecting vCenter specific faults. All the vCenter operations from the Mangle application will be carried out through this adapter.

To deploy the vCenter adapter container run the docker command below on the docker host.

```text
docker run --name mangle-vc-adapter -v /var/opt/mangle-vc-adapter-tomcat/logs:/var/opt/mangle-vc-adapter-tomcat/logs -d -p 8080:8080 -p 8443:8443 mangleuser/mangle_vcenter_adapter:
```

The API documentation for the vCenter Adapter can be found at:

_https://::/mangle-vc-adapter/swagger-ui.html_

The vCenter adapter requires authentication against any API calls. It supports only one user, _admin_ with password _admin_. All the post APIs that are supported by the adapter will take the vCenter information as a request body.

### Deploying Mangle on Kubernetes <a id="deploying-mangle-on-kubernetes"></a>

All the relevant YAML files are available under the Mangle git repository under location 'mangle\mangle-support\kubernetes'

1. Create a namespace called 'mangle' under the K8s cluster.

   `kubectl --kubeconfig=` `create namespace mangle`

   ​

2. Create Cassandra pod and service.

   `kubectl --kubeconfig=-n mangle apply -f/cassandra.yaml`

   `kubectl --kubeconfig=-n mangle apply -f/cassandra-service.yaml`

   ​

3. Create Mangle pod and service

   `kubectl --kubeconfig=-n mangle apply -f/mangle.yaml`

   `kubectl --kubeconfig=-n mangle apply -f/mangle-service.yaml`

## Multi Node Deployment <a id="multi-node-deployment"></a>

A multi-node setup for Mangle ensures availability in case of unexpected failures. We recommend that you use a 3 node Mangle setup.

#### Prerequisites <a id="prerequisites-1"></a>

You need at least 4 docker hosts for setting up a multi node Mangle instance; 1 for the Cassandra DB and 3 for Mangle application containers . You can setup a docker host by following the instructions [here](https://docs.docker.com/install/).

A multi node setup of Mangle is implemented using Hazelcast. Mangle multi node setup uses TCP connection to communicate with each other. The configuration of the setup is handled by providing the right arguments to the docker container run command, which identifies the cluster.

The docker container takes an environmental variable "**CLUSTER\_OPTIONS**", which can take a list of java arguments identifying the properties of the cluster. Following are the different arguments that should be part of "CLUSTER\_OPTIONS": **clusterName** - A unique string that identifies the cluster to which the current mangle app will be joining. If not provided, the Mangle app will by default use string "mangle" as the clusterName, and if this doesn't match the one already configured with the cluster, the node is trying to join to, container start fails. **clusterValidationToken** - A unique string which will act similar to a password for a member to get validated against the existing cluster. If the validation token doesn't match with the one that is being used by the cluster, the container will fail to start.

**publicAddress** - IP of the docker host on which the mangle application will be deployed. This is the IP that mangle will use to establish a connection with the other members that are already part of the cluster, and hence it is necessary to provide the host IP and make sure the docker host is discoverable from other nodes **clusterMembers** - This is an optional property that takes a comma-separated list of IP addresses that are part of the cluster. If not provided, Mangle will query DB and find the members of the cluster that is using the DB and will try connecting to that automatically. It is enough for mangle to connect to at least one member to become part of the cluster.

**deploymentMode** - Takes either CLUSTER/STANDALONE value. Deployment Mode parameter is mandatory for the multi-node deployment, with the value set to CLUSTER on every node that will be part of the cluster, where as for the single node deployment, this field can be ignored, which by default will be STANDALONE. If DB was used by one type of deployment setup, one needs to update this parameter to support the other type, either though the mangle or by directly going in to DB.

**NOTE:**

All the nodes \(docker hosts\) participating in the cluster should be synchronized with a single time server.

If a different mangle app uses the same clusterValidationToken, clusterName and DB of existing cluster, the node will automatically joins that existing cluster.

All the mangle app participating in the cluster should use the same cassandra DB.

The properties clusterValidationToken and publicAddress are mandatory for any mangle container spin up, if not provided container will fail to start.

Deploy a Cassandra DB container by referring to the section [here](./#prerequisites).

Deploy the Mangle cluster by bringing up the mangle container in each docker host.

**For the first node in the cluster:**

```text
docker run --name mangle -d -v /var/opt/mangle-tomcat/logs:/var/opt/mangle-tomcat/logs -e DB_OPTIONS="-DcassandraContactPoints=" -e CLUSTER_OPTIONS="-DclusterName= -DclusterValidationToken= -DpublicAddress= -DdeploymentMode=CLUSTER" -p 8080:8080 -p 443:8443 -p 5701:5701 mangleuser/mangle:
```

**For the subsequent nodes in the cluster:**

```text
docker run --name mangle -d -v /var/opt/mangle-tomcat/logs:/var/opt/mangle-tomcat/logs -e DB_OPTIONS="-DcassandraContactPoints=" -e CLUSTER_OPTIONS="-DclusterName= -DclusterValidationToken= -DpublicAddress= -DclusterMembers= -DdeploymentMode=CLUSTER" -p 8080:8080 -p 443:8443 -p 5701:5701 mangleuser/mangle:
```

```text
docker run --name mangle -d -v /var/opt/mangle-tomcat/logs:/var/opt/mangle-tomcat/logs -e DB_OPTIONS="-DcassandraContactPoints=" -e CLUSTER_OPTIONS="-DclusterName= -DclusterValidationToken= -DpublicAddress= -DclusterMembers= -DdeploymentMode=CLUSTER" -p 8080:8080 -p 443:8443 -p 5701:5701 mangleuser/mangle:
```

## Deployment Mode and Quorum <a id="deployment-mode-and-quorum"></a>

Mangle implements quorum strategy to for supporting HA and to avoid any split brain scenarios when deployed as a cluster.

Mangle can be deployed in two different deployment modes

1. **STANDALONE**: quorum value is 1, cannot be updated to any other value
2. **CLUSTER**: min quorum value is 2, cannot be set to value lesser than ceil\(\(n + 1\)/2\), n being number of nodes currently in the cluster

If a node is deployed initially in the STANDALONE mode, and user changes the deployment mode to CLUSTER, the quorum will automatically be updated to 2. When user keeps adding new nodes to the cluster, the quorum value updates depending on the number of nodes in the cluster. eg: when user adds new node to 3 node cluster\(quorum=2\) making it 4 nodes in the cluster, the quorum value is updated to 3. Determined by the formula, ceil\(\(n + 1\)/2\)

If a node is deployed initially in the CLUSTER mode, and user changes the deployment mode to STANDALONE mode, all other nodes except for the Hazelcast master will shutdown. They will lose the connection with the existing node, and won't take any post calls, and will entries will be removed from the cluster table.

**NOTE:**

Active members list of the active quorum will be maintained in DB under the table cluster. Similarly, master instance entry will also be maintained in the db under the same table.

### **Actions triggered when Mangle loses quorum** <a id="actions-triggered-when-mangle-loses-quorum"></a>

1. All the schedules will be paused
2. No post calls can be made against mangle
3. Task that were triggered on any node will continue to be executed until Failed/Completed
4. Removes its entry from the cluster table's members list
5. If the node was the last known master, it will remove it's entry as master from the table

### **Actions triggered when Mangle gets a quorum \(from no quorum in any of the cluster member\)** <a id="actions-triggered-when-mangle-gets-a-quorum-from-no-quorum-in-any-of-the-cluster-member"></a>

1. All the schedules that are in the scheduled state and all the schedules that were paused because of the quorum failure will be triggered
2. All the tasks that are in progress will be queue to executed after 5minutes of quorum created event \(This is to avoid any simultaneous Execution\)
3. Master \(Oldest member in the cluster\) will take care of adding the list of active members to the cluster table, and updating itself as master

### **Actions triggered when a Mangle instance joins one active cluster** <a id="actions-triggered-when-a-mangle-instance-joins-one-active-cluster"></a>

1. Some of the schedules will be assigned to the new the node due to migration
2. Some of the tasks will assigned to the new node, and they will be queued for triggering after 5mins
3. Existing tasks that in-progress will continue to execute in the old node
4. Master \(Oldest member in the cluster\) will take care of adding new member's entry to the list of active members to the cluster table

### **Scenarios around quorum** <a id="scenarios-around-quorum"></a>

#### **Scenario 1:** When a mangle instance in 3 node cluster, leaves a cluster due to network partition, and the quorum value is 2 <a id="scenario-1-when-a-mangle-instance-in-3-node-cluster-leaves-a-cluster-due-to-network-partition-and-the-quorum-value-is-2"></a>

* Let us consider 3 instances a, b, c in the CLUSTER
* a leaves the cluster due to network partition
* a is not able to communicate with b, c
* two cluster will be created, one having only a, and the other have b and c
* since for the cluster having a doesn't have quorum\(2\), it loses the quorum, and removes itself as active member from cluster config
* cluster having nodes b and c continue execute since they have enough nodes to satisfy the quorum

#### Scenario 2: When mangle instance in 2 node cluster leaves a cluster due to network partition, and the quorum value is 2 <a id="scenario-2-when-mangle-instance-in-2-node-cluster-leaves-a-cluster-due-to-network-partition-and-the-quorum-value-is-2"></a>

* Let us consider 2 instances a, b in the CLUSTER
* a leaves the cluster due to network partition, a cannot communicate with b and vice versa
* two clusters are created with each having one node
* both the cluster loses the quorum\(2\)

