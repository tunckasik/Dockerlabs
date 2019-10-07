# Installing K3s on Jetson Nano


```
 sudo cat /etc/os-release
[sudo] password for jetson:
NAME="Ubuntu"
VERSION="18.04.2 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.2 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
```

```
jetson@jetson-desktop:~$ sudo cat /boot/cmdline.txt
dwc_otg.lpm_enable=0 console=tty1 console=ttyAMA0,115200 root=/dev/mmcblk0p7 rootfstype=ext4 elevator=deadline rootwait fbcon=map:10 fbcon=font:ProFont6x11 logo.nologo
```

```
jetson@jetson-desktop:~$ sudo curl -sfL https://get.k3s.io | sh -
[INFO]  Finding latest release
[INFO]  Using v0.9.1 as release
[INFO]  Downloading hash https://github.com/rancher/k3s/releases/download/v0.9.1/sha256sum-arm64.txt
[INFO]  Downloading binary https://github.com/rancher/k3s/releases/download/v0.9.1/k3s-arm64
[INFO]  Verifying binary download
[INFO]  Installing k3s to /usr/local/bin/k3s
[INFO]  Creating /usr/local/bin/kubectl symlink to k3s
[INFO]  Creating /usr/local/bin/crictl symlink to k3s
[INFO]  Skipping /usr/local/bin/ctr symlink to k3s, command exists in PATH at /usr/bin/ctr
[INFO]  Creating killall script /usr/local/bin/k3s-killall.sh
[INFO]  Creating uninstall script /usr/local/bin/k3s-uninstall.sh
[INFO]  env: Creating environment file /etc/systemd/system/k3s.service.env
[INFO]  systemd: Creating service file /etc/systemd/system/k3s.service
[INFO]  systemd: Enabling k3s unit
Created symlink /etc/systemd/system/multi-user.target.wants/k3s.service → /etc/systemd/system/k3s.service.
[INFO]  systemd: Starting k3s
jetson@jetson-desktop:~$
```

```
jetson@jetson-desktop:~$ sudo k3s kubectl get node -o wide
NAME             STATUS   ROLES    AGE     VERSION         INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION   CONTAINER-RUNTIME
jetson-desktop   Ready    master   3m55s   v1.15.4-k3s.1   192.168.1.3   <none>        Ubuntu 18.04.2 LTS   4.9.140-tegra    containerd://1.2.8-k3s.1
jetson@jetson-desktop:~$
```

```
jetson@jetson-desktop:~$ sudo k3s kubectl get nodes
NAME             STATUS   ROLES    AGE   VERSION
jetson-desktop   Ready    master   71m   v1.15.4-k3s.1

```

```
jetson@jetson-desktop:~$ sudo k3s kubectl run mynginx --image=nginx --replicas=3 --port=80
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
Error from server (InternalError): Internal error occurred: resource quota evaluates timeout
jetson@jetson-desktop:~$ sudo k3s kubectl get po,svc,deploy
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   76m
Error from server (InternalError): an error on the server ("unknown") has prevented the request from succeeding (get pods)
Error from server (Timeout): the server was unable to return a response in the time allotted, but may still be processing the request (get deployments.extensions)
```
```
 sudo k3s kubectl cluster-info
[sudo] password for jetson:
Kubernetes master is running at https://127.0.0.1:6443
CoreDNS is running at https://127.0.0.1:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

