# .NET Worker Service 部署到 Linux 作为 Systemd Service 运行

[Read the related article](https://ittranslator.cn/dotnet/csharp/2021/06/29/worker-service-as-systemd-services-on-linux.html).

基于 [WorkerServiceAsWindowsService](https://github.com/ITTranslate/WorkerServiceAsWindowsService) 项目修改。

```bash
git clone git@github.com:ITTranslate/WorkerServiceAsWindowsService.git
```

## 删除不用的依赖项

```bash
dotnet remove package Microsoft.Extensions.Hosting.WindowsServices
```

## 添加必要的依赖程序包

```bash
dotnet add package Microsoft.Extensions.Hosting.Systemd
```

## 修改文件

修改的文件包含： *appsettings.json*, *Program.cs*

## 构建

```bash
dotnet build
```

## 发布

```bash
dotnet publish -c Release -r linux-x64 -o c:\test\workerpub\linux
```

## 管理服务

```bash
# 查看状态
systemctl status MyService

# 启动
systemctl start MyService

# 停止
systemctl stop MyService

# 开机自动启动
systemctl enable MyService

# 禁用开机自动启动
systemctl disable MyService

# 查看服务是否开机自动启动
systemctl is-enabled MyService

# 查看日志
journalctl -u MyService -f
```
