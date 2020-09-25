## Health Checks

The readiness and liveness probes are misconfigured.

If you look at the events of the failing application pod you will see the `readinessProbe` and `livenessProbe` repeatedly fail.

Specifically, the `timeoutSeconds` is set to `1` by default, but the application takes longer to respond to the `/info` endpoint.

Edit the application deployment config
```bash
oc edit dc app-ui
```

And increase the `timeoutSeconds` value.
