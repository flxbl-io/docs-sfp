# Docker Images

sfp docker images are published from the flxbl-io Github packages registry at the link provided below

{% embed url="https://github.com/orgs/flxbl-io/packages" %}

One can utilize the flxbl-io sfp images by using the&#x20;

```
docker pull ghcr.io/flxbl-io/sfp:36.0.10-8119648554
```



To preview latest images for the docker image, visit the [release candidate page](https://github.com/dxatscale/sfpowerscripts/pkgs/container/sfpowerscripts-rc) and update your container image reference.\
\
For example:

```yaml
default:
   image: ghcr.io/flxbl-io/sfp-rc:<version-number>

or
   image: ghcr.io/flxbl-io/sfp-rc:<sha>
```
