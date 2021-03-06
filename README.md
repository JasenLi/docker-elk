### 环境依赖  
首选准备2台服务器以备使用es01、es02 、es03  

##### 安装docker（es01、es02、es03）  
如果你已安装可跳过此步骤  
bash docker.install  

##### 安装shyaml（es01、es02、es03）
如果你已安装可跳过此步骤  
bash compose.install  

##### 启动docker（es01、es02、es03）    
service/systemctl start docker  

### 修改配置文件  
修改elastic/public.yml  

### 证书处理  
##### 证书生成（es01）  
进入安装目录  
修改elastic/instances.yml（证书/hosts文件 生成依赖）文件备用  
执行bash certs  
等待证书生成...........  
生成的证书文件在elastic/certs文件夹中    
`证书生成一次就好`  

##### 环境同步（es02、es03）  
将上一步证书生成后的项目同步到es02、es03，使其保持一致  

### ElasticSearch运行  
##### 依次启动环境（es01、es02、es03）  
bash es  

##### 直接访问  
直接在浏览器上访问elastic https://es01:9200
查看集群健康度：https://es01:9200/_cluster/health?pretty
惊不惊喜，意不意外，就是这么简单

### Kibana运行  
##### 运行（任意一台）  
bash kib  
直接在浏览器上访问kibana http://es01:5601  
惊不惊喜，意不意外，就是这么简单  

### Dockerfile中已经对Xpack进行了破解处理，有兴趣的可以去看看，直接在Kibana上传修改后的证书即可得到铂金会员  
##### 去官网申请license证书  
https://license.elastic.co/registration  
主要修改这几个地方  
1."type":"basic" 替换为 "type":"platinum"  
2."expiry_date_in_millis":1561420799999 替换为 "expiry_date_in_millis":3107746200000  

### 运行Filebeat+Logstash
##### 运行（任意一台）  
bash log  
bash file  
启动成功后，默认会处理filebeat/logs/nginx文件夹下的两份日志文件，之后可以通过kibana进行各种数据分析操作  
惊不惊喜，意不意外，就是这么简单...  
