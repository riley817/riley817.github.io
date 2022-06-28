# Kubernetes context 관리


## Kubernetes 서비스 어카운트 인증정보 설정

kubectl 명령어를 사용해 크버네티스 클러스터를 제어할 때는 **kubeconfig**라고 하는 특수한 설정파일을 통해 인증을 진행한다. kubeconfig 파일에는 기본적으로 클러스터 관리자 권한을 가지는 인증서 정보가 저장되며 아무런 제한 없이 쿠버네티스를 사용할 수 있다.


