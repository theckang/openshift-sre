## Priority Class

There are two issues with this scenario.

1.  The amount of CPU in use is not the same as the amount of CPU allocated to pods.
2.  A workload with priority can preempt a workload with no priority.

Instead of checking CPU via:
```bash
oc adm top node -l node-role.kubernetes.io/worker
```

We need to look at the total CPU requested:
```bash
oc describe node -l node-role.kubernetes.io/worker
```

Then, it is clear that requesting 75% of the node's capacity is not ok.

In addition, we did not intend for the medium daemonset to overtake the application pods.

Observe the preemption event (this command requires oc client v4.4):
```
oc logs -l app=openshift-kube-scheduler -n openshift-kube-scheduler
```

The daemonset will preempt the application pod because the application pod does not have a priority class.  By default, workloads with any level of priority will take precedence over those that do not.

In order to fix this, the application pod should also be given a priority class.
