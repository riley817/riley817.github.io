# gitignore 적용하기


## .gitignore
- **.gitignore**에 의도적으로 추적을 원하지 않는 파일을 무시하도록 지정할 수 있다.
- 그러나 git이 이미 추적을 한 파일은 영향을 받지 않는다. 이미 한번 추적이 된 파일을 **.gitignore** 파일에 적용하려면 아래와 같이 캐시를 삭제해 주어야 한다.

```bash
git rm -r --cached .
git add .
git commit -m "Apply .gitignore"
git push
```
