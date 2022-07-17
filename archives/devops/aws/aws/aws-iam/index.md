# AWS IAM


# IAM:Identity and Access Management

- **Global Service**로 리전 상관없이 설정함
- 사용자`Users`는 조직내 사용자이며 그룹화 할 수 있다.
- 그룹`Group`는 오직 사용자`users`만 포함할 수 있고 다른 그룹은 포함할 수 없다.
- 사용자는 그룹에 속하지 않을수도 있고 여러 그룹에 속할 수도 있다.

## Permissions
- AWS 서비스 및 리소스를 액세스하고 사용하도록 허용하기 위해서는 권한을 부여해야한다.
- 사용자 또는 그룹에 정책`policies`이라고 하는 JSON 문서를 지정하게 된다.
- 정책은 사용자의 권한`permissions`을 정의한다.
- AWS에서는 최소 권한 원칙(**least
privilege principle**)을 적용한다. 사용자에게 필요 이상의 권한을 부여하지 않는 것을 권장한다.

