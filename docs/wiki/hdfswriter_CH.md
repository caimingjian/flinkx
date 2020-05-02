[English](https://github.com/DTStack/flinkx/wiki/hdfswriter) | 中文

# HDFS Writer

# 插件名称
名称：**hdfswriter**<br />

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
| encoding | string | fileType为text时可配置编码格式 | 否 | utf-8 |
| fileType | string | 文件存储格式 | 否 | text |
| fieldDelimiter | string | fileType为text时，字段的分隔符 | 否 | "\001" |
| rowGroupSize | int | fileType为parquet时，指定的rowGroupSize大小 | 否 | 134217728 |
| maxFileSize | long | 产生的文件大小 | 否 | 1073741824 |
| compress | string | 压缩格式，fileType为text时可选：GZIP，BZIP2，NONE，fileType为orc时可选：SNAPPY，GZIP，BZIP，LZ4，NONE，fileType为parquet时可选：SNAPPY，GZIP，LZO，NONE | 否 | 无 |
| fileName | string | 写入的目录名称 | 否 | 无 |
| fullColumnName | array | 全部字段名称数组 | 否 | 无 |
| fullColumnType | array | 全部字段类型数组 | 否 | 无 |
| writeMode | string | 写入模式，可选：append，overwrite | 否 | 无 |



# 使用示例
### 写入text文件
```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "parameter": {
                        "column": [
                            {
                                "name": "col1",
                                "type": "string"
                            },
                            {
                                "name": "col2",
                                "type": "string"
                            },
                            {
                                "name": "col3",
                                "type": "int"
                            },
                            {
                                "name": "col4",
                                "type": "int"
                            }
                        ],
                        "sliceRecordCount": [
                            "100"
                        ]
                    },
                    "name": "streamreader"
                },
                "writer": {
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
                        "fileType": "text",
                        "writeMode": "append"
                    },
                    "name": "hdfswriter"
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

### 写入orc文件
```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "parameter": {
                        "column": [
                            {
                                "name": "col1",
                                "type": "string"
                            },
                            {
                                "name": "col2",
                                "type": "string"
                            },
                            {
                                "name": "col3",
                                "type": "int"
                            },
                            {
                                "name": "col4",
                                "type": "int"
                            }
                        ],
                        "sliceRecordCount": [
                            "100"
                        ]
                    },
                    "name": "streamreader"
                },
                "writer": {
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
                        "fileType": "orc",
                        "writeMode": "append"
                    },
                    "name": "hdfswriter"
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

### 写入parquet文件
```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "parameter": {
                        "column": [
                            {
                                "name": "col1",
                                "type": "string"
                            },
                            {
                                "name": "col2",
                                "type": "string"
                            },
                            {
                                "name": "col3",
                                "type": "int"
                            },
                            {
                                "name": "col4",
                                "type": "int"
                            }
                        ],
                        "sliceRecordCount": [
                            "100"
                        ]
                    },
                    "name": "streamreader"
                },
                "writer": {
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
                        "fileType": "parquet",
                        "writeMode": "append"
                    },
                    "name": "hdfswriter"
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

# 性能指标
暂无
