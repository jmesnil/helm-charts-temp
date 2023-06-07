# JBoss EAP 7.4 With EAP Operator

This Helm chart packages an application with [JBoss EAP 7.4](https://www.redhat.com/en/technologies/jboss-middleware/application-platform) to be deployed on OpenShift with the EAP Operator.

The Jakarta EE application is a [simple "Hello World" application](https://github.com/jboss-eap-up-and-running/eap7-getting-started).

It uses the `JBoss EAP 7.4` Helm chart to build the applicaiton image from the source code using OpenShift Source-to-Image (S2I).

Once the application image is built, it is deployed with a `WildFlyServer` resource that is controlled by the EAP Operator.

## Prerequisites

**The EAP Operator must be installed** on the OpenShift Cluster by following [the instructions](https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.4/html/getting_started_with_jboss_eap_for_openshift_container_platform/eap-operator-for-automating-application-deployment-on-openshift_default#installing-eap-operator-using-webconsole_default).

## Helm Chart Structure

This examples use the `JBoss EAP 7.4` Helm chart to build the applicaiton image from the source code using OpenShift Source-to-Image (S2I). 
Its `Chart.yaml` declares a dependencies on the `eap74` Helm chart and uses it to build the application image:

```
eap74:
  build:
    uri: https://github.com/jboss-eap-up-and-running/eap7-getting-started
```

By default, the `eap74` would also deploy the application image in OpenShift. However this example uses the EAP Operator to deploy the application, so the `deploy` step of the `eap74` Helm chart is disabled:

```
eap74:
  deploy:
    # disable deployment by the Helm Chart as it will done with the EAP Operator
    enabled: false
```

The EAP Operator defines a `WildFlyServer` resource that declares how the EAP application is deployed:

```
apiVersion: wildfly.org/v1alpha1
kind: WildFlyServer
metadata:
  name: {{ .Release.Name}}
spec:
  # the application image is built by the Helm chart and is named
  # as the Helm release
  applicationImage: {{ .Release.Name}}:latest
  replicas: 1
````

This resource uses Helm templating to specify the name of the `applicationImage`.
The `eap74` Helm Chart creates an `ImageStreamTag` based on the name of the Helm release (`{{ .Release.Name}}:latest`). We use the same values for the `applicationImage` so that this `WildFlyServer` will pull the application image from the corresponding `ImageStreamTag`.


## Source

The source code of the Jakarta EE application is at [https://github.com/jboss-eap-up-and-running/eap7-getting-started](https://github.com/jboss-eap-up-and-running/eap7-getting-started).

The source code of the application Helm Chart is at [https://github.com/jmesnil/helm-charts/tree/main/charts/eap7-with-operator](https://github.com/jmesnil/helm-charts/tree/main/charts/eap7-with-operator)