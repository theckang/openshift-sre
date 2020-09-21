# OpenShift Site Reliability Engineering

The goal of this repo is to demonstrate aspects of Site Reliability Engineering using Kubernetes/OpenShift and Istio/OpenShift Service Mesh.

## Prequisites

* Admin access to OpenShift cluster

## Setup

Follow the [instructions](https://docs.openshift.com/container-platform/4.5/service_mesh/service_mesh_install/installing-ossm.html#ossm-operatorhub-install_installing-ossm) to deploy service mesh.  Use the default CRDs provided.

After you install the service mesh control plane, create a new project:

```bash
oc create myproject
```

Add this project to the service mesh:

```bash
oc create -f - <<EOF
apiVersion: maistra.io/v1
kind: ServiceMeshMemberRoll
metadata:
  name: default
  namespace: istio-system
spec:
  members:
    - myproject
```

Download the repo to install the sample microservices application.  The original source for this application is [here](https://github.com/dudash/openshift-microservices).

```
git clone https://github.com/RedHatGov/service-mesh-workshop-code.git
cd service-mesh-workshop-code/deployment/workshop
```

Follow the instructions [here](https://github.com/RedHatGov/service-mesh-workshop-dashboard/blob/main/workshop/content/lab1.3_deploymsa.md) to deploy the sample application.  Use `istio-ingressgateway` for the `INGRESS_GATEWAY_NAME`.

Set the gateway URL:

```bash
GATEWAY_URL=$(oc get route istio-ingressgateway -n istio-system --template='http://{{.spec.host}}')
echo $GATEWAY_URL
```

## SLO Dashboards

Start by sending traffic to the app:

```bash
while true; do curl -s -o /dev/null $GATEWAY_URL; done
```

Open Grafana dashboards in the browser:

```bash
echo $(oc get route grafana -n istio-system --template='https://{{.spec.host}}/dashboards')
```

Download the `dashboard/sample.json` file and import it to Grafana.

Navigate to the imported dashboard and you should see various SLO charts.  In the top right, switch the time range to `Last 5 minutes`. 

![Dashboard](/dashboard/images/dashboard.png?raw=true)

The SLOs use two Service Level Indicators: availability (% of successful requests) and latency (# of seconds to process request).

SLO #1: 95% of requests are successful and return within 1 second (measured in 1 min interval)

SLO #2: 90% of requests are successful and return within 500 milliseconds (measured in 1 min interval)

The time interval is set to 1 minute for the purposes of demonstration.  In reality, this interval would be longer (e..g 30 days).

The corresponding Error Budget charts are generated for each SLO.

## Failure Scenarios

### Scale Down Zero

In this scenario, we are going to add autoscaling to the application.

```bash
oc apply -f scaledownzero/app-ui-autoscale.yaml
```

Navigate to Grafana.  Wait a minute and click the refresh icon in the top right.

![Refresh Icon](/dashboard/images/refresh.png?raw=true)

The SLO will be breached, and the error budget will be depleted.

![Failure](/dashboard/images/failure.png?raw=true)

What went wrong?  This is an exercise for you to find out :)

Identify:
* How to roll back this change to a previous state
* What is the root cause of the failure?
* How to fix the issue and add autoscaling successfully

Note: When you fix the issue and deploy autosclaing, it can take awhile for the horizontal pod autoscaler to pick up CPU metrics.  (I've seen up to eight failures before the metrics are successfully retrieved).

### Cron Job

TODO

### Preemption

TODO