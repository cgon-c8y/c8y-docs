---
weight: 10
title: Quickstart
layout: bundle
sector:
  - edge_server
---

This section helps you to quickly install Edge on a [Lightweight Kubernetes (K3s)](https://docs.k3s.io/installation) cluster with default options. For detailed instructions, see [Installing Edge on Kubernetes](/edge-kubernetes/installing-edge-on-k8/).

1. Verify that your hardware meets the requirements specified in [Prerequisites](/edge-kubernetes/installing-edge-on-k8/#prerequisites).

2. Run the command below to install K3s.

   ```shell
   USER_NAME=$(whoami)
   sudo sh -c '
   echo "vm.panic_on_oom=0\nvm.overcommit_memory=1\nkernel.panic=10\nkernel.panic_on_oops=1" >> /etc/sysctl.d/90-kubelet.conf && \
   sysctl -p /etc/sysctl.d/90-kubelet.conf && \
   curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=v1.25.13+k3s1 sh -s - \
      --disable=traefik \
      --write-kubeconfig-mode 644 \
      --protect-kernel-defaults true  \
      --kube-apiserver-arg=admission-control=ValidatingAdmissionWebhook,MutatingAdmissionWebhook && \
   mkdir -p '"$HOME"'/.kube && \
   cp /etc/rancher/k3s/k3s.yaml '"$HOME"'/.kube/config && \
   chown '"$USER_NAME:"' '"$HOME"'/.kube/config && \
   chmod 600 '"$HOME"'/.kube/config && \
   echo -e "\e[32mSuccessfully installed k3s!\e[0m"
   '
   ```

3. Run the command below to install Helm v3.

   ```shell
   curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
   ```

4. Run the command below to install the Edge Operator and provide the repository credentials when prompted.

   ```shell
   curl -sfL {{< link-c8y-doc-baseurl >}}files/edge-k8s/c8yedge-operator-install.sh -O && bash ./c8yedge-operator-install.sh
   ```

5. Run the command below to apply Edge CR ([c8yedge-sample.yaml](/files/edge-k8s/c8yedge-sample.yaml)) for installing Edge version **{{< c8y-edge-current-version >}}.0.0** named **c8yedge** with the domain **myown.iot.com**.

   ```shell
   kubectl apply -f {{< link-c8y-doc-baseurl >}}files/edge-k8s/c8yedge-sample.yaml
   ```

6. See [Verifying the Edge installation](/edge-kubernetes/installing-edge-on-k8/#verifying-the-edge-installation) and [Accessing Edge](/edge-kubernetes/installing-edge-on-k8/#accessing-edge) to sign into Edge.
