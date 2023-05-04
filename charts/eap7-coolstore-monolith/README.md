# CoolStore with JBoss EAP 7.4

The source code of the Jakarta EE application is at [https://github.com/deewhyweb/eap-coolstore-monolith/tree/ocp](https://github.com/deewhyweb/eap-coolstore-monolith/tree/ocp)

## Prerequisites

This application requires the installation of 3 Operators:

* EAP
* RH-SSO (which must target the current namespace)
* AMQ Broker

## After the installation

Once the Helm chart is installed, you need to specify the route that was automatically created by OpenShift for RH-SSO.
To do so, you must upgrade the Helm chart and replace the value of `keycloak.url` in the chart's values with the URL of the `keycloak` Route resource with the `/auth` endpoint.

This should be similar to:

```
keycloak:
  url: https://keycloak-coolstore.apps.65c7d3b08ce499ca2f7a.hypershift.aws-2.ci.openshift.org/auth
```