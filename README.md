# Example package
This package contains examples of extra manifests you could apply via kapp-controller to a cluster.

## Examples
### Service Account for Nested Installs
The [config/package-basics](config/package-basics) directory contains the definition of a service account that will be used to install the nested Carvel PackageInstall and App wrapping a Helm Chart.  Take note of the annotations on the resources in this folder that mark them as part of a change-group called `serviceaccount` to kapp-controller.  This let's us write rules (via more annotations) on the PackageInstall and App objects nested in this package to make sure we don't delete the service account before we delete the PackageInstall and App objects successfully.  

Not following this procedure will likely get you into a situation where the PackageInstall and App objects won't fully delete since the service account they were using to do their installs no longer exists.  Not being able to delete those objects would also hang the delete of this package.

If you get into that situation for some reason, you can add the `noopDelete: true` value to the `spec` section for the nested PackageInstall and App objects, and then manually clean up anything those nested installs left behind before trying to install again.

### Existing Carvel Package
The [config/fluent-bit/fluent-bit.yaml](config/fluent-bit/fluent-bit.yaml) file gives an example of installing another kapp-controller package from within this package.  This allows you to "nest" installs or effectively create a "package of packages" that can be installed via one package install.  In this case we're installing fluent-bit from the tanzu-standard registry that is automatically added when a cluster is provisioned by Tanzu Mission Control. 

### Helm Charts
The [config/memcached/memcached-helm.yaml](config/memcached/memcached-helm.yaml) file gives an example of installing a Helm chart via kapp-controller.  This allows you to nest a packaged install that already uses Helm into this package, without having to try and reverse engineer it into a kapp-controller package directly.

### Simple Embedded Manifests
The [config/cronjob](config/cronjob) directory gives an example of installing a collection of manifests that are directly embedded in the package.  This case is a simple namespace creation (dev-cluster-addons-cronjob) and a CronJob that lives in that namespace that  runs once a minute.

Note, this install gets performed by the service account created for PackageInstall of this package (either created manually or via management tools like the `tanzu` CLI, or Tanzu Mission Control), and so it doesn't need to worry about ordering it's deletion, as that service account would be deleted after the PackageInstall for this package.

## Publishing
```
$ kbld -f config/ --imgpkg-lock-output .imgpkg/images.yml
...
$ imgpkg push -b ghcr.io/cdelashmutt-pivotal/dev-cluster-addons.ti.com:0.0.1 -f .
```