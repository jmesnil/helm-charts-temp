# CoolStore with JBoss EAP 7.4

The source code of the Jakarta EE application is at [https://github.com/deewhyweb/eap-coolstore-monolith/tree/ocp](https://github.com/deewhyweb/eap-coolstore-monolith/tree/ocp)

This application *must be installed in a `coolstore` namespace.

This application requires the installation of 3 Operators:

* EAP
* RH-SSO (which must target the `coolstore` namespace)
* AMQ Broker

To install the 3 operators, run the commands:

```
oc apply -f https://raw.githubusercontent.com/deewhyweb/eap-coolstore-monolith/ocp/openshift/operators/amq-broker-operator.yml
oc apply -f https://raw.githubusercontent.com/deewhyweb/eap-coolstore-monolith/ocp/openshift/operators/eap-operator.yml
oc apply -f https://raw.githubusercontent.com/deewhyweb/eap-coolstore-monolith/ocp/openshift/operators/sso-operator.yml
```

You can also install them using the OpenShift Web console (with the RH-SSO operator targeting only the `coolstore` namespace).

Once the Helm chart is installed, you need to specify the route that was automatically created by OpenShift for RH-SSO.
To do so, you must upgrade the Helm chart and replace the value of `keycloak.url` in the chart with the URL of the `keycloak` Route resource with the `/auth` endpoint.

This should be similar to:

```
keycloak:
  url: https://keycloak-coolstore.apps.65c7d3b08ce499ca2f7a.hypershift.aws-2.ci.openshift.org/auth
```
