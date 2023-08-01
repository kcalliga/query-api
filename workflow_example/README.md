This directory will show an example on how to this query API pod to get number of worker nodes in cluster and automatically update the number of deployment replicas based on this value.

From the main query-api repo, apply the following in order (after creating appropriate namespace).  These examples use query-api for namespace/project

1. sa.yaml
2. cluster-role.yaml
3. role-binding.yaml
4. configmap.yaml
5. query-api-deployment.yaml

