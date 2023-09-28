# Tanzu GitOps Reference Implementation

Use this archive contains an opinionated approach to implementing GitOps workflows on Kubernetes clusters.

This reference implementation is pre-configured to install Tanzu Application Platform.

For detailed documentation, refer to [VMware Tanzu Application Platform Product Documentation](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.5/tap/install-gitops-intro.html).


===== Some Important Steps =====
git clone https://github.com/ybhagwan/tap-gitops.git
cd tap-gitops/clusters/auto-aks-dwyohslmvu-1

1. export VAULT_ADDR=MY-VAULT-ADDR    (Ex : export VAULT_ADDR="http://3.16.157.28:8200/")
2. export CLUSTER_NAME=CLUSTER-NAME   (Ex : export CLUSTER_NAME="Ybhagwan-Cluster)
3. vault login using token or $vault login -method=userpass username="username" password="passowrd"
4. vault auth list
5. tanzu-sync/scripts/setup/create-kubernetes-auth.sh
Success! Enabled kubernetes auth method at: Ybhagwan-Cluster/
Success! Data written to: auth/Ybhagwan-Cluster/config
6. vault kv put secret/foo1 bar=bar
7. vault auth disable $CLUSTER_NAME
Success! Disabled the auth method (if it existed) at: Ybhagwan-Cluster/
8. tanzu-sync/scripts/setup/create-kubernetes-auth.sh
Success! Enabled kubernetes auth method at: Ybhagwan-Cluster/
Success! Data written to: auth/Ybhagwan-Cluster/config
10. vault kv put secret/foo2 bar=bar
11. tanzu-sync/scripts/setup/create-policies.sh
Policy name was converted from "Ybhagwan-Cluster--read-tanzu-sync-secrets" to "ybhagwan-cluster--read-tanzu-sync-secrets"
Success! Uploaded policy: ybhagwan-cluster--read-tanzu-sync-secrets
Policy name was converted from "Ybhagwan-Cluster--read-tap-secrets" to "ybhagwan-cluster--read-tap-secrets"
Success! Uploaded policy: ybhagwan-cluster--read-tap-secrets
11. tanzu-sync/scripts/setup/create-roles.sh
Success! Data written to: auth/Ybhagwan-Cluster/role/Ybhagwan-Cluster--tanzu-sync-secrets
Success! Data written to: auth/Ybhagwan-Cluster/role/Ybhagwan-Cluster--tap-install-secrets
12. printf '%s\n'  "$(cat <<EOF
{
  "shared": {
    "image_registry": {
      "project_path": "tapscale.azurecr.io/ipillaigitops/",
      "username": "tapscale",
      "password": "dd5f6appflmdT6NAa3kK+pOnekVt0OKtin2xOTmtD6+ACRBG5C1E"
    }
  }
}
EOF
)" | vault kv put secret/dev/${CLUSTER_NAME}/tap/sensitive-values.yaml -
======================= Secret Path =======================
secret/data/dev/Ybhagwan-Cluster/tap/sensitive-values.yaml

======= Metadata =======
Key                Value
---                -----
created_time       2023-09-28T07:08:55.451099933Z
custom_metadata    <nil>
deletion_time      n/a
destroyed          false
version            1

Note : Before running step-12 make sure  client's policies grant access enabled
