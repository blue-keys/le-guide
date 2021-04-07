# 🇬🇧 Applying Angular Runtime Configurations in Dockerized Environments \| Hacker Noon

![](https://images.unsplash.com/photo-1498050108023-c5249f4df085?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1080&fit=max&ixid=eyJhcHBfaWQiOjEwMDk2Mn0)

With the shift to Cloud-first and the rise of managed infrastructure and orchestrations such as EWS, Azure AKS or GCP clusters the application landscape needs to prepare and adjust itself to match newly rising requirements. This is for such nothing new nor unknown, but acknowledging this fact is one and probably the first important step.

**Outside-In Deep Dive**

Let’s break the technical architecture down by starting from the user perspective. How does the technical setup under the hood look like when a user visits an application orchestrated by Kubernetes from the point onwards where the application's URL is entered to the browser?

![](https://hackernoon.com/images/xzrvis6CEHg5Uaaa5rLU41UQJEo2-l6246z8.jpg)

_Graphic by_ [_https://www.linkedin.com/in/theresa-ohm-06b7a156_](https://www.linkedin.com/in/theresa-ohm-06b7a156?ref=hackernoon.com)

The first configuration to kick in is the DNS config mapping the URL to an IP address, in these scenarios the IP of a load balancer. The load balancer having close to no other job that fulfilling the name, it redirects the user’s request to the orchestration where a web server is listening. As we do operate kubernetes environments we will limit ourselves in here, by specifying this web server as the ingress controller of the kubernetes cluster. There are multiple types of ingress controllers. We decided to go with a default option using an nginx controller. 

As a side node, this layer is subject to be the first layer where an application specific configuration may be required, f.e. when the application shall transmit larger data \(proxy-body-sizes\) and/or support long-lasting operations \(proxy-timeouts\), since the nginx ingress controller has implicit default configurations.

Underneath the ingress controller, the orchestration takes place as via a deployment the kubernetes pod\(s\) are serving the application container. Here the deployment can be configured to specify the environment the container should be served in. We again took an explicit design decision talking about the containers.

Our containers are docker images that do not only include the built angular application \(static javascript\), but a nginx web server as well, since we want to be able to run the docker container independently from the infrastructure and it’s orchestration, f.e. for development and/or testing purposes. We went for this approach and prioritized flexibility and comfort over performance. The docker image is built completely normal, using a multi-stage build to compile the angular source code and bundle the output together with the configured nginx web server.

**Inside-Out Deep Dive**

Having described the above Outside-In perspective, let’s now concentrate on Angular and its default characteristics. The concepts of Angular do include environments, a config or definition intended to be used for these scenarios, where different configurations should be applied for the application depending on whether you develop locally or run it in production. The downside of the Angular concept is, that it is a build-time configuration only.

This means that when running an Angular application, it is at first compiled into static JavaScript and the environment configuration is fixed at compile time as well. Since there is no way to circumvent this static code, you cannot configure the application depending on the infrastructure it is operated on, unless you know and do it in advance.

_How to change a configuration into a runtime-configuration_

A first important step to redesign an Angular application as such that it pairs well with Docker and orchestration is, to change the environment strategy to a run time configuration one. This can be achieved by getting rid of the default concept of environments using a Ajax/XHR call to retrieve the configuration dynamically instead. This is in more detail well described by @juristr in [https://juristr.com/blog/2018/01/ng-app-runtime-config/](https://juristr.com/blog/2018/01/ng-app-runtime-config/?ref=hackernoon.com).

In short, we implement a config service, which is instantiated on the start of the application using the appResolver fetching the config via an XHR call. The resolver is registered to the appRoutes as a guard. The application’s modules can simple inject the config.

_app.routes.ts_

```text
import { AppResolverService } from '@app/resolvers/app-resolver.service';
import { IndexComponent } from '@views/index/index.component';

export const routes: Routes = [
  {
    path: '',
    canActivate: [AppResolverService],
    children: [
      {
        path: '',
        component: IndexComponent,
        pathMatch: 'full'
      }
    ]
  }
];
```

_app-resolver.service.ts_

```text
import { Injectable } from '@angular/core';
import { ConfigService } from '@services/config/config.service';
import { EMPTY, Observable } from 'rxjs';
import { catchError, map } from 'rxjs/operators';
import { CanActivate, UrlTree } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class AppResolverService implements CanActivate {
  constructor(private config: ConfigService) {}

  canActivate(): Observable | Promise | boolean | UrlTree {
    return this.config.loadAppConfig().pipe(
      map(() => true),
      catchError(() => EMPTY)
    );
  }
}
```

_config.service.ts_

```text
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { map } from 'rxjs/operators';
import { Observable } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class ConfigService {
  private _config: any = {};

  constructor(private http: HttpClient) {}

  get data(): any {
    return this._config ? { ...this._config } : {};
  }

  loadAppConfig(): Observable {
    const headers = new HttpHeaders().set('Content-Type', 'application/json; charset=utf-8');

    return this.http.get(`/_ngx-rtconfig.json?cb=${new Date().getTime()}`, { headers }).pipe(
      map(data => { 
        for (const key in data) {
          if (data.hasOwnProperty(key)) {
            this._config[key.replace('NGX_', '').toLowerCase().split('_').map((el, i) => (i > 0 ? el.charAt(0).toUpperCase() + el.slice(1) : el)).join('')] = data[key];
          }
        }
        return this.data;
      })
    );
  }
}
```

If you take a close look we go one step further in our approach, also formalizing the structure of the json keys to be fetched. This is for good reason and will turn out to be a very crucial step.

By adding a unique prefix to the json keys, we can leverage this key later to dynamically assign values to the keys sourced from the respective environments. For local development this setup is straightforward, as we can simply but the values into the config.json file directly.

Going a step further, wrapping the angular application in a docker container, we need to adjust the config.json based on the container environment. Therefore we add shell script as docker-entrypoint during which the config.json with values from the current environment is \(re-\)generated.

_docker-entrypoint.sh_

```text
#!/usr/bin/env sh
set -eu

jq -n env | grep -E '\{|\}|NGX_' | sed ':begin;$!N;s/,\n}/\n}/g;tbegin;P;D' > /usr/share/nginx/html/_ngx-rtconfig.json

exec "$@"
```

_Dockerfile_

```text
FROM node:12.6.0-buster-slim AS builder

ARG NODE_ENV
ENV NODE_ENV ${NODE_ENV}
RUN mkdir -p /app
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile
COPY . .
RUN yarn build


FROM nginx:1.15-alpine

RUN apk update && apk add jq
COPY --from=builder build/ /usr/share/nginx/html
COPY ./ops/config/nginx-custom.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
# generate dynamic json from env
COPY ./ops/config/docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["nginx", "-g", "daemon off;"]
```

We used jq and some basic scripting here, but there are of course dozens of ways to achieve the same. This approach does allow us to specify the angular config values as env variables in docker-compose as well as in the deployment manifest. 

_docker-compose.yml_

```text
version: "3.5"
services:
  ngx-app:
    container_name: ngx-app
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      NGX_API_ENDPOINT: http://localhost:3000
    ports:
      - '4200:80'
```

_deployment.yml_

```text
---
  apiVersion: extensions/v1beta1
  kind: Deployment
  metadata:
    name: {{ CI_PROJECT_NAME }}
  spec:
    replicas: 3
    revisionHistoryLimit: 3
    template:
      spec:
        containers:
        - name: {{ CI_PROJECT_NAME }}
          image: {{ CI_REGISTRY_IMAGE }}:{{ CI_COMMIT_SHA }}
          imagePullPolicy: Always
          env:
          - name: NGX_API_ENDPOINT
            value: https://foo.com/api/
          ports:
          - name: http
            containerPort: 80
```

In the end we have accomplished to establish a flexible, yet consistent mechanism to manage angular configs/environments under different circumstances dynamically. So far we haven’t recognized any downsides of this approach!

**References:**

* \[1\] "How can I use environment variables in Nginx.conf" - [https://serverfault.com/questions/577370/how-can-i-use-environment-variables-in-nginx-conf](https://serverfault.com/questions/577370/how-can-i-use-environment-variables-in-nginx-conf?ref=hackernoon.com)
* \[2\] "How to use environment variables to configure your Angular application without a rebuild" - [https://www.jvandemo.com/how-to-use-environment-variables-to-configure-your-angular-application-without-a-rebuild/](https://www.jvandemo.com/how-to-use-environment-variables-to-configure-your-angular-application-without-a-rebuild/?ref=hackernoon.com)

Source : [https://hackernoon.com/applying-angular-runtime-configurations-in-dockerized-environments-kr3a33pr](https://hackernoon.com/applying-angular-runtime-configurations-in-dockerized-environments-kr3a33pr)

