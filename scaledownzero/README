## Use case: Scale down zero

1. Create new project
```
oc new-project scaledownzero
```

2. Create deployment of 10 replicas
```
oc apply -f hello-world.yaml
```

3. Increase load on application

4. Show need for autoscaling

5. Create HPA and modified deployment with spec.replicas removed
```
oc apply -f hpa1.yaml
```

6. Observe error (all pods are terminated).
```
watch oc get pods
```

7. Application SLO is breached until HPA brings the pod count back up to serve requests.

8. Fix: Don't remove the spec.replicas field.  You don't need to modify it.  Note: `deploymentConfig` without replicas sets the count to 0.  `deployment` without replicas sets the count to 1.
```
oc delete -f hpa1.yaml
oc apply -f hello-world.yaml
oc apply -f hpa2.yaml
```
