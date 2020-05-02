
# 通用配置

## 配置文件

一个完整的Flinkx任务脚本配置包含 content， setting两个部分。content用于配置任务的输入源与输出源，其中包含reader，writer。而setting则配置任务整体的环境设定，其中包含restore，speed，errorLimit，dirty，log。具体如下所示：

```json
{
  "job" : {
    "content" :[{
      "reader" : {
        ......
      },
      "writer" : {
        ......
      }
    }],
   "setting" : {
      "restore" : {
        ......
      },
      "speed" : {
        ......
      },
      "errorLimit" : {
        ......
      },
      "dirty" : {
        ......
      },
      "log" : {
        ......
      }
    }
  }
}
```

<table>
    <tr>
        <th colspan="2">名称</th>
        <th>说明</th>
        <th>是否必填</th>  
    </tr >
    <tr >
        <td rowspan="2">content</td>
        <td>reader</td>
        <td>reader插件详细配置</td>
        <td>是</td>
    </tr>
    <tr>
        <td>writer</td>
        <td>writer插件详细配置</td>
        <td>是</td>
    </tr>
    <tr>
        <td rowspan="5">setting</td>
        <td>restore</td>
        <td>任务类型及断点续传配置</td>
        <td>否</td>
    </tr>
  <tr>
      <td>speed</td>
      <td>速率限制</td>
      <td>否</td>
  </tr>
  <tr>
      <td>errorLimit</td>
      <td>出错控制</td>
      <td>否</td>
  </tr>
  <tr>
      <td>dirty</td>
      <td>脏数据保存</td>
      <td>否</td>
  </tr>
  <tr>
      <td>log</td>
      <td>日志记录配置</td>
      <td>否</td>
  </tr>
</table>

## content配置

### reader

reader用于配置数据的输入源，即数据从何而来。具体配置如下所示：

```json
"reader" : {
  "name" : "xxreader",
  "parameter" : {
    ......
  }
}
```

| 名称        | 说明                        | 是否必填 |
| --------- | ------------------------- | ---- |
| name      | reader插件名称，具体名称参考各数据源配置文档 | 是    |
| parameter | 数据源配置参数，具体配置参考各数据源配置文档    | 是    |

### writer

writer用于配置数据的输出源，即数据写往何处。具体配置如下所示：

```json
"writer" : {
  "name" : "xxwriter",
  "parameter" : {
    ......
  }
}
```

| 名称        | 说明                        | 是否必填 |
| --------- | ------------------------- | ---- |
| name      | writer插件名称，具体名称参考各数据源配置文档 | 是    |
| parameter | 数据源配置参数，具体配置参考各数据源配置文档    | 是    |

## setting配置

### restore

restore用于配置同步任务类型（离线同步、实时采集）和断点续传功能。具体配置如下所示：

```json
"restore" : {
  "isStream" : false,
  "isRestore" : false,
  "restoreColumnName" : "",
  "restoreColumnIndex" : 0,
  "maxRowNumForCheckpoint" : 10000
}
```

| 名称                     | 说明               | 是否必填      | 默认值   | 参数类型    |
| ---------------------- | ---------------- | --------- | ----- | ------- |
| isStream               | 是否为实时采集任务        | 否         | false | Boolean |
| isRestore              | 是否开启断点续传         | 否         | false | Boolean |
| restoreColumnName      | 断点续传字段名称         | 开启断点续传后必填 | 无     | string  |
| restoreColumnIndex     | 断点续传字段索引ID       | 开启断点续传后必填 | -1    | int     |
| maxRowNumForCheckpoint | 触发checkpoint数据条数 | 否         | 10000 | int     |

### speed

speed用于配置任务并发数及速率限制。具体配置如下所示：

```json
"speed": {
    "bytes": 1048576,
    "channel": 2,
    "rebalance": false,
    "readerChannel": 1,
    "writerChannel": 1
}
```

| 名称            | 说明                                                                                                                                          | 是否必填 | 默认值            | 参数类型    |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ---- | -------------- | ------- |
| bytes         | 每秒字节数                                                                                                                                       | 否    | Long.MAX_VALUE | long    |
| channel       | 通道数量                                                                                                                                        | 否    | 1              | int     |
| readerChannel | reader的并发数，配置此参数时会覆盖channel配置的并发数，不配置或配置为-1时将使用channel配置的并发数作为reader的并发数                                                                    | 否    | 无              | int     |
| writerChannel | writer的并发数，配置此参数时会覆盖channel配置的并发数，不配置或配置为-1时将使用channel配置的并发数作为writer的并发数                                                                    | 否    | 无              | int     |
| rebalance     | 此参数配置为true时将强制对reader的数据做Rebalance，不配置此参数或者配置为false时，程序会根据reader和writer的通道数选择是否Rebalance，reader和writer的通道数一致时不使用Reblance，通道数不一致时使用Reblance。 | 否    | 无              | boolean |

