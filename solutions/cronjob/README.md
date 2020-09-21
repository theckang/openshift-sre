## Cron Job

The cron job is configured.  `concurrencyPolicy` is in the wrong place in the spec.  

If you look at the `CronJob` closely:
```bash
oc describe cronjob high-cpu-workload
```

You will see the `concurrencyPolicy` was set to `Allow` when we actually wanted `Forbid`.  This is tricky, since the field was in the wrong place but it was silently ignored, causing the error.

Additionally, you should add `activeDeadlineSeconds` to protect jobs from running indefinitely.  In this case, the simulated workload runs indefinitely, hogging cpu resources.
