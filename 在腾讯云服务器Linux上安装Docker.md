# **在腾讯云服务器上基于CentOS 7安装Docker**



## 选择云服务器



**<font color='red'>云服务器</font>(Elastic Compute Service, ECS)**是一种<font color='cornflowerblue'>简单高效</font>、<font color='cornflowerblue'>安全可靠</font>、<font color='cornflowerblue'>处理能力可弹性伸缩</font>的计算服务。

<font color='cornflowerblue'>云服务器</font>管理方式比<font color='cornflowerblue'>物理服务器</font>更简单高效，我们无需提前购买昂贵的硬件，即可迅速创建或删除云服务器，云服务器费用一般在几十到几百不等，可以根据我们的需求配置。

------

目前市场上的云服务器很多，这里主要介绍三家：

> - [阿里云](https://www.aliyun.com/)：活动折扣力度很大
> - [腾讯云](https://cloud.tencent.com/)：腾讯云目前活动多一些，性价比也高
> - [华为云](https://www.huaweicloud.com/)：领券购买也很划算

我这里是选择了腾讯云的新用户免费送的一款云服务器来安装Docker。

------



## 配置云服务器



- 打开腾讯云的<font color='cornflowerblue'>控制台</font>界面，点击左侧信息栏的“<font color='cornflowerblue'>实例与镜像</font>“内的“<font color='cornflowerblue'>实例</font>”，就能看到你拥有的云服务器。

![image-20210825022417628](https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022417628.png)

- 点击服务器信息右侧“<font color='cornflowerblue'>操作</font>“中的“<font color='cornflowerblue'>登录</font>”

  <img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210822022846038.png" alt="image-20210822022846038" style="zoom:50%;" />
  
  

- 选择“<font color='cornflowerblue'>标准登录方式</font>”，点击“<font color='cornflowerblue'>立即登录</font>”，这里我选择的是密码登录（<font color='cornflowerblue'>用户名密码在你领取云服务器时会给你初始密码</font>），点击确定。

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022439917.png" alt="image-20210825022439917" style="zoom: 67%;" />

- 登录后就会进入云服务器的界面

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022508918.png" alt="image-20210825022508918" style="zoom: 67%;" />

------



## 安装Docker



### 安装环境

- 先看看我这台云服务器的环境

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022527234.png" alt="image-20210825022527234" style="zoom: 67%;" />

### 卸载旧版本

- 较旧的 Docker 版本称为 <font color='red'>docker</font> 或 <font color='red'>docker-engine</font> 。如果已安装这些程序，请卸载它们以及相关的<font color='cornflowerblue'>依赖项</font>。


```shell
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```





### 需要的安装包

```shell
yum install -y yum-utils
```



### 使用 Docker 仓库进行安装

#### 官方源地址

- 由于官方源地址是国外的链接，下载速度会比较慢，<font color='red'>**不建议使用**</font>。


```shell
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

#### 阿里云地址（<font color='cornflowerblue'>推荐</font>）

```shell
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

#### 清华大学源

```shell
yum-config-manager \
    --add-repo \
    https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
```



- 这样就是显示已经安装好源了

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210825022549896.png" alt="image-20210825022549896" style="zoom:67%;" />

### 更新yum软件包索引

```shell
yum makecache fast
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210825022608201.png" alt="image-20210825022608201" style="zoom:67%;" />

### 安装Docker Engine-Community



- 安装最新版本的 <font color='red'>Docker Engine-Community</font> 和 <font color='red'>containerd</font>，或者转到下一步安装特定版本。(<font color='red'>docker-ce</font> 是社区版  <font color='red'>docker-ee</font> 是企业版)


```shell
yum install docker-ce docker-ce-cli containerd.io
```

​	

- 安装过程中会提示让你确认，输入 **<font color='red'>y</font>** 即可。

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022623053.png" alt="image-20210825022623053" style="zoom:67%;" />



- 显示 <font color='red'>Complete！</font>代表安装成功。

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022642272.png" alt="image-20210825022642272" style="zoom:67%;" />

##### 安装特定版本

- 列出并排序您<font color='cornflowerblue'>存储库</font>中可用的版本。此示例按版本号<font color='cornflowerblue'>（从高到低）</font>对结果进行排序。

```shell
yum list docker-ce --showduplicates | sort -r

docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable
```



- 通过其完整的软件包名称安装<font color='cornflowerblue'>特定版本</font>，该软件包名称是软件包名称（<font color='red'>docker-ce</font>）加上版本字符串（<font color='red'>第二列</font>），从第一个冒号（<font color='red'>:</font>）一直到第一个连字符，并用<font color='red'>连字符</font>（<font color='red'>-</font>）分隔。

```shell
yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
```

------



## 启动Docker



- <font color='cornflowerblue'>启动</font>Docker

```shell
systemctl start docker
```



- 查看Docker的<font color='cornflowerblue'>版本</font>

```shell
docker version
```

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022659734.png" alt="image-20210825022659734" style="zoom:67%;" />

- 通过运行 <font color='cornflowerblue'>hello-world</font> 映像来验证是否正确安装了 <font color='red'>Docker Engine-Community</font>。

```shell
docker run hello-world
```

出现 “<font color='red'>**Hello from Docker！**</font>” 证明运行成功。

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022717198.png" alt="image-20210825022717198" style="zoom:50%;" />

- 查看一下下载的<font color='cornflowerblue'>hello-world</font>镜像

```shell
docker images
```

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022732402.png" alt="image-20210825022732402" style="zoom: 67%;" />

------



## 卸载Docker



- 删除安装包

```shell
yum remove docker-ce docker-ce-cli containerd.io
```

- 删除<font color='cornflowerblue'>镜像</font>、<font color='cornflowerblue'>容器</font>、<font color='cornflowerblue'>配置文件</font>等内容

```shell
rm -rf /var/lib/docker
```

------



## 写在最后



以上就是在腾讯云服务器上安装Docker的全部过程，本文完整教学过程在此：[CentOS Docker安装](https://www.runoob.com/docker/centos-docker-install.html)，需要可自行查看。

```
由于能力有限，若有错误或者不当之处，还请批评指正。
```

