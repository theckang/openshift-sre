## Use case: Cron job

1. Create new project
```
oc new-project cronjob
```

2. Create deployment of 10 replicas with autoscaling
```
oc apply -f hello-world.yaml
oc apply -f hpa.yaml
```

3. Create cron job
```
oc apply -f cronjob1.yaml
```

4. Wait 5 minutes.  Observe cron job overtakes the worker nodes.

5. Increase load on application.  Observe inability for application to scale and SLO is breached.

6. Fix: Misconfigured cron job.  `concurrencyPolicy` is in the wrong place in the spec.  Also use `activeDeadlineSeconds` to protect jobs from running indefinitely.
```
oc describe cronjob high-cpu-workload
oc delete -f cronjob1.yaml
oc apply -f cronjob2.yaml
```
