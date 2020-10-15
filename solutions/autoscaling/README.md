## Autoscaling

Don't remove the spec.replicas field.  You don't need to modify it.  `DeploymentConfig` without `spec.replicas` changes the replica to 0, and the HPA will not scale the application.  

Note: The behavior is different with a `Deployment`.  A `Deployment` without `spec.replicas` changes the replica count to 1, and the HPA will scale the application.  That drop can still negatively impact your application if your replicas are serving live traffic.
