# Example package
This package contains examples of extra manifests you could apply via kapp-controller to a cluster

## Publishing
```
$ kbld -f config/ --imgpkg-lock-output .imgpkg/images.yml
...
$ imgpkg push -b ghcr.io/cdelashmutt-pivotal/dev-cluster-addons.ti.com:0.0.1 -f .
```