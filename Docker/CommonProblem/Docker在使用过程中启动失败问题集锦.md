
### 问题一

```
[root@itdeer ~]# systemctl start docker
Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.

```

```
[root@itdeer ~]# systemctl status docker

● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: failed (Result: exit-code) since Tue 2018-03-27 19:42:23 CST; 27s ago
     Docs: http://docs.docker.com
  Process: 5951 ExecStart=/usr/bin/dockerd-current --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current --default-runtime=docker-runc --exec-opt native.cgroupdriver=systemd --userland-proxy-path=/usr/libexec/docker/docker-proxy-current $OPTIONS $DOCKER_STORAGE_OPTIONS $DOCKER_NETWORK_OPTIONS $ADD_REGISTRY $BLOCK_REGISTRY $INSECURE_REGISTRY $REGISTRIES (code=exited, status=1/FAILURE)
 Main PID: 5951 (code=exited, status=1/FAILURE)

Mar 27 19:42:22 itdeer systemd[1]: Starting Docker Application Container Engine...
Mar 27 19:42:22 itdeer dockerd-current[5951]: time="2018-03-27T19:42:22.360667471+08:00" level=info msg="libcontainerd: new containerd process, pid: 5960"
Mar 27 19:42:23 itdeer dockerd-current[5951]: time="2018-03-27T19:42:23.378490201+08:00" level=warning msg="devmapper: Usage of loopback devices is strongly discouraged for production use. Please use `--storage-opt dm.thinpooldev` or use `man docker` to refer to dm.thinpooldev section."
Mar 27 19:42:23 itdeer dockerd-current[5951]: time="2018-03-27T19:42:23.446557154+08:00" level=error msg="[graphdriver] prior storage driver \"devicemapper\" failed: devmapper: Base Device UUID and Filesystem verification failed: devmapper: Current Base Device UUID:1d9c20aa-ff71-4627-a6cd-cb606f3f28f3 does not match with stored UUID:9fd2ca8d-9c43-4362-aae6-a296c8e8c92d. Possibly using a different thin pool than last invocation"
Mar 27 19:42:23 itdeer dockerd-current[5951]: time="2018-03-27T19:42:23.447288794+08:00" level=fatal msg="Error starting daemon: error initializing graphdriver: devmapper: Base Device UUID and Filesystem verification failed: devmapper: Current Base Device UUID:1d9c20aa-ff71-4627-a6cd-cb606f3f28f3 does not match with stored UUID:9fd2ca8d-9c43-4362-aae6-a296c8e8c92d. Possibly using a different thin pool than last invocation"
Mar 27 19:42:23 itdeer systemd[1]: docker.service: main process exited, code=exited, status=1/FAILURE
Mar 27 19:42:23 itdeer systemd[1]: Failed to start Docker Application Container Engine.
Mar 27 19:42:23 itdeer systemd[1]: Unit docker.service entered failed state.
Mar 27 19:42:23 itdeer systemd[1]: docker.service failed.
```

> 解决方式：

```
vim /data/docker/devicemapper/metadata/deviceset-metadata

{"next_device_id":1,"BaseDeviceUUID":"9fd2ca8d-9c43-4362-aae6-a296c8e8c92d","BaseDeviceFilesystem":"xfs"}
改为
{"next_device_id":1,"BaseDeviceUUID":"1d9c20aa-ff71-4627-a6cd-cb606f3f28f3","BaseDeviceFilesystem":"xfs"}
```

> 问题产生原因：

    因为更改了Docker默认的镜像存储地址，使用的是一块虚拟存储盘挂载到根目录下的/data。有一次服务器断电重启了，虚拟盘没有挂载上，当手动挂载之后，Docker服务是正常启动的，但是没有镜像，所以我把虚拟盘重新挂载上，然后重启了Docker。这时Docker启动报错，原因是虚拟盘的驱动UUID改变了和Docker记录的UUID值不一样，导致不能启动。

------


### 问题二

```
[root@itdeer ~]# systemctl start docker
Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.

```

