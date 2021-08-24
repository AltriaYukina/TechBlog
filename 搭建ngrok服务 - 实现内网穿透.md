

# 搭建ngrok服务 - 实现内网穿透



## 前言

为何需要<font color='red'>**ngrok**</font>服务？有些运营商（比如<font color='cornflowerblue'>移动</font>）是不会给用户发放<font color='red'>公网IP</font>，但我们又需要<font color='red'>公网IP</font>（<font color='cornflowerblue'>远程访问家里的NAS</font>、<font color='cornflowerblue'>远程家里的电脑</font>等）怎么办，那我们只能自己搭建一台==**内网穿透服务器**==。



## Ngrok是什么

> ngrok的核心功能：能够将你本机的HTTP服务（站点）或TCP服务，通过部署有ngrok服务的外网伺服器暴露给外网访问。



<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825000559226.png" alt="image-20210825000559226" style="zoom:80%;" />

如上图所示：

- 笔记本是你的<font color='red'>工作机器</font>，安装ngrok<font color='red'>**客户端**</font>
- <font color='red'>ngrok.com</font>所在的服务器安装了ngrok的<font color='red'>**服务端**</font>（<font color='cornflowerblue'>ngrokd</font>）
- 利用<font color='red'>ngrok 8080</font>命令可以将你本机的<font color='red'>8080</font>端口暴露给<font color='red'>**反向代理**</font>至ngrok.com的某个<font color='red'>二级域名</font>如：<font color='red'>xxx.ngrok.com</font>
- 别人通过访问<font color='red'>xxx.ngrok.com</font>就可以访问到你本机<font color='red'>8080</font>端口上的<font color='cornflowerblue'>站点内容</font>了。

------

ngrok服务是==开源==的，但是如果想让别人通过你自己的域名来访问站点，那么你就需要购买<font color='red'>域名</font>和<font color='red'>服务器</font>，而且服务器不是中国内地的话还需要**==备案==**。不想这么麻烦的话，也可以选择购买第三方的服务，例如国内有名的<font color='cornflowerblue'>花生壳</font>，

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022806421.png" alt="image-20210825022806421" style="zoom: 67%;" />

可以看到这价格也不算太便宜，接下来我将介绍一个免费搭建ngrok服务的办法。



## 搭建Ngrok

### 1.注册国内内网映射服务器账号

打开网站https://ngrok.cc/，注册账号。

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022822586.png" alt="image-20210825022822586" style="zoom:67%;" />



### 2.登录服务器后台界面

注册好账户后登录，进入服务器的<font color='red'>后台管理界面</font>。

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022840276.png" alt="image-20210825022840276" style="zoom:67%;" />



### 3.开通隧道

现在我们是没有开通服务的，我们需要去开通一个隧道。点开==隧道管理-开通隧道==，

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022902631.png" alt="image-20210825022902631" style="zoom:67%;" />

选择购买最后一个“<font color='cornflowerblue'>美国Ngrok免费服务器</font>”（既然想免费，就得接受服务器速度慢的现实），

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022914754.png" alt="image-20210825022914754" style="zoom:67%;" />

隧道协议随意，我这里选择“<font color='cornflowerblue'>http</font>”，隧道名称随意。前置域名购买后<font color='red'>无法修改</font>，所以要慎重填写。本地端口就是你需要映射的站点，我这里填的是我家里的路由器管理地址。

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022929868.png" alt="image-20210825022929868" style="zoom: 67%;" />

填写完后点“<font color='cornflowerblue'>确认添加</font>”=>"<font color='cornflowerblue'>确认开通</font>"，开通好后就会得到你的隧道信息了。

![image-20210825022944828](https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022944828.png)



### 4.安装客户端

点击“<font color='cornflowerblue'>客户端下载</font>”，根据你的系统选择下载对应版本，我这里下载的是“<font color='red'>Win 64Bit</font>”版本。

<img src="https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825022957506.png" alt="image-20210825022957506" style="zoom: 67%;" />

将下载得到的zip文件解压后就会得到"<font color='cornflowerblue'>windows_amd64</font>"这个文件。

![](C:\Users\Administrator\Downloads\image-20210825023011073.png)



### 5.配置Ngrok

打开“<font color='cornflowerblue'>Sunny-Ngrok启动工具.bat</font>”，第一行的“==客户端id==”就是隧道信息里的“==隧道id==“，将id复制粘贴到命令行上。

![](C:\Users\Administrator\Downloads\image-20210825023024963.png)



按“<font color='cornflowerblue'>回车</font>”执行命令，执行后得到如下信息，则证明隧道启动成功。

![image-20210825023038785](https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825023038785.png)

### 6.验证

复制http://fjmhome.free.idcfengye.com到浏览器运行，顺利跳转到我的路由器管理界面。

![image-20210825023049715](https://fjmimages.oss-cn-shenzhen.aliyuncs.com/img/image-20210825023049715.png)





## 写在最后

本文主要介绍了搭建ngrok服务的全部过程，也是十分适合新手的一种方法。

```
由于能力有限，若有错误或者不当之处，还请批评指正。
```

