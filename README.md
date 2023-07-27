This repository is meant for creating a deployment using a minimal ubi8 container to easily query the Kubernetes API for some type of information.

In this example, we are querying the API to get the number of worker nodes

Here is what happens with this code
1.  A service account called query-api is created
2.  A cluster role with specific APIgroups to run operations on (verbs).  This one only allows queries to machineconfigpools resource
3.  A role binding to link the query-api service account to this clusterrole
4.  A configmap which contains the script that will be run to do curl against Kubernetes API
5.  The deployment which runs once and will output the query to the log

This output can be used later
