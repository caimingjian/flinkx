[English](https://github.com/DTStack/flinkx/wiki/hdfsreader) | 中文

# HDFS Reader

# 插件名称
名称：**hdfsreader**<br />

# 数据源版本
| 协议 | 是否支持 |
| --- | --- |
| Hadoop 2.x | 支持 |
| Hadoop 3.x | 支持 |



# 数据源配置
单机模式：[地址](http://hadoop.apache.org/docs/r2.7.6/hadoop-project-dist/hadoop-common/SingleCluster.html)<br />集群模式：[地址](http://hadoop.apache.org/docs/r2.7.6/hadoop-project-dist/hadoop-common/ClusterSetup.html)<br />

# 参数说明
| 名称 | 类型 | 说明 | 是否必填 | 默认值 |
| --- | --- | --- | --- | --- |
| defaultFs | string | Hadoop core-site.xml配置中的fs.defaultFS | 是 | 无 |
| hadoopConfig | map | 集群HA模式时需要填写的namespace配置及其它配置 | 否 | 无 |
| path | string | 数据文件的路径 | 是 | 无 |
| filterRegex | string | 文件过滤正则表达式 | 否 | 无 |
| fileType | string | 文件存储格式 | 否 | text |
| fieldDelimiter | string | fileType为text时，字段的分隔符 | 否 | "\001" |



# 使用示例
### 读取text文件
```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "parameter": {
                        "path": "hdfs://ns1/flinkx/text",
                        "defaultFS": "hdfs://ns1",
                        "hadoopConfig": {
                            "dfs.ha.namenodes.ns1": "nn1,nn2",
                            "dfs.namenode.rpc-address.ns1.nn2": "flinkx02:9000",
                            "dfs.client.failover.proxy.provider.ns1": "org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider",
                            "dfs.namenode.rpc-address.ns1.nn1": "flinkx01:9000",
                            "dfs.nameservices": "ns1"
                        },
                        "column": [
                            {
                                "name": "col1",
                                "index": 0,
                                "type": "string"
                            },
                            {
                                "name": "col2",
                                "index": 1,
                                "type": "string"
                            },
                            {
                                "name": "col3",
                                "index": 2,
                                "type": "int"
                            },
                            {
                                "name": "col4",
                                "index": 3,
                                "type": "int"
                            }
                        ],
                        "fieldDelimiter": ",",
                        "fileType": "text"
                    },
                    "name": "hdfsreader"
                },
                "writer": {
                    "parameter": {},
                    "name": "streamwriter"
                }
            }
        ],
        "setting": {
            "speed": {
                "bytes": 0,
                "channel": 1
            }
        }
    }
}
```

### 过滤文件名称
```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "parameter": {
                        "path": "hdfs://ns1/flinkx/text",
                        "filterRegex" : ".*\\.csv",
                        "defaultFS": "hdfs://ns1",
                        "hadoopConfig": {
                            "dfs.ha.namenodes.ns1": "nn1,nn2",
                            "dfs.namenode.rpc-address.ns1.nn2": "flinkx02:9000",
                            "dfs.client.failover.proxy.provider.ns1": "org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider",
                            "dfs.namenode.rpc-address.ns1.nn1": "flinkx01:9000",
                            "dfs.nameservices": "ns1"
                        },
                        "column": [
                            {
                                "name": "col1",
                                "index": 0,
                                "type": "string"
                            },
                            {
                                "name": "col2",
                                "index": 1,
                                "type": "string"
                            },
                            {
                                "name": "col3",
                                "index": 2,
                                "type": "int"
                            },
                            {
                                "name": "col4",
                                "index": 3,
                                "type": "int"
                            }
                        ],
                        "fieldDelimiter": ",",
                        "fileType": "text"
                    },
                    "name": "hdfsreader"
                },
                "writer": {
                    "parameter": {},
                    "name": "streamwriter"
                }
            }
        ],
        "setting": {
            "speed": {
                "bytes": 1048576,
                "channel": 1
            }
        }
    }
}
```


# 性能指标
暂无