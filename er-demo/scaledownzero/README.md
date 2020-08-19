## ER Demo: Scale down zero

1. Create 5000 responders

2. Create incidents (5000 with 250 millisecond delay)

3. TODO: Observe SLO on mission assignment

4. Remove spec.replicas from `process-service` and create HPA
```
oc patch dc user1-process-service --type json -p='[{"op": "remove", "path": "/spec/replicas"}]'
oc apply -f user1-process-service-hpa.yaml
```

5. Observe error
```
watch oc get pods -l app=user1-process-service
```

7. Application SLO is breached indefinitely
