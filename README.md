# Rapid Observability Systems Integration

## What's going on here?
ROSI is a framework to bootstrap an observability and response environment. A ROSI yaml file is created for each application which can specify SLOs, Synthetics, dependencies, and ROSI will align to a given team for response based on standardized tagging. Speaking strictly to the existing functionality, ROSI is terraform that consumes YAML for teams and applications. This terraform creates teams, folders, contact points, notification policies, synthetics, SLOs, & some standardized views in Grafana while coordinating alerts to matching OpsGenie teams.

### More specifically
Let's say I turn on Grafana Cloud & deploy the Grafana Agent to my K8s cluster. With ROSI configured, based on the `pod`, `deployment`, or `container_name` of a kube-mixin alert I can route to appropriate team. One set of alerts covering my entire configuration. This is done by auto-generating an extremely thorough notification policy. This policy can also include custom dependencies. For example, my service depends on RDS and I want alerts for a given `arn` or `application` tag to route alongside my kube-mixin alerts. This example would depend on ingesting AWS metrics with YACE.

To take it a step further, if I implement OTEL on my service and align labels it becomes very easy to create high reproducible dashboards and alerts as well as SLOs. I can add SLOs and Synthetics to my ROSI file and these can be viewed and aggregated with the same labeling standards. This means monitoring-as-code for most of the configuration in Grafana.

#### Amazing, what can't it do?
ROSI isn't currently intended to force users to create all of their alerts and dashboards via terraform. ROSI configures folders for teams, giving them write access to their folder & read to all. While alerts and dashboards can be created with terraform, the user experience really calls for the UI. That's not to say alerts & dashboards couldn't be terraformed outside of ROSI.

## What's the general flow?
The move to OSS calls for a bit more flexibility, this is WIP. Support for additional vendors will come in the form of adjacent terraform modules.

The desired end workflow looks something like this:

### 1. Init
Initialize ROSI in a given repository using the upcoming ROSI-cli. In the end you will have set of directories holding yaml files that define your teams & applications. 

### 2. Define your ROSI files
Lots to explain here, but don't worry, it's just YAML. We will have GHAs and other CI tools to help keep things in shape.

### 3. Implement your response stack
Starting with support for OpsGenie, implementing `terraform-rosi-respond-opsgenie` will create matching teams and coordinate integration.

### 4. Implement terraform module for monitoring stack
Starting with support for Grafana, implementing `terraform-rosi-grafana` will boot strap Grafana Cloud from your ROSI files. This will need outputs from step 3.
