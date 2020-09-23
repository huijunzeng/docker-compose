### Jenkins持续集成工具部署

假如启动报以下错误：
```
jenkins       | touch: cannot touch '/var/jenkins_home/copy_reference_file.log': Permission denied
jenkins       | Can not write to /var/jenkins_home/copy_reference_file.log. Wrong volume permissions?
```
![启动报错](https://github.com/huijunzeng/docker-compose/blob/master/images/jenkins/dfa7c800d0f9a57ab95957d4f34734f.png)

则需对容器内的root用户授权，在docker-compose.xml文件加上以下内容：  
``` 
#授权容器内用户root超管权限 
privileged: true
user: root 
```

***
启动成功后，访问控制台地址(ip:port)
![控制台访问页面](https://github.com/huijunzeng/docker-compose/blob/master/images/jenkins/1600839160.jpg)
![控制台访问页面](https://github.com/huijunzeng/docker-compose/blob/master/images/jenkins/1600839235.jpg)  
首次访问可能加载缓慢，然后需要输入密码，进入到挂在卷的路径下：  
比如案例中的```/media/blog/jenkins/home/secrets```路径下复制initialAdminPassword文件密码即可访问成功，然后按照新手入门教程安装相关插件！
