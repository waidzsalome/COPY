# 视频教学
## 组件间的传值
- 父组件向子组件传递内容通过属性的形式传递,既可以传递数据，也可以传递方法
- 子组件接收内容
```bash
{this.props.content}
```
- 子组件调用父组件的方法
```bash
this.props.xxx(一个属性）(this.props.xxx);
在父组件传递过来的函数，绑定this指向
```
## props，state，与render函数的关系
- 当组件的state或者props发生改变的时候，render函数会被重新执行一次

##  虚拟DOM
- 虚拟DOM就是一个JS对象，用它来描述真实DOM
- 能提高性能是因为js中比较对象不怎么消耗性能，真实的DOM很耗性能
### 深入了解虚拟DOM
- //4-5,4-6.

## ref(不建议使用)
- 可以用e.target获得事件对应的元素对应的节点

## react生命周期函数
- 生命周期函数是指某一时刻组件会自动调用执行的函数
- constructor在组件一创建就被调用
- mounting（挂载，组件第一次被执行）
    - componentWillMount 在组件即将被挂载到页面时自动执行，现在已经不用了
    - render 做页面的挂载
    - componentDidMount 在组件被挂载到页面之后自动被执行
- upadation
    - componentWillReceiveProps 一个组件要从父组件接受参数 只要父组件的render函数被重新执行 子组件的生命周期函数就会被执行
        - 如果这个组件第一次存在与父组件中，不会执行
        - 如果这个组件之前已经崔在与父组件中，才会执行
    - shouldComponentUpdate 组件被更新之前 被自动执行 返回值是bool
    - componentWillUpdate 组件被更新之前执行，在should之后，且should返回指为true
    - componentDidUpdate 组件更新完成之后
- Unmounting
    - componentWillUnmount 当这个组件被从页面移除的时候
## 使用charles进行接口数据模拟
- 物业项目线上地址http://154.8.214.49:8080
    
## 发送ajax请求
- 使用axios 
```bash
yarn add axios
```
## react-transition-group 实现动画

## 上线一个项目
### 本地
```bash
cd到dist文件目录的上一层
eg.  cd /home/waidzsalome/文档/Project/SignUp
使用scp命令递归上传： scp -r dist ubuntu@ip地址:/var/www/你用来存储页面的文件夹
eg. scp -r dist ubuntu@188.131.185.76:/var/www/Test
```
### 服务器
前期准备
```bash
切换到root用户: su root
删除一堆没用的
安装nginx： apt-get install nginx
查看nginx下目录： ls 
本次实践将nginx配置放在conf.d文件夹内
```
创建存放页面代码的文件夹
```bash
创建文件夹：mkdir Test
查看文件夹权限： ll
修改文件夹权限： chown -R ubuntu:ubuntu Test/
```
配置nginx
```bash
创建文件： touch Test.conf
配置文件夹内容
重启nginx： sudo nginx -s reload
```
Test.conf内容
```bash
server {
        listen 80;
        #listen [::]:80 default_server ipv6only=on;

        root /var/www/Test; 存放代码的地方，Test文件夹要有index.html文件
        index index.html index.htm;

        # Make site accessible from http://localhost/  
        server_name Test.waidzsalome.cn;//访问时的二级域名

        location / {
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;

        }
}
```

常用命令
```bash
打开文件进行编辑： vi 文件名
编辑： a
保存并退出 :wq
```

### 域名解析
```bash
主机记录： 二级域名
记录值： 服务器地址
```
