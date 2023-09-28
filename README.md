# Tanzu GitOps Reference Implementation

Use this archive contains an opinionated approach to implementing GitOps workflows on Kubernetes clusters.

This reference implementation is pre-configured to install Tanzu Application Platform.

For detailed documentation, refer to [VMware Tanzu Application Platform Product Documentation](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.5/tap/install-gitops-intro.html).


===== Some Important Steps =====
$git clone https://github.com/ybhagwan/tap-gitops.git
$cd tap-gitops/clusters/auto-aks-dwyohslmvu-1

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

