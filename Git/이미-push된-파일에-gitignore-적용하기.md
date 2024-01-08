```
git rm -r --cached .
git add .
git commit -m "Apply .gitignore"
git push
```

- git rm -r: 하위 디렉토리를 포함해 모두 삭제
- git rm -r --cached: Staging Area에서만 제거하고, 워킹 디렉토리에 있는 파일은 제거하지 않음