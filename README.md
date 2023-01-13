# Example package
This package contains examples of extra manifests you could apply via kapp-controller to a cluster.

## Examples
### Existing Package
The [config/fluent-bit/fluent-bit.yaml](config/fluent-bit/fluent-bit.yaml) file gives an example of installing another kapp-controller package from within this package.  This allows you to "nest" installs or effectively create a "package of packages" that can be installed via one package install.  In this case we're installing fluent-bit from the tanzu-standard registry that is automatically added when a cluster is provisioned by Tanzu Mission Control. 

### Helm Charts

### Embedded Manifests
The [config/cronjob](config/cronjob) director gives an example of installing a collection of manifests that are directly embedded in the package.  This case is a simple namespace creation (dev-cluster-addons-cronjob) and a CronJob that lives in that namespace that  runs once a minute.

## Publishing
```
$ kbld -f config/ --imgpkg-lock-output .imgpkg/images.yml
...
$ imgpkg push -b ghcr.io/cdelashmutt-pivotal/dev-cluster-addons.ti.com:0.0.1 -f .
```