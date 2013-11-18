#Web应用防火墙（Waf）
__Version 1.0  (2013-11-18)__

>是一个对HTTP会话执行一系列规则的应用，服务器组件，或者是一个硬件设备。一般这些规则覆盖了传统攻击手法：Xss，Sql injection。对于自己应用定制的规则，许多的攻击就会被发现和拦截。
* * *
x-waf（xiaomi waf）
##一，Nginx部分

>1. 编译安装[LuaJIT-2.0.2](http://luajit.org/download.html)。  
>2. 准备nginx[（Tengine）](http://tengine.taobao.org/)源码。  
>3. 下载[ngx_devel_kit](https://github.com/simpl/ngx_devel_kit) 解压。  
>4. 下载 [nginx_lua_module](https://github.com/chaoslawful/lua-nginx-module)解压。  
>5. 编译安装**pcre**。  
>6. 导入lua环境变量  
        export LUAJIT_LIB=/usr/local/lib 
        export LUAJIT_INC=/usr/local/include/luajit-2.0
>7. 编译nginx  
        ./configure --prefix=/opt/nginx \ #nginx的安装路径
        --add-module=/path/to/ngx_devel_kit \ #ngx_devel_kit 的源码路径
        --add-module=/path/to/lua-nginx-module #nginx_lua_module 的源码路径

##二，X-waf部分
>1. 下载x-waf文件。  
        git clone http://git.n.xiaomi.com/kangjingsong/x-waf.git  
>2. 将conf目录中内容拷贝到nginx的conf目录。将sbin目录内容copy到nginx的sbi目录。  
>3. 配置**xwaf.conf**中的**area**和**logpath**值。  
如果为新建area需要在此时通知webconsole管理员新建对应的area。  
>4. 配置**init.lua**中**logpath**和**rulepath**值。  
>5. 编译xwafclient，进入client目录执行make，将生成的**xwafclient**拷贝到nginx的**sbin**目录。  _该步骤会需要**mysql develop**包，请自行安装。_  
>6. 在nginx.conf中的**http**部**default_type**之后配置  
        init_by_lua_file  /opt/nginx/conf/init.lua;  
        access_by_lua_file /opt/nginx/conf/waf.lua;  
>7. 启动nginx，运行xwafclient。观察**syslog**无报错则启动成功。
