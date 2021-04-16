# argocd-helmchart
A repository to test ArgoCD with Helm

# Install ArgoCD

```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.0.0/manifests/install.yaml
```

## Get pwd

```shell
ARGO_PWD=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)
# Set in nodePort
kubectl patch svc argocd-server -n argocd -p '{"spec": { "type": "NodePort", "ports": [ { "nodePort": 31550, "port": 80, "protocol": "TCP", "targetPort": 8080 } ] } }'
argocd login --username admin --password $ARGO_PWD localhost:31550 --insecure
# argocd account update-password
```

# Configure a repository

Select the configuration menu and add a repositry in ssh

![Configure Repository](images/01-configure-repo.png)

# Create a project

Select Projects in the configuration menu and create a project

![Create a Project](images/02-create-project.png)

# Configure a project

- Set the name
- Set the repository
- Set the cluster

![Configure a Project](images/03-configure-project.png)

- Allow cluster resources and namespace resources

![Configure a Project](images/04-configure-project-2.png)

# Create a role for the project

Select the Roles tab.

Remember to create the role **without policies**, then update it with the policy

![Create a Role](images/05-configure-project-roles.png)

# Create an Application

From the Manage application menu create a new app

![Create an Application](images/06-create-application.png)

## Configure the Application

1. Give the application a name
2. Select a project
3. Always nice to have an automatic sync
4. Prune resources and self-heal to have the **exact** state of Git
5. Auto-create namespace if you need it

![Configure an Application](images/07-configure-application-1.png)

6. Set the repository url, branch and path to the chart

![Configure an Application](images/08-configure-application-2.png)

7. Select the cluster and the namespace
8. Choose the `values.yaml` file for the helm chart
9. Customize parameters

![Configure an Application](images/09-configure-application-3.png)

# Application Created

![Application Created](images/10-application-created.png)
