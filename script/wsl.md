# WSL
[Win10中内置Linux更换阿里源]: https://blog.csdn.net/lu900618/article/details/74955065

[Win-KeX](https://www.kali.org/docs/wsl/win-kex/)

```powershell
wslconfig /l ## 查看发行版本
wslconfig /setdefault Name ## 设置默认版本
```

```powershell
# 列出分发版
wsl -l , wsl --list
wsl --list --all
wsl --list --running
# 设置默认分发版
wsl -s Ubuntu
# 取消注册和重新安装分发版
# 注销后，与该分发关联的所有数据、设置和软件都将永久丢失。 从 Store 重新安装会安装分发版的干净副本。
wsl --unregister <DistributionName>
wsl --unregister Ubuntu
# 将 WSL 2 设置为默认版本
wsl --set-default-version 2
# 用于管理适用于 Linux 的 Windows 子系统的参数
--export <分发版> <文件名>  将分发版导出到 tar 文件
--import <分发版> <安装位置> <文件名>  导入指定的 tar 文件作为新的分发版
--list、-l [选项]  列出分发版

wsl --shutdown
立即终止所有正在运行的发行版和 WSL 2 轻量级实用程序虚拟机
wsl --terminate Ubuntu 关闭ubuntu

service docker status/start/restart
```