```
[root@itdeer ~]# systemctl status docker.service

● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: failed (Result: exit-code) since Sat 2017-03-18 01:10:06 PDT; 2min 53s ago
     Docs: http://docs.docker.com
  Process: 3494 ExecStart=/usr/bin/dockerd-current --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current --default-runtime=docker-runc --exec-opt native.cgroupdriver=systemd --userland-proxy-path=/usr/libexec/docker/docker-proxy-current $OPTIONS $DOCKER_STORAGE_OPTIONS $DOCKER_NETWORK_OPTIONS $ADD_REGISTRY $BLOCK_REGISTRY $INSECURE_REGISTRY (code=exited, status=1/FAILURE)
 Main PID: 3494 (code=exited, status=1/FAILURE)

Mar 18 01:10:04 itdeer systemd[1]: Starting Docker Application Container Engine...
Mar 18 01:10:04 itdeer dockerd-current[3494]: time="2017-03-18T01:10:04.976205048-07:00" level=info msg="libcontainerd: new containerd process, pid: 3500"
Mar 18 01:10:06 itdeer dockerd-current[3494]: time="2017-03-18T01:10:06.011206081-07:00" level=warning msg="devmapper: Usage of loopback devices is strongly discouraged for production use. Please use `--storage-opt dm.thinpooldev` or use `man docker` to refer to dm.thinpooldev section."
Mar 18 01:10:06 itdeer dockerd-current[3494]: time="2017-03-18T01:10:06.022012288-07:00" level=warning msg="devmapper: Base device already exists and has filesystem xfs on it. User specified filesystem  will be ignored."
Mar 18 01:10:06 itdeer dockerd-current[3494]: time="2017-03-18T01:10:06.027998073-07:00" level=fatal msg="Error starting daemon: error initializing graphdriver: \"/var/lib/docker\" contains several valid graphdrivers: devicemapper, overlay; Please cleanup or explicitly choose storage driver (-s <DRIVER>)"
Mar 18 01:10:06 itdeer systemd[1]: docker.service: main process exited, code=exited, status=1/FAILURE
Mar 18 01:10:06 itdeer systemd[1]: Failed to start Docker Application Container Engine.
Mar 18 01:10:06 itdeer systemd[1]: Unit docker.service entered failed state.
Mar 18 01:10:06 itdeer systemd[1]: docker.service failed.
```

> 解决方式：

    删除/var/lib/docker/devicemapper下面的数据，重启docker服务即可，重启后/var/lib/docker/devicemapper里面的数据会重新生成。但是若这个目录中有很多数据，建议提前备份，之后再Copy回来。

> 问题产生原因：

    因为Docker不能初始化devicemapper

------

### 问题三

```
[root@itdeer ~]# systemctl start docker
Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.

```

```
[root@itdeer ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: failed (Result: exit-code) since Mon 2017-03-06 10:57:41 CST; 10s ago
     Docs: http://docs.docker.com
  Process: 48584 ExecStart=/usr/bin/dockerd-current --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current --default-runtime=docker-runc --exec-opt native.cgroupdriver=systemd --userland-proxy-path=/usr/libexec/docker/docker-proxy-current $OPTIONS $DOCKER_SE_OPTIONS $DOCKER_NETWORK_OPTIONS $ADD_REGISTRY $BLOCK_REGISTRY $INSECURE_REGISTRY (code=exited, status=1/FAILURE)
 Main PID: 48584 (code=exited, status=1/FAILURE)

Mar 06 10:57:41 itdeer dockerd-current[48584]: time="2017-03-06T10:57:41.033103724+08:00" level=warning msg="Docker could not enable SELinux on the host system"
Mar 06 10:57:41 itdeer dockerd-current[48584]: time="2017-03-06T10:57:41.038337006+08:00" level=info msg="Graph migration to content-addressability took 0.00 seconds"
Mar 06 10:57:41 itdeer dockerd-current[48584]: time="2017-03-06T10:57:41.038541983+08:00" level=warning msg="mountpoint for pids not found"
Mar 06 10:57:41 itdeer dockerd-current[48584]: time="2017-03-06T10:57:41.038684660+08:00" level=info msg="Loading containers: start."
Mar 06 10:57:41 itdeer dockerd-current[48584]: time="2017-03-06T10:57:41.043425594+08:00" level=info msg="Firewalld running: false"
Mar 06 10:57:41 itdeer dockerd-current[48584]: time="2017-03-06T10:57:41.111389603+08:00" level=fatal msg="Error starting daemon: Error initializing network controller: Error creating default \"bridge\" network: failed to parse pool request for address spa
Mar 06 10:57:41 itdeer systemd[1]: docker.service: main process exited, code=exited, status=1/FAILURE
Mar 06 10:57:41 itdeer systemd[1]: Failed to start Docker Application Container Engine.
Mar 06 10:57:41 itdeer systemd[1]: Unit docker.service entered failed state.
Mar 06 10:57:41 itdeer systemd[1]: docker.service failed.
Hint: Some lines were ellipsized, use -l to show in full.
You have new mail in /var/spool/mail/root
```

