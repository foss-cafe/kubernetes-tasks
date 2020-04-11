### Check certificate expiration

```bash
kubeadm alpha certs check-expiration
[check-expiration] Reading configuration from the cluster...
[check-expiration] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'

CERTIFICATE                EXPIRES                  RESIDUAL TIME   CERTIFICATE AUTHORITY   EXTERNALLY MANAGED
admin.conf                 Apr 11, 2021 15:26 UTC   364d                                    no
apiserver                  Apr 11, 2021 15:26 UTC   364d            ca                      no
apiserver-etcd-client      Apr 11, 2021 15:26 UTC   364d            etcd-ca                 no
apiserver-kubelet-client   Apr 11, 2021 15:26 UTC   364d            ca                      no
controller-manager.conf    Apr 11, 2021 15:26 UTC   364d                                    no
etcd-healthcheck-client    Apr 11, 2021 15:26 UTC   364d            etcd-ca                 no
etcd-peer                  Apr 11, 2021 15:26 UTC   364d            etcd-ca                 no
etcd-server                Apr 11, 2021 15:26 UTC   364d            etcd-ca                 no
front-proxy-client         Apr 11, 2021 15:26 UTC   364d            front-proxy-ca          no
scheduler.conf             Apr 11, 2021 15:26 UTC   364d                                    no

CERTIFICATE AUTHORITY   EXPIRES                  RESIDUAL TIME   EXTERNALLY MANAGED
ca                      Apr 09, 2030 15:26 UTC   9y              no
etcd-ca                 Apr 09, 2030 15:26 UTC   9y              no
front-proxy-ca          Apr 09, 2030 15:26 UTC   9y              no
```

Renew certs

```bash
sudo kubeadm alpha certs renew apiserver --use-api &
[1] 25213
root@ubuntu-bionic:~# Flag --use-api has been deprecated, certificate renewal from kubeadm using the Kubernetes API is deprecated and will be removed when 'certificates.k8s.io/v1' releases.
[renew] Reading configuration from the cluster...
[renew] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'

[certs] Certificate request "kubeadm-cert-kube-apiserver-j5c5q" created

root@ubuntu-bionic:~# kubectl get csr
NAME                                AGE     SIGNERNAME                                    REQUESTOR                   CONDITION
csr-r4jv7                           7m10s   kubernetes.io/kube-apiserver-client-kubelet   system:node:ubuntu-bionic   Approved,Issued
kubeadm-cert-kube-apiserver-j5c5q   8s      kubernetes.io/legacy-unknown                  kubernetes-admin            Pending
root@ubuntu-bionic:~# kubectl certificate approve kubeadm-cert-kube-apiserver-j5c5q
root@ubuntu-bionic:~# kubectl get csr
NAME                                AGE     SIGNERNAME                                    REQUESTOR                   CONDITION
csr-r4jv7                           7m36s   kubernetes.io/kube-apiserver-client-kubelet   system:node:ubuntu-bionic   Approved,Issued
kubeadm-cert-kube-apiserver-j5c5q   34s     kubernetes.io/legacy-unknown                  kubernetes-admin            Approved,Issued
```
