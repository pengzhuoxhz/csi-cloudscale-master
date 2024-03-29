## unreleased

## v1.0.0 - 2019.02.05

* Add support for luks-encrypted volumes

* Add support for `bulk` volumes

* Read cloudscale.ch access token from environment variable

* Disable reserved blocks for privileged processes for `ext3` and `ext4` volumes

* Update to CSI spec v1.0.0

* Forked this repository from csi-digitalocean. They have a similar API. Thanks
  a lot to DigitalOcean - mostly Fatih Arslan - for his work.
  
### Important
 
This release contains breaking changes, because of the update to the CSI
spec v1.0.0 and the introduction of the `csi.cloudscale.ch/volume-type` parameter.
 
Your kubernetes version must be **at least v1.13.0** to support CSI spec v1.0.0.

If you have deployed previous versions of the CSI plugin, you cannot update the existing
`cloudscale-volume-ssd` storage class, because parameters of a storage class cannot be changed.
However, the default behaviour of the CSI plugin is to use volumes of type ssd unless specified
otherwise, so you can just leave the `cloudscale-volume-ssd` storage class as it is. 

Make sure you have the following [feature gates](https://kubernetes.io/docs/reference/command-line-tools-reference/feature-gates/) 
activated:

* `CSINodeInfo=true`
* `CSIDriverRegistry=true`

The following features are required but are GA in kubernetes v1.13.0. No explicit 
feature gate activation is needed:

* `CSIPersistentVolume=true`
* `MountPropagation=true`
* `KubeletPluginsWatcher=true`

## v0.2.0 - 2018.09.05

* Add support to CSI Spec `v0.3.0`. This includes many new changes, make sure 
  to read the Github PR for more information
  [[GH-72]](https://github.com/digitalocean/csi-digitalocean/pull/72)
* Check volume limits before provisioning calls
  [[GH-73]](https://github.com/digitalocean/csi-digitalocean/pull/73)
* Rename resource (DaemonSet, StatefulSet, containers, etc..) names and combine the
  attacher and provisioner into a single Statefulset.
  [[GH-74]](https://github.com/digitalocean/csi-digitalocean/pull/74)

**IMPORTANT**:This release contains breaking changes, mainly about how thing
are deployed. The minimum Kubernetes version needs to be now **v1.10.5**. 
To upgrade from a prior `v0.1.x` versions please remove the old CSI plugin
completely and re-install the new one:

```sh
# delete old version, i.e: v0.1.5
kubectl delete -f https://raw.githubusercontent.com/cloudscale-ch/csi-cloudscale/master/deploy/kubernetes/releases/csi-cloudscale-v0.1.5.yaml

# install v0.2.0
kubectl apply -f https://raw.githubusercontent.com/cloudscale-ch/csi-cloudscale/master/deploy/kubernetes/releases/csi-cloudscale-v0.2.0.yaml
```


## v0.1.5 - 2018.08.27

* Makefile improvements. Please check the GH link for more information.
  [[GH-66]](https://github.com/digitalocean/csi-digitalocean/pull/66)
* Validate volume capabilities during volume creation.
  [[GH-68]](https://github.com/digitalocean/csi-digitalocean/pull/68)
* Add version information to logs
  [[GH-65]](https://github.com/digitalocean/csi-digitalocean/pull/65)

## v0.1.4 - 2018.08.23

* Add logs to mount operations
  [[GH-55]](https://github.com/digitalocean/csi-digitalocean/pull/55)
* Remove description to allow users to reuse volumes that were created by the
  UI/API
  [[GH-59]](https://github.com/digitalocean/csi-digitalocean/pull/59)
* Handle edge cases from external action, such as Volume deletion via UI more
  gracefully. We're not very strict anymore in cases we don't need to be, but
  we're also returning a better error in cases we need to be.
  [[GH-60]](https://github.com/digitalocean/csi-digitalocean/pull/60)
* Fix attaching multiple volumes to a single pod
  [[GH-61]](https://github.com/digitalocean/csi-digitalocean/pull/61)

## v0.1.3 - 2018.08.03

* Fix passing an empty source to `IsMounted()` function during `NodeUnpublish`.
  This would prevent a pod to be deleted successfully in case of a dettached
  volume, because `NodeUnpublish` would never return success as `IsMounted()`
  was failing.
  [[GH-50]](https://github.com/digitalocean/csi-digitalocean/pull/50)

## v0.1.2 - 2018.08.02

* Check if mounts are propagated (`MountPropagation` is enabled on the host) in
  Node plugin to prevent silent failing. 
  [[GH-46]](https://github.com/digitalocean/csi-digitalocean/pull/46)
* Fix `IsMounted()` for bind mounts where it was returning false positives.
  [[GH-46]](https://github.com/digitalocean/csi-digitalocean/pull/46)
* Log 422 errors for visibility in Controller publish/unpublish methods.
  [[GH-38]](https://github.com/digitalocean/csi-digitalocean/pull/38)

## v0.1.1 (alpha) - 2018.05.29

* Fix panicking on errors for nil response objects
  [[GH-34]](https://github.com/digitalocean/csi-digitalocean/pull/34)

## v0.1.0 (alpha) - 2018.05.15

* Add method names to each log entry 
  [[GH-22]](https://github.com/digitalocean/csi-digitalocean/pull/22)
* Kubernetes deployment uses the `kube-system` namespace instead of the prior
  `default` namespace. Please make sure to delete and re-deploy the plugin.
  [[GH-21]](https://github.com/digitalocean/csi-digitalocean/pull/21)
* Change secret name from `dotoken` to `digitalocean`. Please make sure to
  update your keys (delete old secret and create new secret with name
  `digitalocean`). Checkout the README for instructions if needed.
  [[GH-21]](https://github.com/digitalocean/csi-digitalocean/pull/21)
* Make DigitalOcean API configurable via the new `--url` flag
  [[GH-27]](https://github.com/digitalocean/csi-digitalocean/pull/27)

## v0.0.1 (alpha) - 2018.05.10

* First release with all important methods of the CSI spec implemented
