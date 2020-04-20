
## 3.1 Jenkinsfile Linter

```
curl -X POST -F "jenkinsfile=<./3.1/Jenkinsfile" http://localhost:8080/pipeline-model-converter/validate
```

catches syntax errors

- try quote mismatch
- missing brackets for call

> VS Code linter integration


## 3.2 Pipeline Replay & Restart

restart from stage uses original lib
replay - allows edit

## 3.3 Pipeline Unit Test