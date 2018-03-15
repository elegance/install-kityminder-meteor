## kityminder 脑图 Docker 版
基于 `https://github.com/yivanus/install-kityminder-meteor` 改造.

#### 主要解决一下几个问题：
* apt-get 源安装 git慢，**将源替换为阿里云ubuntu的镜像**
* https://install.meteor.com | sh 安装meteor时，下载`meteor-bootstrap-os.linux.x86_64.tar.gz` 很慢的问题 —— 先本地用下载工具下载好 meteor-bootstrap-os.linux.x86_64.tar.gz，然后在宿主机上启动 nginx 映射此文件，使用 sed修改shell内容的下载地址为172.17.0.1
* remove blaze-html-templates 不存在此目录的问题 —— 直接注释掉了
* 界面 error: conflict: two packages included in the app (angular-templates and static-html) are both trying to handle *.html 问题 —— 增加 `meteor remove static-html`


#### 使用方法

1. docker 宿主机下载好 `meteor-bootstrap-os.linux.x86_64.tar.gz`

2. 启动web服务(如nginx,nodejs http-server)，将`meteor-bootstrap-os.linux.x86_64.tar.gz`放置在根目录

3. 注意看下的docker 容器是否可以通过`172.17.0.1`访问到宿主机的http服务，如果不行，请自行修改`Dockfile`中的`172.17.0.1`地址为你的宿主机ip地址。

4. 构建镜像

   ```bash
   docker build -t elegance/naotu:latest -f naotu.Dockerfile .
   ```

5. 启动容器

   ```bash
   docker run -itd --name naotu -p 8899:8899 elegance/naotu:latest
   ```

   ​

