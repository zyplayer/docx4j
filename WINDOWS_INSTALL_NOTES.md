# Windows 本地 install 说明

## 推荐命令

```powershell
mvn -Dgpg.skip=true -DskipTests install -f pom.xml
```

在 IntelliJ Maven Runner 里也可以只加：

```text
-Dgpg.skip=true -DskipTests
```

## 已做的本地修改

`docx4j-generated-objects/pom.xml` 增加了 Windows profile：

- Windows 下使用 `D:/install/Git/bin/bash.exe`
- 由 `bash.exe` 执行 `./modify-generated-sources.sh`
- 非 Windows 环境仍保持原来的 `.sh` 执行方式

原因：Windows 不能直接执行 `.sh` 文件，否则会报：

```text
CreateProcess error=193, %1 不是有效的 Win32 应用程序
```

## 报错处理

### maven-gpg-plugin: sign 失败

本地 install 不需要签名，加：

```text
-Dgpg.skip=true
```

### modify-generated-sources.sh 执行失败

这是生成源码后的修正脚本，不能随便跳过。Windows 下需要用 Git Bash 执行。

注意：当前写死的 Git Bash 路径是：

```text
D:/install/Git/bin/bash.exe
```

如果本机 Git 安装路径不同，需要同步修改 `docx4j-generated-objects/pom.xml`。

### maven-surefire-plugin 测试失败

当前 JDK 21 下测试 JVM 可能因为模块依赖缺失失败，例如：

```text
Module org.apache.commons.codec not found, required by org.apache.commons.compress
```

如果只是本地安装依赖，直接跳过测试：

```text
-DskipTests
```

