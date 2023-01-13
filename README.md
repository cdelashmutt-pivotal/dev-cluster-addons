# Example package
This package contains examples of extra manifests you could apply via kapp-controller to a cluster.

## Examples
### Existing Package
The [config/contour.yaml](config/contour.yaml) file gives an example of installing another kapp-controller package from within this package.  This allows you to "nest" installs or effectively create a "package of packages" that can be installed via one package install.

### Helm Charts

### Embedded Manifests

## Publishing
```
$ kbld -f config/ --imgpkg-lock-output .imgpkg/images.yml
...
$ imgpkg push -b ghcr.io/cdelashmutt-pivotal/dev-cluster-addons.ti.com:0.0.1 -f .
```