---
description: 发布于 2022.10.25
---

# ubuntu 安装 Python 3 和 pip

#### 安装 Python 3.10

[参考链接](https://computingforgeeks.com/how-to-install-python-on-ubuntu-linux-system/)

Install the required dependency for adding custom PPAs.

```
sudo apt install software-properties-common -y
```

Then proceed and add the deadsnakes PPA to the APT package manager sources list as below.

```
sudo add-apt-repository ppa:deadsnakes/ppa
```

Press **Enter** to continue.

```
..........
To install 3rd-party Python modules, you should use the common Python packaging tools.  For an introduction into the Python packaging ecosystem and its tools, refer to the Python Packaging User Guide:
https://packaging.python.org/installing/

Sources
=======
The package sources are available at:
https://github.com/deadsnakes/

Nightly Builds
==============

For nightly builds, see ppa:deadsnakes/nightly https://launchpad.net/~deadsnakes/+archive/ubuntu/nightly
 More info: https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa
Press [ENTER] to continue or ctrl-c to cancel adding it
```

With the deadsnakes repository added to your Ubuntu 20.04|18.04 system, now download Python 3.10 with the single command below.

```
sudo apt install python3.10
```

Dependency Tree:

```
...................
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libpython3.10-minimal libpython3.10-stdlib python3.10-minimal
Suggested packages:
  python3.10-venv binfmt-support
The following NEW packages will be installed:
  libpython3.10-minimal libpython3.10-stdlib python3.10 python3.10-minimal
0 upgraded, 4 newly installed, 0 to remove and 192 not upgraded.
Need to get 5,023 kB of archives.
After this operation, 19.7 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
```

Verify the installation by checking the installed version.

```
$ python3.10 --version
3.10.4
```

#### 脚本安装 pip

[参考链接](https://pip.pypa.io/en/latest/installation/)

<pre class="language-bash"><code class="lang-bash"><strong>wget https://bootstrap.pypa.io/get-pip.py</strong></code></pre>

```bash
python get-pip.py
```
