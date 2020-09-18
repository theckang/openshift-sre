# Site Reliability Engineering

The goal of this repo is to demonstrate aspects of Site Reliability Engineering using Kubernetes/OpenShift and Istio/OpenShift Service Mesh.

## Prequisites

* Admin access to OpenShift cluster

## Setup

Follow the [instructions](https://docs.openshift.com/container-platform/4.5/service_mesh/service_mesh_install/installing-ossm.html#ossm-operatorhub-install_installing-ossm) to deploy service mesh.  Use the default CRDs provided.

After you install the service mesh control plane, create a new project:

`oc create myproject`

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

```
GATEWAY_URL=$(oc get route istio-ingressgateway -n istio-system --template='http://{{.spec.host}}')
echo $GATEWAY_URL
```

## SLO Dashboards

TODO: Deploy SLO dashboards

Open Dashboards in Grafana:

```
echo $(oc get route grafana -n istio-system --template='https://{{.spec.host}}/dashboards')
```

Download the `dashboard/sample.json` file and import it to Grafana.

Navigate to the imported dashboard and you should see various SLO charts.

TODO: Explain each chart

Send sample requests:

`while true; do curl -s -o /dev/null $GATEWAY_URL; done`

## Failure Scenarios

TODO: Exact failure scenarios

