# 개발협업 가이드 Sample


# 개발 협업 가이드

우리 팀에서 내부 및 외부 공동 작업자가 개발 협업하기 위한 방법으로 **애자일의 Scrum 방법론**을 사용합니다. Scrum 관리를 위한 도구로는 **GitLab의 이슈 시스템과 칸반보드 기능**을 활용합니다. 

GitLab에 등록되는 `이슈(Task)`는 시스템 개발과 관련된 작업만 작성하도록 합니다. 프로젝트 계약 등 개발작업이 아닌 작업의 경우 **Microsoft Planner의 칸반보드**를 사용합니다.

> PBTeam – Microsoft Planner 사용가이드

GitLab은 Git 원격 저장소 관리, CI/CD, 이슈 관리, 테스트 등 소프트웨어 개발과 운영의 전반적인 라이프사이클을 관리할 수 있는 통합 툴입니다. Git과 GitLab에 익숙하지 않다면 아래 문서를 참고하여 숙지하시기 바랍니다.

> - [Git 간편 안내서](https://rogerdudler.github.io/git-guide/index.ko.html)
> - [GitLab Documentation – GitLab basics](https://docs.gitlab.com/ee/user/index.html)

## 우리 팀이 Scrum을 진행하는 순서

1. `PO(Product Owner)`는 제품의 요구 기능(`User Story`)을 바탕으로 큰 틀 작업(`Epic`)을 정의합니다.
2. `PO`는 `Epic`의 우선순위와 제품 개발 범위를 적용하여 `Product Blacklog`를 작성하고 팀원과 조율합니다.
3. 개발 팀원은 스프린트 목표를 세우고 `Product Blocklog`를 `linked issue`하는 `Sprint Backlog`를 작성합니다. **우리 팀의 스프린트는 2주 단위로 진행합니다.** 스크럼 마스터는 전반적인 스크럼 관리를 담당하게 됩니다.
4. 매회의 스프린트가 종료할 때마다 스프린트 리뷰를 통해 만들어진 제품을 검토하고 개선사항을 토론하는 스프린트 리뷰를 갖습니다. 
5. 스프린트 리뷰가 끝나면 스프린트 회고를 통해 팀의 개발문화에 대한 개선 시간을 갖습니다.
6. 다음 스프린트에서 수행할`Backlog`를 `PO`와 필요 인원이 모여 선정하고 계획합니다.

## GitLab을 이슈를 통한 Scrum 관리 방법

GitLab의 이슈 시스템과 칸반보드로 Scrum을 관리합니다. 이슈를 통하여 아이디어 제안, 문제 해결, 작업 계획을 합니다. 작업자가 이슈를 제안하고 토론할 수 있습니다. 등록하는 사례는 아래와 같습니다.
 
- 새로운 아이디어 구현 논의
- Task 및 상태 추적
- 기능 제안, 질문, 지원 요청 또는 버그 리포트
- 코드 구현 설명

{{< figure src="/categories/images/TIL/guide1.png#center" caption="[예시] Epic 유형의 이슈" >}}

이슈를 생성할 때 이슈 유형을 반드시 라벨로 추가해야 합니다. 이슈의 Description 항목에서 이슈 유형에 따른 템플릿을 선택할 수 있습니다. 이슈 유형은 다음과 같습니다.

#### Issue(Task) 유형

| Issue Type |  정의 |
|------------|---|
|`Epic(에픽)`|작업의 큰 틀. 여러 작업의 모음|
|`Task (작업)`|작업, 업무, 해야할 일|
|`Change (변경)`|기존 기능의 변경|
|`Improvement (개선)`|기존 기능의 성능, 품질 개선|
|`New Feature (새 기능)`|새로운 아이디어 기능 제안|
|`Document (문서 작업)`|문서화 작업|
|`Incident (장애)`|장애 처리|
|`Problem (문제)`|결과값에 문제는 없지만 문제가 되는 상황|
|`Bug(버그)`|기존 기능의 오류 보고|

#### Issue(Task) 생성시 필수 입력 사항
- Milestone (Sprint backlog task)
- Label
- Assignee

#### Linked issue
하나의 Task를 처리하는 과정에서 추가 Task가 발생하는 경우 또는 Task의 작업 규모가 생각했던 것 보다 커지는 경우 보다 작은 규모의 Task를 나누어져야 할 필요가 종종 발생합니다. 이런 경우 새로운 Task를 생성하고 작업중이던 Task를 linked issue로 설정합니다.

## Git-Workflow
이슈는 Workflow에 따라 특정 status 값을 관리합니다. 전체적인 Workflow는 아래와 같습니다.

{{< figure src="/categories/images/TIL/guide2.png#center" caption="" >}}

| Issue Workflow status  |  정의 |
|------------|---|
|**Product Backlog**|해야 할 과제.`PO` 관점에서 `고객 요구사항(User Story)`을 상세히 기술. 최초 작성은 `PO`가 진행
|**Sprint Backlog(To Do)**|`Product Backlog`의 `Task` 중 현재 스프린트 동안 구현하기로 결정 된 개발 작업 단위의 `Task`. <br/> `Sprint Backlog`에는 1day~3day 내에 처리할 수 있는 `sub task`를 담당자가 작성 `Task`가 3day 초과 시 새로운 `task`를 생성하고 작업 중인 `task`를 `linked issue`로 설정
|**Doing**|진행 중(배포 대기중 포함)|
|**Done**|과제가 끝남. 최종 확인자 확인 대기|
|**Confirmed**|최종 확인자 확인 완료|
|**Cancel**|Task 취소|
|**Reject**|최종 확인자 반려|

### Issue Workflow status에 따른 칸반보드
{{< figure src="/categories/images/TIL/guide3.png#center" caption="" >}}

## 브랜치 규칙
우리 팀은 Git-flow 브랜치 전략을 사용합니다. 브랜치는 5가지 종류의 브랜치를 사용합니다.

- **master** : 제품으로 출시될 수 있는 브랜치
- **develop** : 다음 출시 버전을 개발하는 브랜치
- **feature** : 기능을 개발하는 브랜치
- **release** : 이번 출시 버전을 준비하는 브랜치
- **hotfix** : 출시 버전에서 발생한 버그를 수정 하는 브랜치

## 작업 순서
#### 1. Bakclog이슈에 해당하는 feature 브랜치를 생성합니다.
 
`feature/issue-<이슈번호>` 형식으로 브랜치를 생성합니다. `feature` 브랜치는 `develop` 브랜치에서부터 시작합니다. git 명령어를 이용하거나 **GitLab이슈 수정 > Create Branch** 버튼으로도 생성할 수 있습니다.
{{< figure src="/categories/images/TIL/guide4.png#center" caption="" >}}

#### 2. 작업 브랜치에서 소스 코드 수정 후 변경사항을 커밋합니다.

#### 3. 작업 브랜치를 origin에 푸시합니다.

#### 4. GtiLab에 작업 브랜치를develop 브랜치로 merge하는 Pull Request를 생성합니다.
작업 브랜치 개발이 끝나면 작업브랜치의 `Merge Request`를 작성합니다. 타겟 브랜치는 `develop`을 선택합니다.
{{< figure src="/categories/images/TIL/guide5.png#center" caption="" >}}

`Documentation` 이슈 템플릿을 선택하여 내용을 작성합니다. `Assignee`는 `MR`을 등록하는 계정으로 지정하고 `Reviewer`는 동료 개발자 혹은 소스리뷰 담당자를 지정합니다. `Sprint Backlog`의 경우 `Milestone`을 선택합니다. Labels는 `Documentation` 지정합니다.<br/>

작성이 완료되면 **Create Merge Request** 버튼을 클릭합니다. <br>

{{< figure src="/categories/images/TIL/guide6.png#center" caption="" >}}

#### 5. Merge Request 코드 리뷰 후 merge

같은 `feature`를 개발하는 동료에게 리뷰 승인을 받은 후 자신의 `Pull Request`를 `merge`합니다. 
만약 혼자 `feature`를 개발한다면 1~2명의 동료에게 리뷰 승인을 받은 후 `Pull Request`를 `merge` 합니다. 
- 리뷰어는 코드리뷰 후 승인버튼을 클릭합니다

{{< figure src="/categories/images/TIL/guide7.png#center" caption="" >}}
- 리뷰어의 승인이 완료되면 `merge` 버튼을 클릭하여 `develop` 브랜치까지 `merge` 작업 완료합니다.

