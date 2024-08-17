---
title: "🧸 Kubernetes 내부 인증서 갱신하기 (Unable to connect the server: x509: certificate has expired)"
date: 2024-08-19
categories:
  - misc
tags: 
render_with_liquid: true
mermaid: true
---
#### 쿠버네티스의 child node 가 restart 되었는데 자동 연결이 안된다
----
최근에 직접 할당받은 virtual machine 에 구축했던 kubernetes 의 child node 로 사용하던 친구가 운영하던 측에서 restart 해버렸다. (사실 거기에는 elastic search 도 띄워져 있던 곳이었는데 다행스럽게도 elastic search 는 cluster 로 구성이 되어 있어서 다행스럽게도 운영환경에는 문제가 없었다. 그냥 restart 만 함!)

원래였다면 restart 되었다면 다시 master node 가 알아서 연결을 해줘야 겠지만 이번에 신기하게 다시 연결이 되지 않았다... 

Stage 에서 사용하던 하나의 서비스가 (code runner 서비스) 오직 restart 된 node 에서만 띄워지도록 설정해 놨기에 운영에 영향은 없지만 손놓고 있을순 없었다.

#### kubernetes master node 에 갔더니 certificate 에러가!
---
마스터 노드에 들어가서 `kubectl get pods` 명령어를 치자 이런 에러가 나왔다.
```bash
Unable to connect the server: x509: certificate has expired....
```

쿠버네티스에서 내부적으로 사용하는 인증서가 있었고 그것이 만료가 되었다... 기가막히게 10일 전에 만료 되었는데 기가막히게 갑자기 vm 운영자가 restart 해버리다니! 오히려 운이 좋았다고 생각했다!

#### 인증서 문제를 한번 고쳐보자!
---
우선 인증서가 확실히 만료되었는지는 다음 명령어를 통해서 알수 있다

```bash
kubeadm certs check-expiration
```

이제 새롭게 인증서를 발급해보자 방법은 정말 하나도 어렵지 않았다. 아래의 명령어로 인증서를 renew 한다
```bash
kubeadm certs renew all
```
renew 를 하고 나면 kube-apiserver, kube-controller-manager, kube-scheduler 를 restart 하라고 나오는데 그건 아래에서 진행하겠다!

그보다 우선 `ls -al /etc/kubernetes/admin.conf` 를 해보면 오늘 날짜로 새롭게 변경되었음을 확인 할 수 있다. 이제 그 admin.conf 를 `kubectl` 명령어를 사용 할 수 있도록 `~/.kube/config` 로 복사해준다

```bash
cp /etc/kubernetes/admin.conf ~/.kube/config
```

이제 아까 말한 것들을 restart 해준다

```bash
kill -s SIGHUP ($pidof kube-apiserver)
kill -s SIGHUP ($pidof kube-controller-manager)
kill -s SIGHUP ($pidof kube-scheduler)
systemctl restart kubelet
systemctl restart daeon-reload
```

다시 아래의 명령어를 통해 certificate expiration date 를 확인해보면 갱신이 되었음을 확인 할 수 있다!

```bash
kubeadm certs check-expiration
```

휴! 인증서의 기간은 1년이라서 잘 정리해 두고 1년뒤에 다시! 갱신해야지!