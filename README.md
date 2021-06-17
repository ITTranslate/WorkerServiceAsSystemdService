# .NET Worker Service 部署到 Linux 作为 Systemd Service 运行

<!-- [Read the related article](https://ittranslator.cn/dotnet/csharp/2021/06/17/worker-service-as-windows-services.html). -->

基于 [WorkerServiceAsWindowsService](https://github.com/ITTranslate/WorkerServiceAsWindowsService) 项目修改。

```bash
git clone git@github.com:ITTranslate/WorkerServiceAsWindowsService.git
```

## 删除不用的依赖库

```bash
dotnet remove package Microsoft.Extensions.Hosting.WindowsServices
```

## 添加必要的依赖库

```bash
dotnet add package Microsoft.Extensions.Hosting.Systemd
```

## 修改文件

修改的文件包含： *appsettings.json*, *Program.cs*, *Worker.cs*

## 构建

```bash
dotnet build
```

## 发布

```bash
dotnet publish -c Release -r linux-x64 -o c:\test\workerpub\linux
```

## 安装服务

未完待续...

<!-- 
使用 sc.exe 实用工具安装和管理服务。

```bash
sc create MyService binPath= C:\test\workerpub\MyService.exe start= auto displayname= "技术译站的测试服务"

sc description MyService "这是一个由 Worker Service 实现的测试服务。"

sc start MyService

sc stop MyService

sc delete MyService
``` -->
