# nginx-ocp

OpenShift-compatible NGINX Helm chart containing:

- Deployment
- ClusterIP Service
- OpenShift Route with optional edge TLS
- HorizontalPodAutoscaler (`autoscaling/v2`)
- PersistentVolumeClaim

The default image is `nginxinc/nginx-unprivileged` and listens on port `8080`, avoiding privileged port usage. No fixed UID is configured, allowing OpenShift to assign an arbitrary UID.

## Install

```bash
oc new-project nginx-demo
helm upgrade --install nginx ./nginx-ocp -n nginx-demo
```

## Validate before installation

```bash
helm lint ./nginx-ocp
helm template nginx ./nginx-ocp -n nginx-demo
```

## Get the route

```bash
oc get route nginx-nginx-ocp -n nginx-demo
```

## Common overrides

```bash
helm upgrade --install nginx ./nginx-ocp -n nginx-demo \
  --set route.host=nginx.apps.example.com \
  --set persistence.storageClassName=ocs-storagecluster-ceph-rbd \
  --set persistence.size=5Gi \
  --set autoscaling.maxReplicas=10
```

For RWX-capable storage, override the access mode and use an RWX storage class:

```bash
helm upgrade --install nginx ./nginx-ocp -n nginx-demo \
  --set persistence.accessModes[0]=ReadWriteMany \
  --set persistence.storageClassName=<rwx-storage-class>
```

## Notes

- The init container seeds the PVC with the default NGINX page only when `index.html` is absent.
- CPU requests are configured because the HPA CPU utilization target is calculated relative to requested CPU.
- Set `autoscaling.enabled=false` to use `replicaCount` directly.
- Set `route.tls.enabled=false` for a non-TLS route.