### errorLimit

errorLimit用于配置任务运行时数据读取写入的出错控制。具体配置如下所示：

```json
"errorLimit" : {
  "record": 100,
  "percentage": 10.0
}
```

| 名称         | 说明                       | 是否必填 | 默认值 | 参数类型   |
| ---------- | ------------------------ | ---- | --- | ------ |
| record     | 错误阈值，当错误记录数超过此阈值时任务失败    | 否    | 0   | int    |
| percentage | 错误比例阈值，当错误记录比例超过此阈值时任务失败 | 否    | 0.0 | double |

### dirty

dirty用于配置脏数据的保存，通常与上文出错控制联合使用。具体配置如下所示：

```json
"dirty" : {
  "path" : "xxx",
  "hadoopConfig" : {
    ......
  }
 }
```

| 名称           | 说明         | 是否必填 | 默认值 | 参数类型   |
| ------------ | ---------- | ---- | --- | ------ |
| path         | 脏数据保存路径    | 是    | 无   | string |
| hadoopConfig | Hadoop相关配置 | 是    | 无   | K-V键值对 |

参考模板如下：

```json
"dirty" : {
        "path" : "/user/hive/warehouse/xx.db/xx",
        "hadoopConfig" : {
          "fs.default.name": "hdfs://0.0.0.0:9000",
          "dfs.ha.namenodes.ns1" : "nn1,nn2",
          "dfs.namenode.rpc-address.ns1.nn1" : "0.0.0.0:9000",
          "dfs.namenode.rpc-address.ns1.nn2" : "0.0.0.1:9000",
          "dfs.client.failover.proxy.provider.ns1" : "org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider",
          "dfs.nameservices" : "ns1"
        }
      }
```

### log

log用于配置Flinkx中定义的插件日志的保存与记录。具体配置如下所示：

```json
"log" : {
  "isLogger": false,
  "level" : "info",
  "path" : "/tmp/dtstack/flinkx/",
  "pattern":""
}
```

| 名称       | 说明         | 是否必填 | 默认值                                                                                                                                                   |
| -------- | ---------- | ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| isLogger | 是否保存日志记录   | 否    | false                                                                                                                                                 |
| level    | 日志级别       | 否    | info                                                                                                                                                  |
| path     | 服务器上日志保存路径 | 否    | /tmp/dtstack/flinkx/                                                                                                                                  |
| pattern  | 日志输出格式     | 否    | log4j：%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{60} %X{sourceThread} - %msg%n logback : %d{yyyy-MM-dd HH:mm:ss,SSS} %-5p %-60c %x - %m%n |

# 插件使用文档

| 数据库                                                                                             | 文档                                                             | 文档                                                             |
| ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------- | -------------------------------------------------------------- |
| 关系数据库(MySQL,Oracle,Sqlserver,Postgresql,Db2,Gbase,PolarDB,ClickHouse,SAP Hana,Teradata,Phoenix) | [reader]()                                                     | [writer]()                                                     |
| HDFS                                                                                            | [reader](https://github.com/DTStack/flinkx/wiki/hdfsreader_CH) | [writer](https://github.com/DTStack/flinkx/wiki/hdfswriter_CH) |
| FTP                                                                                             | [reader](https://github.com/DTStack/flinkx/wiki/ftpreader_CH)  | [writer](https://github.com/DTStack/flinkx/wiki/ftpwriter_CH)  |
| Kudu                                                                                            | [reader]()                                                     | [writer]()                                                     |
| ElasticSearch                                                                                   | [reader]()                                                     | [writer]()                                                     |
| Hbase                                                                                           | [reader]()                                                     | [writer]()                                                     |
| Cassandra                                                                                       | [reader]()                                                     | [writer]()                                                     |
| ODPS                                                                                            | [reader]()                                                     | [writer]()                                                     |
| Carbondata                                                                                      | [reader]()                                                     | [writer]()                                                     |
| Redis                                                                                           | [reader]()                                                     |                                                                |
| Hive                                                                                            |                                                                | [writer]()                                                     |
| MongoDB                                                                                         | [reader]()                                                     | [writer]()                                                     |
| Kafka                                                                                           | [reader]()                                                     | [writer]()                                                     |
| EMQX                                                                                            | [reader]()                                                     | [writer]()                                                     |
| MySQL Binlog                                                                                    | [reader]()                                                     |                                                                |
