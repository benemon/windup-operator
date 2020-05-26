# windup-operator

A Kubernetes Operator to deploy Windup (Red Hat Application Migration Toolkit) and its dependencies.

## Usage

* Create a namespace in which to deploy the Operator - `kubectl create namespace rhamt`
* Create the Windup CRD - `kubectl create -f deploy/crds/org.jboss.windup_windups_crd.yaml -n rhamt`
* Deploy the Windup Operator - `kubectl create -f deploy/ -n rhamt`
* Edit the CR as required, then create - `kubectl create -f deploy/crds/org.jboss.windup_v1_windup_cr.yaml -n rhamt`

## Example Windup Custom Resource

This is a deliberately opinionated installation of Windup, using the defaults defined in the standard OpenShift Template shipped with each distribution. A user can influence the resources available to the console and the executor, as well as the database name, and the Windup image version deployed.

The example Windup CR provided represents a minimal known good configuration, but you will want to edit the resources consumed by  Windup to balance your usage of Windup against the capacity of your cluster. The more CPU / Memory you can allocate to it, the faster the application assessments will run.

The Windup CR looks like this:

```yaml
  apiVersion: org.jboss.windup/v1
  kind: Windup
  metadata:
    name: example-windup
  spec:
    resources:
      storage:
        console: 5Gi
        database: 5Gi
      cpu:
        console:
          request: 500m
          limit: 1
        executor:
          request: 500m
          limit: 1
      memory:
        console:
          request: 1Gi
          limit: 2Gi
        executor:
          request: 1Gi
          limit: 2Gi
    database:
      name: rhamt
    images:
      externalNamespace: windup3
      tag: 4.3.1.Final
```