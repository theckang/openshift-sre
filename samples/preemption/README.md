## Use case: Preemption

1. Create new project
```
oc new-project preemption
```

2. Create deployment targeted at node (max out its CPU as much as possible)
```
oc apply -f my-app.yaml
```

3. Observe CPU usage
```
oc adm top node <node-ip>
```

4. Create medium priority class
```
oc apply -f medium-priority.yaml
```

5. Create medium priority pod targeted at the same node
```
oc apply -f medium-workload.yaml
```

6. Observe my-app pods are kicked out
watch oc get pods
```
oc get events
```

7. Observe the resource request is almost full.  Where did my-app pods go?
```
oc describe node <node-ip>
```

8. Observe the preemption event
```
oc logs -f <scheduler-pod> -n openshift-kube-scheduler
```
