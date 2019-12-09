# npm 设置

nodejs 的包管理通过 npm 来管理，但是但是， 使用 npm 来安装包的时候， 速度慢的无法接受，还经常失败， 所以要安装一个 cnpm 来替换：

在命令行输入下面命令来安装 cnpm:
~~~text
npm install -g cnpm --registry=https://registry.npm.taobao.org
~~~

安装好之后，就可以使用 cnpm 来管理 nodejs 的包了。

安装好 cnpm 还不管问题全部解决了，有些程序内部还是会使用 npm 来进行包管理，如 react-create-app 程序，所以我们还要把 npm 的默认地址改为淘宝的镜像：

~~~text
npm config set registry https://registry.npm.taobao.org
~~~

处理完这些之前，整个 nodejs 的包管理应该就没有什么问题了。


cnpm 是淘宝的 npm 镜像，官网是 <http://npm.taobao.org/>

# 在 PowerShell 下执行脚本错误

现在一般使用 **ps** 来执行命令行，但是在使用 npm 安装一个工具后再执行命令，会发现有安全错误，这个是因为 ps 默认情况下是禁止从远程下载脚本来执行，需要修改一下 ps 的安全设置：

~~~text
Set-ExecutionPolicy RemoteSigned
~~~

> 注意：必须使用管理员模式运行才能执行此命令。
