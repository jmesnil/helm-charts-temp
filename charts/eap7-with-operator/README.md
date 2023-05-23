# JBoss EAP 7.4 With EAP Operator

This Helm chart packages an application with [JBoss EAP 7.4](https://www.redhat.com/en/technologies/jboss-middleware/application-platform) to be deployed on OpenShift with the EAP Operator.

The Jakarta EE application is a simple "Hello World" application.
It uses the `JBoss EAP 7.4` Helm chart to build the applicaiton image from the source code using OpenShift Source-to-Image (S2I).
Once the application image is built, it is deployed with a `WildFlyServer` resource that is controlled by the EAP Operator.

## Prerequisites

The EAP Operator must be installed on the OpenShift Cluster.

## Source

The source code of the Jakarta EE application is at [https://github.com/jboss-eap-up-and-running/eap7-getting-started](https://github.com/jboss-eap-up-and-running/eap7-getting-started).

The source code of the application Helm Chart is at [https://github.com/jmesnil/helm-charts/tree/main/charts/eap7-with-operator](https://github.com/jmesnil/helm-charts/tree/main/charts/eap7-with-operator)