# k8s-elk-nfs
1.部署elk
部署elk的yaml文件都在附件中，唯一需要注意的是持久化nfs的路径需要存在，如代码中的/public/logs,/public/logstash,/public/elasticsearch。
logs是日志的存放路径；logstash是配置文件存放路径，即附加中的container.conf；elasticsearch是持久化的路径

2.Yaml文件说明
在部署elk的时候使用到了三个yaml文件，这几个文件中elasticsearch.yaml和logstash.yaml有一些需要注意的地方，进行一下说明：
Elasticsearch：这里对保存到elasticsearch的数据进行了nfs持久化，所以对于不同的服务器需要修改不同的server地址，即第22行的ip地址
Logstash：logstash使用了两个持久化路径，所以也需要修改为nfs的server地址，即第16行和49行；由于设置了logstash自动读取日志路径下的文件，在第89行中有--config.reload.automatic 30，这里的意思是每隔30秒读取一次，如果有新增的日志产生会自动加载，这里可以根据需要修改读取间隔时间。

3.配置文件说明
在elk中配置文件使用到的是container.conf，在这个文件中包含两部分，input和output。
Input：是logstash读取日志的地方，这里的path路径是容器内的路径，不需要外部创建。
Output：是logstash将日志输出到的地方，即elasticsearch

