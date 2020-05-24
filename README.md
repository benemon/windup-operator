# windup-operator

A Kubernetes Operator to deploy Windup (Red Hat Application Migration Toolkit) and its dependencies.

## Usage

* Create a namespace in which to deploy the Operator - `kubectl create namespace rhamt`
* Create the Windup CRD - `kubectl create -f deploy/crds/org.jboss.windup_windups_crd.yaml -n rhamt`
* Deploy the Windup Operator - `kubectl create -f deploy/ -n rhamt`
* Edit the CR as required, then create - `kubectl create -f deploy/crds/org.jboss.windup_v1_windup_cr.yaml -n rhamt`

## Example Windup Custom Resource

This is a deliberately opinionated installation of Windup, using the defaults defined in the standard OpenShift Template shipped with each distribution. A user can influence the resources available to the console and the executor, as well as the database name, and the Windup image version deployed.

The Windup CR looks like this:

```yaml
  apiVersion: org.jboss.windup/v1
  kind: Windup
  metadata:
    name: example-windup
  spec:
    resources:
      storage:
        console: 10Gi
        database: 10Gi
      cpu:
        console:
          request: 2
          limit: 4
        executor:
          request: 1
          limit: 2
      memory:
        console:
          request: 2Gi
          limit: 4Gi
        executor:
          request: 1Gi
          limit: 2Gi
    database:
      name: rhamt
    images:
      externalNamespace: windup3
      tag: 4.3.1.Final
```