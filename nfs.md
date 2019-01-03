## nfs 

### 服务端

1.查看是否安装

```shell
rpm qa|grep  nfs
rpm qa|grep  rpcbind
```

2.安装nfs rpcbind

```shell
yum  -y   install nfs-util,rpcbind
```

3. 设置共享

   vim  /etc/exports

   /gx_apps         192.168.1.227(rw,no_root_squash,sync)

4.重启服务

service  nfs   restart

5.查看

```shell
[root@localhost /]# exportfs   -rv
exporting 192.168.1.227:/gx_apps
```

6.设置开机启动

 chkconfig nfs on  







### 客户端

1.安装rpcbind   设置开机启动

2.查看共享信息

Showmount –e 192.168.1.235

3.挂载

mount -t nfs  192.168.41.220:/gx_apps   /gx_apps  

mount -t nfs 192.168.1.227:/share/gx_apps_235/gx_apps     /gx_apps

 4.df查看挂载情况







#### 卸载

 umount  /gx_apps

 umount  -rf  /gx_apps  //强制卸载

```
 fuser -m /gx_apps             //实在不行杀进程方式
```









