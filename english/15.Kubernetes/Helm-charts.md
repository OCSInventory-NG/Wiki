# OCS Invetory Helm Chart

This Helm chart is designed to facilitate the deployment and configuration of
OCS Inventory on a Kubernetes cluster.

## Add Helm repository

Launch the following commands to add the ocs inventory helm repo :

```bash
helm repo add ocsinventory https://ocsinventory-ng.github.io/helm-charts
helm repo update
```

you can retrieve the values.yaml with this command :

```bash
wget https://raw.githubusercontent.com/OCSInventory-NG/helm-charts/main/charts/ocsinventory/values.yaml
```

> Note : all parameters on values.yaml are explained in the "Values" section of the README

See [this link](https://github.com/OCSInventory-NG/helm-charts#values) for more informations

## Values configuration

### Version

In the first section, you can specify the version of OCS you wish to install in `image.tag`:

```yaml
image:
  repository: ocsinventory/ocsinventory-docker-image
  pullPolicy: IfNotPresent
  tag: "2.12.1"
```

### PHP Config

In the phpconfig section, you can add the PHP configurations you need. These
will be included in `/etc/php/8.1/apache2/conf.d/ocsinventory.ini`.

* Example with default PHP values for OCS:

```yaml
phpconfig:
  ocsinventory: |
    upload_max_filesize = 200M
    post_max_size = 201M
    max_execution_time = -1
    max_input_time = -1
```

See [php ini core documentation](https://www.php.net/manual/ini.core.php) for more information.


### Volumes

In the persistence section, you can configure the size, access mode, and
storageClass. The storageClass varies depending on how your Kubernetes cluster
is hosted. Finally, you can add annotations, and if the volume you wish to use
already exists, you can specify it with `persistence.existingClaim` and set
`persistence.enabled` to false.

* Example for OVH Cloud:

```yaml
persistence:
  enabled: true
  size: 10Gi
  accessMode: ReadWriteOnce
  storageClass: "csi-cinder-classic"
```

Kubernetes provide a full-featured list / documentation on [the following link](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

### Database

Here, you have three options. The first allows you to configure an existing
database. The next option connects to an existing database server to create the
database. The last option is to deploy the MariaDB server and database.

* Example for method 1 :

```yaml
externalDatabase:
  enabled: true
  hostname: "mariadb-svc.namespace.svc.cluster.local"
  username: "ocsuser"
  password: "ocspassword"
  database: "ocsdb"
```

* Example for method 2 :

```yaml
database:
  create: true
  jobName: "createdb"
  hostname: "mariadb-svc.namespace.svc.cluster.local"
  username: "ocsuser"
  password: "ocspassword"
  database: "ocsdb"
  root_password: "RootDbPassword"
```

* Example for method 3 :

```yaml
mariadb:
  enabled: true
  jobName: "createmariadbserver"
  auth:
    rootPassword: "RootDbPassword"
    database: "ocsdb"
    username: "ocsuser"
    password: "ocspassword"
```

### Ingress

Concerning ingress, you can configure one or more hosts in ingress.hosts, along with
annotations. Be sure to edit the cluster-issuer for the cert manager. You can
add IPs or ranges to a whitelist if you wish to be the only one accessing OCS.
Finally, you can enable basic authentication for OCS agents and the API.

* Example :

```yaml
ingress:
  enabled: true
  labels: {}
  hosts:
    - "ocs.domain.tld"
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
    nginx.ingress.kubernetes.io/proxy-body-size: 200M
    nginx.ingress.kubernetes.io/proxy-connect-timeout: 300s
    nginx.ingress.kubernetes.io/proxy-send-timeout: 300s
    nginx.ingress.kubernetes.io/proxy-read-timeout: 300s
    cert-manager.io/cluster-issuer: "letsencrypt-staging"
    nginx.ingress.kubernetes.io/app-root: /ocsreports/
  basicauth:
    enabled: true
    username: "agent-user"
    password: "agent-password"
    authRealm: "Authentication Required"
    paths:
      - "/ocsapi"
      - "/ocsinventory"
```

> Note: It is advised for the cert manager to start by testing it in staging
> and then upgrade the chart with the cert manager in production.

## PHP Session Sharing

If you are using multiple pods, while waiting for the integration of Redis, PHP
sessions can be shared by adding an additional volume in the
`templates/deployment.yaml` file:

```yaml
(...)
volumeMounts:
  (...)
  - mountPath: /var/lib/php/sessions
    name: {{ template "ocsinventory-fullname" . }}-data
    subPath: sessions
(...)
```

## Installing the Chart

Once the values have been configured, you can install ocs with this command:

```bash
cd OCSInventory-Helm-Chart
helm -n <namespace> install <release_name> ocsinventory/ocsinventory -f ./values.yaml --create-namespace
```

### Upgrading the Chart

If you've made changes to the values or want to update the pod to a more recent
version of OCS, here's the command:

```bash
helm -n <namespace> upgrade <release_name> ocsinventory/ocsinventory -f ./values.yaml
```

### Uninstalling the Chart

With this command, you can uninstall the helm release of ocs and next, delete
the namespace if only this release were in there.

```bash
helm uninstall -n <namespace> <release_name>
kubectl delete ns <namespace>
```
