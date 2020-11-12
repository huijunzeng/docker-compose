### ELK+Filebeat日志服务器

假如启动Elasticsearch报以下错误：

(一)
```
elasticsearch    | Error: Could not create the Java Virtual Machine.
elasticsearch    | Error: A fatal exception has occurred. Program will exit.
elasticsearch    | [0.001s][error][logging] Error opening log file 'logs/gc.log': Permission denied
elasticsearch    | [0.001s][error][logging] Initialization of output 'file=logs/gc.log' using options 'filecount=32,filesize=64m' failed.
```
解决办法：因为挂载卷的访问权限不足(针对需要写操作的文件)，所以我们需要给Elasticsearch挂载卷授权，进入到`/media/elk/elasticsearch`分别给三个挂载卷授权：
```
chmod 777 conf data logs
``` 

(二)
```
max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```
解决办法：因为用户拥有的内存权限太小，至少需要262144；切换到root超管，进入到/etc目录，执行编辑vim sysctl.conf，在最末添加属性行vm.max_map_count=262144，保存退出后执行加载系统参数命令sudo sysctl -p，然后输入命令sysctl -a|grep vm.max_map_count即可看到修改后的配置值，最后重新启动即可

(三)
```
java.nio.file.AccessDeniedException: /usr/share/elasticsearch/data/nodes/0/node.lock
```
解决办法：把Elasticsearch的挂载数据卷data路径下的数据删掉(索引数据需要定时清理，不然磁盘空间不足)
