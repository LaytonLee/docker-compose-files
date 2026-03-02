# PhotoPrism with Podman

1. create secrets

use command `echo -n 'password' | base64` to encode your password, then add it to `secret.yaml`, run the following command to create podman secrets:

```bash
podman kube play secret.yaml
```

2. test pod

```bash
podman kube play --log-driver=k8s-file --replace photoprism.yaml
```

3. set systemd service

copy `photoprism.kube` to `~/.config/containers/systemd/`, run it as systemd service:

```bash
systemctl --user daemon-reload
systemctl --user start photoprism.service
```
