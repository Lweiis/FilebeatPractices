# Filebeat 安装

## 使用 rpm 文件安装
[filebeat 7.14.0 64位 rpm 下载链接](https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.14.0-x86_64.rpm)

[filebeat 7.14.0 32位 rpm 下载链接](https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.14.0-i686.rpm)

`rpm -ivh {filebeat 安装包文件路径}`

## 安装完后的配置
配置 systemctl 单元，开机自启动：`systemctl enable filebeat`

启动命令：`systemctl start filebeat`

停止命令：`systemctl stop filebeat`

重启命令：`systemctl restart filebeat`

## 默认路径
类型 | 默认路径(rpm 安装)
---- | ---- 
日志 | /var/log/filebeat/ 
config | /etc/filebeat/

## 配置文件修改
###备份默认配置文件

`cp /etc/filebeat/filebeat.yml /etc/filebeat/filebeat.yml.bak`

****这里注意，请确保 filebeat.yml 中有指定 modules 目录，如下：****

![modules.jpg](http://tva1.sinaimg.cn/large/6bed307bly1gw1pv0ceolj20g004sdh0.jpg)

###开启应用 kafka 模块

`filebeat modules enable kafka`

###修改 filebeat.yml
1. 新增一个 input 模块
   ![input.jpg](http://tva1.sinaimg.cn/large/6bed307bgy1gw1qazj9eoj20h80exgq5.jpg)
   
   1. 冒号后需要一个空格，配置项大小写敏感
   2. `multiline` 的作用是对存在换行的日志进行多行合并
      
      示例配置中，`pattern` 是匹配单条日志的开头位置的正则表达式，它代表 TIMESTAMP_ISO8601 开头的时间
   
      `negate: true` 与 `match: after` 的组合，会将不符合 `pattern` 的日志行，一直向上匹配至符合正则表达式的那一行为止
      
      关于 multiline 的更多配置，可以参考：[filebeat-input-multiline](https://www.elastic.co/guide/en/beats/filebeat/7.14/multiline-examples.html)
   3. ``paths`` 支持多个文件，也支持 glob 通配符，可参考：[filebeat-input-log](https://www.elastic.co/guide/en/beats/filebeat/7.14/filebeat-input-log.html)
    
2. 注释 filebeat.yml 中默认的 output.elasticsearch 模块
   
   filebeat 6 之后的版本仅支持单个 output
   
   注释默认的 output.elasticsearch：
   
   