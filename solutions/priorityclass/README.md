## Priority Class

Once you deploy the medium workload, the workload will preempt the the application pod.

Observe the preemption event (this command requires oc client v4.4):
```
oc logs -l app=openshift-kube-scheduler -n openshift-kube-scheduler
```

The workload will preempt the application pod because the application pod does not have a priority class.  By default, workloads with any level of priority will take precedence over those that do not.

In order to fix this, the application pod should also be given a priority class.
