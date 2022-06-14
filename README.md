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
