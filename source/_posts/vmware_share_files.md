---
title: vmware中自动挂载共享文件夹的问题
date: 2025-03-15
tags: vmware
category: tech
---
安装`vmware`之后，想要使用共享文件夹，按照向导执行操作之后，共享文件夹还是没有出现。这个时候还需要手动挂载一下这个文件夹，使用下面的命令执行挂载。

```shell
sudo mount -t fuse.vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other
```

执行完挂载命令之后，可以在`/mnt/hgfs/`目录下看到我们挂载的共享文件夹。但是在`ubuntu`重启之后还需要再次执行这个命令，我们可以把这个命令加入到开机执行的脚本中。

在用户目录下新建一个`bin`目录，并且新建一个我们需要开机启动的脚本`init.sh`。

```shell
#! /bin/bash
sudo mount -t fuse.vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other
exit 0

```

添加`chmod +x  init.sh`命令，修改`/etc/rc.local`文件，把启动脚本添加进该文件中。

```shell
#! /bin/bash -e
bash /home/ubuntu/bin/init.sh
exit 0
```

添加执行权限`chmod +x /etc/rc.local`，下次再次启动的时候就可以自动挂载共享文件夹了。