> 解决方式：

    在Docker配置文件/etc/sysconfig/docker “OPTIONS”中加入:–bip=172.17.42.1/16, 如下:
    # Modify these options if you want to change the way the docker daemon runs
    OPTIONS='--selinux-enabled --log-driver=journald --signature-verification=false --bip=172.17.42.1/16'


> 问题产生原因：

    因为Docker不能初始化devicemapper

------

### 问题四

```
[root@itdeer ~]# systemctl start docker
Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.

```

```
[root@itdeer ~]# systemctl status docker.service
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: failed (Result: exit-code) since 三 2016-10-19 02:58:23 UTC; 20s ago
     Docs: http://docs.docker.com
  Process: 10835 ExecStart=/usr/bin/docker-current daemon --exec-opt native.cgroupdriver=systemd $OPTIONS $DOCKER_STORAGE_OPTIONS $DOCKER_NETWORK_OPTIONS $ADD_REGISTRY $BLOCK_REGISTRY $INSECURE_REGISTRY (code=exited, status=1/FAILURE)
 Main PID: 10835 (code=exited, status=1/FAILURE)


10月 19 02:58:23 itdeer systemd[1]: Starting Docker Application Container Engine...
10月 19 02:58:23 itdeer docker-current[10835]: time="2016-10-19T02:58:23.880006578Z" level=fatal msg="can't create unix socket /var/run/docker.sock: is a directory"
10月 19 02:58:23 itdeer systemd[1]: docker.service: main process exited, code=exited, status=1/FAILURE
10月 19 02:58:23 itdeer systemd[1]: Failed to start Docker Application Container Engine.
10月 19 02:58:23 itdeer systemd[1]: Unit docker.service entered failed state.
10月 19 02:58:23 itdeer systemd[1]: docker.service failed.
```

> 解决方式：

    删除docker.sock目录,
    service docker start 启动docker服务

> 问题产生原因：


### 问题五

```
[root@itdeer ~]# systemctl start docker
Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service" and "journalctl -xe" for details.

```

```
[root@itdeer ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: failed (Result: exit-code) since Tue 2018-05-15 18:47:52 CST; 4s ago
     Docs: http://docs.docker.com
  Process: 13969 ExecStart=/usr/bin/dockerd-current --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current --default-runtime=docker-runc --exec-opt native.cgroupdriver=systemd --userland-proxy-path=/usr/libexec/docker/docker-proxy-current --init-path=/usr/libexec/docker/docker-init-current --seccomp-profile=/etc/docker/seccomp.json $OPTIONS $DOCKER_STORAGE_OPTIONS $DOCKER_NETWORK_OPTIONS $ADD_REGISTRY $BLOCK_REGISTRY $INSECURE_REGISTRY $REGISTRIES (code=exited, status=1/FAILURE)
 Main PID: 13969 (code=exited, status=1/FAILURE)

May 15 18:47:50 sundafei systemd[1]: Starting Docker Application Container Engine...
May 15 18:47:50 sundafei dockerd-current[13969]: time="2018-05-15T18:47:50.810609716+08:00" level=warning msg="could not change group /var/run/docker.sock to docker: group docker not found"
May 15 18:47:50 sundafei dockerd-current[13969]: time="2018-05-15T18:47:50.813820636+08:00" level=info msg="libcontainerd: new containerd process, pid: 13974"
May 15 18:47:51 sundafei dockerd-current[13969]: time="2018-05-15T18:47:51.823345223+08:00" level=warning msg="overlay2: the backing xfs filesystem is formatted without d_type support, which leads...
May 15 18:47:52 sundafei dockerd-current[13969]: Error starting daemon: SELinux is not supported with the overlay2 graph driver on this kernel. Either boot into a newer kernel or disab...abled=false)
May 15 18:47:52 sundafei systemd[1]: docker.service: main process exited, code=exited, status=1/FAILURE
May 15 18:47:52 sundafei systemd[1]: Failed to start Docker Application Container Engine.
May 15 18:47:52 sundafei systemd[1]: Unit docker.service entered failed state.
May 15 18:47:52 sundafei systemd[1]: docker.service failed.
Hint: Some lines were ellipsized, use -l to show in full.
```

> 解决方式：

    关闭SELinux即可

```
[root@sundafei ~]# vim /etc/selinux/config 

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected. 
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted 

```  

> 问题产生原因：

------