## Jenkins调用接口前准备
要需要对jenkins的安全做一些设置，依次点击系统管理-全局安全配置-授权策略，勾选"匿名用户具有可读权限"
操作步骤：
- [d] 系统管理-全局安全配置-授权策略，勾选"匿名用户具有可读权限"

- 1、关闭 CSRF
由于在调用 `Jenkins` 中，操作执行 `Job` 一些命令时会用到 `Post` 方式命令，
所以需要关闭 `Jenkins` 的 `CSRF` 选项。
>站点伪造保护(CSRF)请求必须要关闭，但是在Jenkins版本自2.2xx版本之后，在web界面里已经没法关闭了：


2.2xx版本之前操作步骤：
- [d] 关闭 系统管理->全局安全配置->跨站请求伪造保护 选项

2.2xx版本之后操作步骤：
- [d] 通过docker进入到对应Jenkins容器，在容器内进行Jenkins.sh文件编写
```shell
docker exec -u root  -it myjenkins  bash

apt-get update
apt-get install vim
vi /usr/local/bin/jenkins.sh
```

找到`exec java`那行(大概是在第37行)，添加`-Dhudson.security.csrf.GlobalCrumbIssuerConfiguration.DISABLE_CSRF_PROTECTION=true`
最终的效果如下：

```shell
exec java -Duser.home="$JENKINS_HOME" -Dhudson.security.csrf.GlobalCrumbIssuerConfiguration.DISABLE_CSRF_PROTECTION=true "${java_opts_array[@]}" -jar ${JENKINS_WAR} "${jenkins_opts_array[@]}" "$@"
```
重启jenkins容器使设置生效
```shell
[root@vms9 ~]# docker restart myjenkins
```
2、系统设置中和 jenkins 地址一致

操作步骤：
- [d] 设置 系统管理->系统设置->Jenkins Location 的 URL 和 Jenkins 访问地址保持一致