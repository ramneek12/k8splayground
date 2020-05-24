Metrics server may fail to authenticate if kubelet is running with --anonymous-auth=false flag.
Passing --authentication-token-webhook=true and --authorization-mode=Webhook flags to kubelet can fix this.
kops config for kubelet:

```shell script
kubelet:
  anonymousAuth: false
  authenticationTokenWebhook: true
  authorizationMode: Webhook
```


This might break authorization for kubelet-api user if ClusterRoleBinding is not created with system:kubelet-api-admin. Which can be fixed by creating the ClusterRoleBinding:

```shell script
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: kubelet-api-admin
subjects:
- kind: User
  name: kubelet-api
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: system:kubelet-api-admin
  apiGroup: rbac.authorization.k8s.io

```