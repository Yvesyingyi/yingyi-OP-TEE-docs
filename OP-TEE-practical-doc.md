# OP-TEE实用文档

本文档主要记录OP-TEE的安装方法，以及使用gdb调试内核代码的方法。

## 安装-代码同步

### 关于git repo

OP-TEE整个开发环境的代码需要使用git repo建立。[git repo是依赖于git，负责自动化多个git仓库的拉取和同步的工具](https://source.android.com/setup/develop)，属于AOSP的一部分，与git submodule功能类似。

![repo flowchart](img-practical-doc/01.jpg)

整个流程如图。蓝色是OP-TEE的相应git，黄色是git repo所依赖的一个git，绿色是用户需要进行的操作。三个pull操作需要联网。执行完`repo sync`指令后代码就同步完成了。

### 官方代码同步方法

总结自官方文档[prerequisite](https://optee.readthedocs.io/en/latest/building/prerequisites.html)及[build](https://optee.readthedocs.io/en/latest/building/gits/build.html)页面：

#### 要求

Ubuntu系统

#### 执行指令

```bash
# 安装相应依赖
sudo apt-get install android-tools-adb android-tools-fastboot autoconf \
        automake bc bison build-essential ccache cscope curl device-tree-compiler \
        expect flex ftp-upload gdisk iasl libattr1-dev libc6:i386 libcap-dev \
        libfdt-dev libftdi-dev libglib2.0-dev libhidapi-dev libncurses5-dev \
        libpixman-1-dev libssl-dev libstdc++6:i386 libtool libz1:i386 make \
        mtools netcat python-crypto python3-crypto python-pyelftools \
        python3-pyelftools python-serial python3-serial rsync unzip uuid-dev \
        xdg-utils xterm xz-utils zlib1g-dev
# repo init
# 参数 使用qemu分支: -m default.xml 使用某版本: -b 3.7.0
repo init -u https://github.com/OP-TEE/manifest.git -m default.xml -b 3.7.0
# repo sync
repo sync -j4 --no-clone-bundle
```

### 加速代码同步

执行`repo init`时，需要从`https://gerrit.googlesource.com/`拉取资源，国内会被GFW拦截，repo自带的工具也不能设置代理，所以需要用代理执行`git clone -mirror https://gerrit.googlesource.com/git-repo`将整个仓库以bare的形式拉到本地，然后在`repo init`设定参数：
```bash
repo init --repo-url=<下载的git-repo.git路径> -u https://github.com/OP-TEE/manifest.git -m default.xml -b 3.7.0
```

执行`repo sync`时，也需要从多个来源拉取git仓库，其中包括整个Linux内核，因此速度很慢。要解决这个问题，可以在执行完`repo init`后，在`.repo/manifests/default.xml`中找到所有的相关url，全部用`git clone -mirror`拉到本地（需要一定时间），然后把`default.xml`里所有的http地址替换成本地的仓库路径。这样之后如果需要重新执行`repo sync`，就可以直接指定使用本地仓库了，将几个小时到十几个小时的等待时间缩短到小于5分钟。使用命令如`sed -i "s+https://github.com+<本地路径>+g" .repo/manifests/default.xml`可以查找并替换所有的url，便于自动化。

## 安装-build

总结自官方文档[build](https://optee.readthedocs.io/en/latest/building/gits/build.html)页面：

```bash
cd build/
# arm-toolchains, 交叉编译依赖
make -j2 toolchains
# 总的make
make -j `nproc`
```

arm-toolchains在make里使用wget从互联网获取，因此有几率失败。可以从浏览器下载相应文件后，修改`toolchains.mk`脚本，让脚本使用本地文件就可以解决这个问题。
