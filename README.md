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

## 服务单元配置文件

MyService.service

```ini
[Unit]
Description=Long running service/daemon created from .NET worker template

[Service]
# The systemd service file must be configured with Type=notify to enable notifications.
Type=notify
# will set the Current Working Directory (CWD). Worker service will have issues without this setting
WorkingDirectory=/srv/Worker
# systemd will run this executable to start the service
ExecStart=/srv/Worker/MyService
# to query logs using journalctl, set a logical name here  
SyslogIdentifier=MyService

# Use your username to keep things simple.
# If you pick a different user, make sure dotnet and all permissions are set correctly to run the app
# To update permissions, use 'chown yourusername -R /srv/Worker' to take ownership of the folder and files,
#       Use 'chmod +x /srv/Worker/Worker' to allow execution of the executable file
User=yourusername

# ensure the service restarts after crashing
# Restart=always
# amount of time to wait before restarting the service
# RestartSec=5

# This environment variable is necessary when dotnet isn't loaded for the specified user.
# To figure out this value, run 'env | grep DOTNET_ROOT' when dotnet has been loaded into your shell.
Environment=DOTNET_ROOT=/usr/share/dotnet/dotnet

# This gives time to MyService to shutdown gracefully.
TimeoutStopSec=300

[Install]
WantedBy=multi-user.target
```

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
