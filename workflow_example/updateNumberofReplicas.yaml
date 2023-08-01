#! /bin/bash

# This script will update number of replicas based on return value of query-api pod

export KUBECONFIG=<pathtokubeconfig>

# Change to query-api project/namespace
oc project query-api
# Get Name of query-api pod
QUERYPOD=`oc get po|grep query-api|awk '{print $1}'`;
# Get Machine/Worker count from logs
WORKERNUMBER=`oc logs $QUERYPOD|grep \"machineCount|awk -F "," '{print $1}'|awk '{print $2}'`;
# Take Number of workers and update a deployment based off of this value
cat > deployment.yaml << EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  annotations:
    deployment.kubernetes.io/revision: "1"
  creationTimestamp: "2023-06-07T20:22:18Z"
  generation: 1
  name: dgedg2-config-listener
  namespace: testdg
  ownerReferences:
  - apiVersion: infinispan.org/v1
    blockOwnerDeletion: true
    controller: true
    kind: Infinispan
    name: dgedg2
    uid: a7c16ebf-1047-408e-a622-0bb91932782d
  resourceVersion: "257946411"
  uid: 3d84b1b8-09bd-434c-b4e0-602cd20ce1bc
spec:
  progressDeadlineSeconds: 600
  replicas: $WORKERNUMBER
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: infinispan-config-listener-pod
      clusterName: dgedg2
      infinispan_cr: dgedg2
      rht.comp: Data_Grid
      rht.comp_ver: 8.4.2
      rht.prod_name: Red_Hat_Runtimes
      rht.prod_ver: 2023-Q2
      rht.subcomp_t: application
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: infinispan-config-listener-pod
        clusterName: dgedg2
        infinispan_cr: dgedg2
        rht.comp: Data_Grid
        rht.comp_ver: 8.4.2
        rht.prod_name: Red_Hat_Runtimes
        rht.prod_ver: 2023-Q2
        rht.subcomp_t: application
    spec:
      containers:
      - args:
        - listener
        - -namespace
        - testdg
        - -cluster
        - dgedg2
        - -zap-log-level
        - info
        env:
        - name: INFINISPAN_OPERAND_VERSIONS
          value: '[{"upstream-version":"13.0.10","downstream-version":"8.3.1-1","image":"registry.redhat.io/datagrid/datagrid-8-rhel8@sha256:fef9789befa5796a9bec23023c9dfda27810846dc4406b126e0c344a656611a2","cve":false,"deprecated":false},{"upstream-version":"14.0.0","downstream-version":"8.4.0-1","image":"registry.redhat.io/datagrid/datagrid-8-rhel8@sha256:532dca6facb584c1261c0480c499af9f857c70dc390e0d46e46afec2561468af","cve":false,"deprecated":false},{"upstream-version":"14.0.0","downstream-version":"8.4.0-2","image":"registry.redhat.io/datagrid/datagrid-8-rhel8@sha256:bf7760520f5bea151a531d538d525c4591c47166995aefb4d77fef138de11544","cve":true,"deprecated":false},{"upstream-version":"14.0.6","downstream-version":"8.4.1-1","image":"registry.redhat.io/datagrid/datagrid-8-rhel8@sha256:ae852e4679978725bb7d24c1bdfe25b40bd883130ed04a36d4fca49dbea024ef","cve":false,"deprecated":false},{"upstream-version":"14.0.6","downstream-version":"8.4.1-2","image":"registry.redhat.io/datagrid/datagrid-8-rhel8@sha256:24cf79efa89bbfa495665b1711a27531b7ab0813695a89b2c38c96c2e792eb24","cve":true,"deprecated":false},{"upstream-version":"14.0.6","downstream-version":"8.4.1-3","image":"registry.redhat.io/datagrid/datagrid-8-rhel8@sha256:985193caf9bc3698c4d85aeedd1cfea09bccda881636d67885387b2a83c65ebd","cve":true,"deprecated":false},{"upstream-version":"14.0.8","downstream-version":"8.4.2-1","image":"registry.redhat.io/datagrid/datagrid-8-rhel8@sha256:9d9e466b7c367d0406406550c74bc790aa68f3f875ea4f652eb28f2e25a05458","cve":false,"deprecated":false}]'
        image: registry.redhat.io/datagrid/datagrid-8-rhel8-operator@sha256:c64f2b0b08f793698c1e1d2e41edbeb4bc0544d0ab25e986f4287f75c4bc5663
        imagePullPolicy: IfNotPresent
        name: infinispan-listener
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      serviceAccount: dgedg2-config-listener
      serviceAccountName: dgedg2-config-listener
      terminationGracePeriodSeconds: 30
EOF
# Apply Deployment yaml
oc apply -f deployment.yaml
