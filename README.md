# Approuter User API Service

Content from github repository: <https://github.com/https://github.com/SAP-samples/sap-tech-bytes/tree/2021-02-20-approuter-user-api-service>

The content in this branch accompanies the post [SAP Tech Bytes: Approuter User API Service](https://blogs.sap.com/2021/02/20/sap-tech-bytes-approuter-user-api-service/).

To try this out for yourself, once you have this branch of the repo locally, follow these steps.

**Authenticate with Cloud Foundry**

```shell
cf login
```

**Create the xsuaa service instance**

```shell
cf create-service xsuaa application xsuaa-application -c xs-security.json
```

check created service instance:

```shell
cf service xsuaa-application
Showing info of service xsuaa-application in org 1111trial / space dev as <mail>...

name:            xsuaa-application
guid:            d8c37255-3eaa-461a-87bd-a46e02a23b9f
type:            managed
broker:          sm-xsuaa-333078da-dff3-4cf8-aef6-c089b0560467
offering:        xsuaa
plan:            application
tags:
offering tags:   xsuaa
description:     Manage application authorizations and trust to identity providers.
documentation:   <https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/6373bb7a96114d619bfdfdc6f505d1b9.html>
dashboard url:

Showing status of last operation:
   status:    create succeeded
   message:
   started:   2022-06-14T09:55:07Z
   updated:   2022-06-14T09:55:07Z

Showing bound apps:
   There are no bound apps for this service instance.

Showing sharing info:
   This service instance is not currently being shared.

   Service instance sharing is disabled for this service offering.

Showing upgrade status:
   Upgrades are not supported by this broker.
```

**Create and deploy the app**

```shell
cf push -f manifest.yml
```

**Open the URL in your browser to see the data**

To see what the User API Service returns for you, look for the URL in the route information for your app (it will be randomly generated). It will look something like this (the value for `routes`):

```
Waiting for app to start...

name:                userapitest
requested state:     started
isolation segment:   trial
routes:              userapitest-<random>.cfapps.<region>.hana.ondemand.com
stack:               cflinuxfs3
buildpacks:          nodejs
```

Open that as a URL in your browser. You should information shown similar to what was described in the blog post.

check xsuaa instance, note that application is bound:

```shell
type:            web
sidecars:
instances:       1/1
memory usage:    256M
start command:   npm start
     state     since                  cpu    memory   disk     details
# 0   running   2022-06-14T10:04:12Z   0.0%   0 of 0   0 of 0

cf service xsuaa-application
Showing info of service xsuaa-application in org 1111trial / space dev as mail...

name:            xsuaa-application
guid:            d8c37255-3eaa-461a-87bd-a46e02a23b9f
type:            managed
broker:          sm-xsuaa-333078da-dff3-4cf8-aef6-c089b0560467
offering:        xsuaa
plan:            application
tags:
offering tags:   xsuaa
description:     Manage application authorizations and trust to identity providers.
documentation:   <https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/6373bb7a96114d619bfdfdc6f505d1b9.html>
dashboard url:

Showing status of last operation:
   status:    create succeeded
   message:
   started:   2022-06-14T09:55:07Z
   updated:   2022-06-14T09:55:07Z

Showing bound apps:
   name          binding name   status             message
   userapitest                  create succeeded

Showing sharing info:
   This service instance is not currently being shared.

   Service instance sharing is disabled for this service offering.

Showing upgrade status:
   Upgrades are not supported by this broker.
```

---
**Increase logging level**

<https://www.npmjs.com/package/@sap/approuter/v/11.1.0#troubleshooting>

Approuter logging level

```shell
cf set-env userapitest XS_APP_LOG_LEVEL debug
```

Log incoming, outgoing requests

```shell
cf set-env userapitest REQUEST_TRACE debug
```

NodeJS minimal logging level

```shell
cf set-env userapitest CF_NODEJS_LOGGING_LEVEL debug
```

Packages using debug package

```shell
cf set-env userapitest DEBUG *
```

Node.js traces

```shell
cf set-env userapitest NODE_DEBUG debug
```

After changes restage the application

```shell
cf restage userapitest
```

Getting application log:

```shell
cf logs userapitest --recent
```

[More info about this repository](https://github.com/SAP-samples/sap-tech-bytes)
