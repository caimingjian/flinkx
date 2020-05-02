[English](https://github.com/DTStack/flinkx/wiki/ftpwriter) | 中文

# FTP Writer

# 插件名称

名称：**ftpwriter**

# 数据源版本

| 协议   | 是否支持 |
| ---- | ---- |
| FTP  | 支持   |
| SFTP | 支持   |

# 数据源配置

FTP服务搭建

windows：[地址

linux：[地址](https://help.aliyun.com/document_detail/92048.html?spm=a2c4g.11186623.6.1184.7a9a2dbcRLDNlf)

sftp服务搭建

windows：[地址](http://www.freesshd.com/)

linux：[地址](https://yq.aliyun.com/articles/435356?spm=a2c4e.11163080.searchblog.102.576f2ec1BVgWY7)



# 参数说明

| 名称             | 类型     | 说明                                                                                                  | 是否必填 | 默认值                 |
| -------------- | ------ | --------------------------------------------------------------------------------------------------- | ---- | ------------------- |
| host           | string | FTP服务器地址                                                                                            | 是    | 无                   |
| port           | int    | FTP服务端口                                                                                             | 否    | FTP：21<br />SFTP：22 |
| username       | string | 连接FTP的用户名                                                                                           | 是    | 无                   |
| password       | string | 密码，当使用私钥的时候，密码可不填                                                                                   | 否    | 无                   |
| privateKeyPath | string | 私钥文件路径                                                                                              | 否    | 无                   |
| protocol       | string | FTP协议类型，可选：ftp，sftp                                                                                 | 是    | 无                   |
| connectPattern | string | 协议为ftp时的连接模式，可选pasv，port，参数含义可参考：[模式说明](https://blog.csdn.net/qq_16038125/article/details/72851142) | 是    | 无                   |
| path           | string | 文件路径，多个路径用逗号分隔填写                                                                                    | 是    | 无                   |
| fieldDelimiter | string | 字段分隔符                                                                                               | 否    | 英文','               |
| encoding       | string | 字符编码                                                                                                | 否    | UTF-8               |
| writeMode      | string | 写入模式，可选：overwrite，append                                                                            | 是    | 无                   |
| maxFileSize    | int    | 单个文件大小，单位为字节                                                                                        | 否    | 1073741824          |
| timeout        | int    | 连接超时时间，单位毫秒                                                                                         | 否    | 5000                |

# 使用示例

### append模式写入

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
                        "path": "/data/ftp/flinkx",
                        "protocol": "sftp",
                        "port": 22,
                        "writeMode": "append",
                        "host": "localhost",
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
                        "password": "pass",
                        "fieldDelimiter": ",",
                        "encoding": "utf-8",
                        "username": "user"
                    },
                    "name": "ftpwriter"
                }
            }
        ],
        "setting": {
            "restore": {
                "maxRowNumForCheckpoint": 0,
                "isRestore": false,
                "restoreColumnName": "",
                "restoreColumnIndex": 0
            },
            "errorLimit": {
                "record": 100
            },
            "speed": {
                "bytes": 0,
                "channel": 1
            }
        }
    }
}
```

### 指定文件大小

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
                            "0"
                        ]
                    },
                    "name": "streamreader"
                },
                "writer": {
                    "parameter": {
                        "path": "/data/ftp/flinkx",
                        "protocol": "sftp",
                        "port": 22,
                        "writeMode": "append",
                        "host": "localhost",
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
                        "password": "pass",
                        "fieldDelimiter": ",",
                        "encoding": "utf-8",
                        "username": "user",
                        "maxFileSize" : 5242880
                    },
                    "name": "ftpwriter"
                }
            }
        ],
        "setting": {
            "restore": {
                "maxRowNumForCheckpoint": 0,
                "isRestore": false,
                "restoreColumnName": "",
                "restoreColumnIndex": 0
            },
            "errorLimit": {
                "record": 100
            },
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
