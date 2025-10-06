# Introduction-to-kubernetes
쿠버네티스 입문 책을 읽고 개념 및 실습내용 정리

> GCP 가상 컴퓨팅 사용
> OS: Ubuntu 22.04 LTS Minimal | 표준 영구 디스크

## RSA 세팅법
```bash
ssh-keygen -t rsa # 키 생성 후 엔터만 눌러서 기본값으로 생성
ls -al .ssh/ # 해당 명령으로 잘 생성되었나 확인
cat .ssh/id_rsa.pub # 공개키 내용 따로 복사해놓음
```
 - Compute Engine > 메타데이터 메뉴 > 왼쪽 위 SSH 키 탭 클릭 후 수정 클릭 > SSH 공개키 붙여넣기
```bash
cat .ssh/authorized_keys # instance-1(위의 명령어를 실행한 master node)외의 인스턴스에서 SSH 들어가서 확인
```

## Kubespray 설치법
```bash
ansible all -i inventory/mycluster/inventory.ini -m apt -a "name=iputils-ping state=present update_cache=yes" --become
ansible kube-master -i inventory/mycluster/inventory.ini -m file -a "path=/etc/kubernetes state=directory mode=0755" --become
ansible kube-master -i inventory/mycluster/inventory.ini -m shell -a "NERDCTL_VERSION=2.1.6; curl -L https://github.com/containerd/nerdctl/releases/download/v\$NERDCTL_VERSION/nerdctl-\$NERDCTL_VERSION-linux-amd64.tar.gz | sudo tar -C /usr/local/bin -xz && sudo chmod +x /usr/local/bin/nerdctl" --become
ansible-playbook -i inventory/mycluster/inventory.ini -v --become --become-user=root cluster.yml

#각 마스터 노드에 들어가서 설치
sudo apt install containerd -y
sudo apt-get install -y kubelet kubeadm kubectl
```
