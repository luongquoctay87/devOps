
[Helm Project Journey Report](https://www.cncf.io/reports/helm-project-journey-report/)
Helm is an open source package manager for Kubernetes. It provides the ability to provide, share, and use software built for Kubernetes.

# Using Helm

#### 1. Three Big Concepts
- __A Chart__ is a Helm package. It contains all of the resource definitions necessary to run an application, tool, or service inside of a Kubernetes cluster.
- __A Repository__ is the place where charts can be collected and shared
- __A Release__ is an instance of a chart running in a Kubernetes cluster. One chart can often be installed many times into the same cluster. And each time it is installed, a new release is created.


#### 2. `helm search`: Finding Charts
- `helm search hub` searches the [Artifact Hub](https://artifacthub.io/packages/helm/bitnami/wordpress), which lists helm charts from dozens of different repositories.
    ```
    helm search hub <Artifact Hub>
    helm search hub wordpress
    ```
- `helm search repo` searches the repositories that you have added to your local helm client (with `helm repo add`). This search is done over local data, and no public network connection is needed.

- If you want Helm to generate a name for you, leave off the release name and use `--generate-name`
- To keep track of a release's state, or to re-read configuration information, you can use `helm status`

- Helm installs resources in the following order:
    - Namespace
    - NetworkPolicy
    - ResourceQuota
    - LimitRange
    - PodSecurityPolicy
    - PodDisruptionBudget
    - ServiceAccount
    - Secret
    - SecretList
    - ConfigMap
    - StorageClass
    - PersistentVolume
    - PersistentVolumeClaim
    - CustomResourceDefinition
    - ClusterRole
    - ClusterRoleList
    - ClusterRoleBinding
    - ClusterRoleBindingList
    - Role
    - RoleList
    - RoleBinding
    - RoleBindingList
    - Service
    - DaemonSet
    - Pod
    - ReplicationController
    - ReplicaSet
    - Deployment
    - HorizontalPodAutoscaler
    - StatefulSet
    - Job
    - CronJob
    - Ingress
    - APIService


#### 3. Customizing the Chart Before Installing
You can then override any of these settings in a YAML formatted file, and then pass that file during installation.

There are two ways to pass configuration data during install:
```
--values (or -f): Specify a YAML file with overrides. This can be specified multiple times and the rightmost file will take precedence
--set: Specify overrides on the command line.
```
If both are used, `--set` values are merged into `--values` with higher precedence. Overrides specified with `--set` are persisted in a ConfigMap. Values that have been `--set` can be viewed for a given release with helm get values <release-name>. Values that have been `--set` can be cleared by running helm upgrade with `--reset-values` specified.

__The Format and Limitations of --set__
```
--set name=value
name: value
```

```
--set a=b,c=d
a: b
c: d
```

```
--set outer.inner=value
outer:
  inner: value
```

```
--set name={a, b, c}
name:
  - a
  - b
  - c
```

```
--set name=[],a=null
name: []
a: null
```

```
--set servers[0].port=80
servers:
  - port: 80
```

```
--set servers[0].port=80,servers[0].host=example
servers:
  - port: 80
    host: example
```

```
--set name=value1\,value2
```

```
--set nodeSelector."kubernetes\.io/role"=master
nodeSelector:
  kubernetes.io/role: master
```

#### 2. `helm upgrade` and `helm rollback`: Upgrading a Release, and Recovering on Failure
- When a new version of a chart is released, or when you want to change the configuration of your release, you can use the `helm upgrade` command.

- We can use `helm get values` to see whether that new setting took effect.

- If something does not go as planned during a release, it is easy to roll back to a previous release using `helm rollback [RELEASE] [REVISION]`.
    ```
    helm rollback happy-panda 1
    ```

#### 3. Helpful Options for Install/Upgrade/Rollback
`--timeout`: A Go duration value to wait for Kubernetes commands to complete. This defaults to 5m0s.

`--wait`: Waits until all Pods are in a ready state, PVCs are bound, Deployments have minimum (Desired minus maxUnavailable) Pods in ready state and Services have an IP address (and Ingress if a LoadBalancer) before marking the release as successful. It will wait for as long as the `--timeout` value. If timeout is reached, the release will be marked as FAILED. Note: In scenarios where Deployment has replicas set to 1 and maxUnavailable is not set to 0 as part of rolling update strategy, `--wait` will return as ready as it has satisfied the minimum Pod in ready condition.

`--no-hooks:` This skips running hooks for the command

`--recreate-pods` (only available for upgrade and rollback): This flag will cause all pods to be recreated (with the exception of pods belonging to deployments). (DEPRECATED in Helm 3)


#### 4. `helm uninstall`: Uninstalling a Release

- Remove the release from the cluster.
    ```
    helm uninstall <name>
    ```

- See all of your currently deployed releases with the `helm list` command
    ```
    helm list
    ```

- In Helm 3, deletion removes the release record as well. If you wish to keep a deletion release record, use `helm uninstall --keep-history`. Using `helm list --uninstalled` will only show releases that were uninstalled with the `--keep-history` flag.

- The `helm list --all` flag will show you all release records that Helm has retained, including records for failed or deleted items (if `--keep-history` was specified)

#### 5. `helm repo`: Working with Repositories

- You can see which repositories are configured using `helm repo list`
- And new repositories can be added with `helm repo add`
    ```
    helm repo add dev https://example.com/dev-charts
    ```
- Because chart repositories change frequently, at any point you can make sure your Helm client is up to date by running `helm repo update`
- Repositories can be removed with `helm repo remove
`

#### 6. Creating Your Own Charts
- Create helm chart by command
    ```
    helm create <chart-name>
    ```
- You can validate that it is well-formed by running `helm lint`

- When it's time to package the chart up for distribution, you can run the helm package command
    ```
    helm package <chart-name>
    ~/first-chart-0.1.0.tgz
    ```
- Chart can now easily be installed by `helm install`
    ```
    helm install <chart-name> <helm-chart>
    ```
