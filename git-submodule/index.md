# [Git] git submodule 삭제하기



## git submodule 삭제하기

#### 1. .gitmodules를 열어 해당 서브 모듈이 정의된 부분을 제거하거나 파일을 지운다.
**.gitmodules**에 불필요한 모듈만 제거하거나 **.gitmodules** 파일이 불필요하다면 아예 삭제해 버린다. 
![images](/posts/images/git/gitsubmodule.png)

#### 2. .git/config 파일을 열어 불필요한 서브 모듈을 삭제한다.
```bash
vi PROJECT_ROOT/.git/config
```
#### 3. 해당 저장소의 캐시를 제거한다.
```bash
# git rm --cached path_to_submodule
git rm --cached spring-module-common
```

#### 4. .git/modules/path_to_submodule 파일을 삭제한다.
```bash
cd PROJECT_ROOT/.git/modules
rm --rf spring-module-common
```
#### 5. 변경된 사항을 커밋 한다.

## 참고
* [https://git.wiki.kernel.org/index.php/GitSubmoduleTutorial#Removal](https://git.wiki.kernel.org/index.php/GitSubmoduleTutorial#Removal)
