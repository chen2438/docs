---
description: 转载于 2022-10-26
categories:
- ubuntu
date: 2022-10-26
slug: samba
title: Ubuntu 安装 samba
---

# Ubuntu 安装 samba

[参考链接](https://www.myfreax.com/how-to-install-and-configure-samba-on-ubuntu-18-04/#-samba-)

我们将创建以下Samba共享和用户。

用户：

* **sadmin** -具有对所有共享的读写访问权限的管理用户。
* **josh** -具有自己的私有文件共享的常规用户。

共享：

* **users** -所有用户都可以使用读取/写入权限访问此共享。
* **josh** -此共享可以通过读取/访问仅由josh和sadmin用户写权限。

可以从网络上的所有设备访问文件共享。

### 在Ubuntu上安装Samba

更新apt软件包索引：

```bash
sudo apt update
```

安装Samba软件包

```bash
sudo apt install samba
```

安装完成后，Samba服务将自动启动。要检查Samba服务器是否正在运行，请键入：

```bash
sudo systemctl status nmbd
```

输出应如下所示，表明Samba服务处于活动状态并正在运行：

```bash
● nmbd.service - Samba NMB Daemon
Loaded: loaded (/lib/systemd/system/nmbd.service; enabled; vendor preset: enabled)
Active: active (running) since Sun 2019-01-27 02:36:20 PST; 4s ago
    Docs: man:nmbd(8)
        man:samba(7)
        man:smb.conf(5)
Main PID: 4262 (nmbd)
Status: "nmbd: ready to serve connections..."
    Tasks: 1 (limit: 2319)
CGroup: /system.slice/nmbd.service
        `-4262 /usr/sbin/nmbd --foreground --no-process-group
```

至此，已经安装了Samba并准备对其进行配置。

### 配置防火墙

如果您的Ubuntu系统上运行着防火墙，则需要允许端口`137`和`138`上的传入UDP连接以及端口`139`和`445`上的TCP连接。

假设您正在使用 `UFW` 管理防火墙，则可以通过启用“ Samba”配置文件来打开端口：

```bash
sudo ufw allow 'Samba'
```

### 配置全局Samba选项

在更改Samba配置文件之前，创建备份以供将来参考：

```bash
sudo cp /etc/samba/smb.conf{,.backup}
```

Samba软件包随附的默认配置文件是为独立的Samba服务器配置的。打开文件并确保将`server role`设置为`standalone server`

```bash
sudo nano /etc/samba/smb.conf
```

/etc/samba/smb.conf

```bash
...
# Most people will want "standalone sever" or "member server".
# Running as "active directory domain controller" will require first
# running "samba-tool domain provision" to wipe databases and create a
# new domain.
   server role = standalone server
...
```

默认情况下，Samba监听所有接口。如果您只想从内部网络限制对Samba服务器的访问，请取消注释以下两行并指定要绑定到的接口：

/etc/samba/smb.conf

```bash
...
# The specific set of interfaces / networks to bind to
# This can be either the interface name or an IP address/netmask;
# interface names are normally preferred
interfaces = 127.0.0.0/8 eth0

# Only bind to the named interfaces and/or networks; you must use the
# 'interfaces' option above to use this.
# It is recommended that you enable this feature if your Samba machine is
# not protected by a firewall or is a firewall itself.  However, this
# option cannot handle dynamic or non-broadcast interfaces correctly.
bind interfaces only = yes
...
```

完成后，运行`testparm`实用程序以检查Samba配置文件中是否有错误。如果没有语法错误，您将看到`Loaded services file OK.`

最后，使用以下命令重新启动Samba服务：

```bash
sudo systemctl restart nmbd
```

### 创建Samba用户和目录结构

为了更易于维护和灵活性，而不是使用标准主目录（`/home/user`），所有Samba目录和数据都位于`/samba`目录中。

要创建`/samba`目录，请输入：

```bash
sudo mkdir /samba
```

将群组所有权设置为`sambashare`。该组是在安装Samba的过程中创建的，稍后我们将所有Samba用户添加到该组。

```bash
sudo chgrp sambashare /samba
```

Samba使用Linux用户和组权限系统，但具有其自己的身份验证机制，与标准Linux身份验证分开。我们将使用标准的Linux `useradd`工具创建用户，然后使用`smbpasswd`实用程序设置用户密码。

正如引言中所述，我们将创建一个普通用户，该用户将有权访问其私有文件共享和一个具有对Samba服务器上所有共享的读写访问权限的管理帐户。

#### 创建Samba用户

要创建名为`josh`的新用户，请使用以下命令：

```bash
sudo useradd -M -d /samba/josh -s /usr/sbin/nologin -G sambashare josh
```

`useradd`选项的含义如下：

* `-M`-不创建用户的主目录。我们将手动创建此目录。
* `-d /samba/josh` -将用户的主目录设置为`/samba/josh`。
* `-s /usr/sbin/nologin` -禁止该用户访问shell。
* `-G sambashare` -将用户添加到`sambashare`组。

创建用户的主目录，并将目录所有权设置为用户`josh`和组`sambashare`：

```bash
sudo mkdir /samba/josh
sudo chown josh:sambashare /samba/josh
```

以下命令会将setgid位添加到`/samba/josh`目录，以便该目录中的新创建文件将继承父目录的组。这样，无论哪个用户创建新文件，该文件的组所有者均为`sambashare`。例如，如果您未将目录的权限设置为`2770`，并且`sadmin`用户创建了一个新文件，则该用户`josh`将无法读取/写入该文件。

```bash
sudo chmod 2770 /samba/josh
```

通过设置用户密码将`josh`用户帐户添加到Samba数据库：

```bash
sudo smbpasswd -a josh
```

系统将提示您输入并确认用户密码。

```bash
New SMB password:
Retype new SMB password:
Added user josh.
```

一旦设置了密码即可启用Samba帐户运行：

```bash
sudo smbpasswd -e josh
Enabled user josh.
```

要创建另一个用户，请重复与创建用户`josh`时相同的过程。

接下来，让我们创建一个用户和组`sadmin`。该组的所有成员将具有管理权限。稍后，如果您想授予其他用户管理权限，只需将该用户添加到`sadmin`组。

通过键入以下内容来创建管理用户：

```bash
sudo useradd -M -d /samba/users -s /usr/sbin/nologin -G sambashare sadmin
```

上面的命令还将创建一个组`sadmin`，并将用户添加到`sadmin`和`sambashare`组中。

设置密码并启用用户：

```bash
sudo smbpasswd -a sadmin
sudo smbpasswd -e sadmin
```

接下来，创建`Users`共享目录：

```bash
sudo mkdir /samba/users
```

将目录所有权设置为用户`sadmin`和组`sambashare`：

```bash
sudo chown sadmin:sambashare /samba/users
```

所有身份验证的用户都可以访问此目录。以下 `chmod` 命令为`/samba/users`目录中的`sambashare`组的成员提供写/读访问权限：

```bash
sudo chmod 2770 /samba/users
```

### 配置Samba共享

打开Samba配置文件，并添加以下部分：

```bash
sudo nano /etc/samba/smb.conf
```

/etc/samba/smb.conf

```bash
[users]
    path = /samba/users
    browseable = yes
    read only = no
    force create mode = 0660
    force directory mode = 2770
    valid users = @sambashare @sadmin

[josh]
    path = /samba/josh
    browseable = no
    read only = no
    force create mode = 0660
    force directory mode = 2770
    valid users = josh @sadmin
```

选项的含义如下：

* `[users]`和`[josh]`-登录时将使用的共享名称。
* `path` -分享的路径。
* `browseable` -是否应在可用共享列表中列出该共享。通过设置为`no`，其他用户将看不到共享。
* `read only` -`valid users`列表中指定的用户是否能够写入此共享。
* `force create mode` -设置此共享中新创建文件的权限。
* `force directory mode` -设置此共享中新创建目录的权限。
* `valid users` -允许访问共享的用户和组的列表。群组以`@`符号为前缀。

有关可用选项的更多信息，请参见Samba配置文件文档页面。

完成后，使用以下方法重新启动Samba服务：

```bash
sudo systemctl restart nmbd
```

在以下各节中，我们将向您展示如何从Linux，macOS和Windows客户端连接到Samba共享。

### 从Linux连接到Samba共享

Linux用户可以使用文件管理器从命令行访问samba共享或挂载Samba共享。

#### 使用smbclient客户端

`smbclient`是允许您从命令行访问Samba的工具。 `smbclient`软件包尚未预先安装在大多数Linux发行版中，因此您需要使用分发软件包管理器进行安装。

要在Ubuntu和Debian上安装，请运行：

```bash
sudo apt install smbclient
```

要在CentOS和Fedora上安装`smbclient`，请运行：

```bash
sudo yum install samba-client
```

访问Samba共享的语法如下：

```bash
mbclient //samba_hostname_or_server_ip/share_name -U username
```

例如，要以用户`josh`的身份连接到IP地址为`192.168.121.118`的Samba服务器上名为`josh`的共享，您将运行：

```bash
smbclient //192.168.121.118/josh -U josh
```

系统将提示您输入用户密码。

```bash
Enter WORKGROUP\josh's password: 
```

输入密码后，您将登录Samba命令行界面。

```bash
Try "help" to get a list of possible commands.
smb: \>
```

#### 安装Samba共享

要在Linux上挂载Samba共享，您需要安装`cifs-utils`软件包。

在Ubuntu和Debian上运行：

```bash
sudo apt install cifs-utils
```

在CentOS和Fedora上运行：

```bash
sudo yum install cifs-utils
```

接下来，创建安装点：

```bash
sudo mkdir /mnt/smbmount
```

使用以下命令挂载共享：

```bash
sudo mount -t cifs -o username=username //samba_hostname_or_server_ip/sharename /mnt/smbmount
```

例如，将名为`josh`的共享作为用户`josh`装载到IP地址为`192.168.121.118`的Samba服务器上，将其运行到`/mnt/smbmount`装载点：

```bash
sudo mount -t cifs -o username=josh //192.168.121.118/josh /mnt/smbmount
```

系统将提示您输入用户密码。

```bash
Password for josh@//192.168.121.118/josh:  ********
```

#### 使用GUI

“文件”是Gnome中的默认文件管理器，具有内置选项来访问Samba共享。

1. 打开文件，然后单击侧栏中的“其他位置”。
2. 在“连接到服务器”中，以以下格式`smb://samba_hostname_or_server_ip/sharename`输入Samba共享的地址。
3. 单击“连接”，将出现以下屏幕：
4. 选择“注册用户”，输入Samba用户名和密码，然后单击“连接”。
5. 将显示Samba服务器上的文件。

### 从macOS连接到Samba共享

在macOS中，您可以从命令行或使用默认的macOS文件管理器Finder访问Samba共享。以下步骤显示了如何使用Finder访问共享。

1. 打开“查找器”，选择“执行”，然后单击“连接到”。
2. 在“连接到”中，以以下格式`smb://samba_hostname_or_server_ip/sharename`输入Samba共享的地址。
3. 点击“连接”：
4. 选择“注册用户”，输入Samba用户名和密码，然后单击“连接”。
5. 将显示Samba服务器上的文件。

### 从Windows连接到Samba共享

Windows用户还可以选择从命令行和GUI连接到Samba共享。以下步骤显示了如何使用Windows File Explorer访问共享。

1. 打开文件资源管理器，然后在左窗格中右键单击“此PC”。
2. 选择“选择自定义网络位置”，然后单击“下一步”。
3. ]在“互联网或网络地址”中，以以下格式输入Samba共享的地址`\\samba_hostname_or_server_ip\sharename`。
4. 单击“下一步”，将提示您输入登录凭据，如下所示
5. 在下一个窗口中，您可以为网络位置键入自定义名称。默认值将由Samba服务器接收
6. 单击“下一步”移至连接设置向导的最后一个屏幕。
7. 单击“完成”，将显示Samba服务器上的文件。

### 结论

在本教程中，您学习了如何在Ubuntu 18.04上安装Samba服务器以及如何创建不同类型的共享和用户。我们还向您展示了如何从Linux，macOS和Windows设备连接到Samba服务器。
