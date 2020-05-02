[English](https://github.com/DTStack/flinkx/wiki/ftpreader) | 中文

# FTP Reader

# 插件名称

名称：**ftpreader**

# 数据源版本

| 协议   | 是否支持 |
| ---- | ---- |
| FTP  | 支持   |
| SFTP | 支持   |

# 数据源配置

FTP服务搭建

    windows：[地址](https://help.aliyun.com/document_detail/92046.html?spm=a2c4g.11186623.6.1185.6371dcd5DOfc5z)

    linux：[地址](https://help.aliyun.com/document_detail/92048.html?spm=a2c4g.11186623.6.1184.7a9a2dbcRLDNlf)

sftp服务搭建

    windows：[地址](http://www.freesshd.com/)

    linux：[地址](https://yq.aliyun.com/articles/435356?spm=a2c4e.11163080.searchblog.102.576f2ec1BVgWY7)

# 参数说明

| 名称                | 类型      | 说明                                                                                                  | 是否必填 | 默认值                 |
| ----------------- | ------- | --------------------------------------------------------------------------------------------------- | ---- | ------------------- |
| host              | string  | FTP服务器地址                                                                                            | 是    | 无                   |
| port              | int     | FTP服务端口                                                                                             | 否    | FTP：21<br />SFTP：22 |
| username          | string  | 连接FTP的用户名                                                                                           | 是    | 无                   |
| password          | string  | 密码，当使用私钥的时候，密码可不填                                                                                   | 否    | 无                   |
| privateKeyPath    | string  | 私钥文件路径                                                                                              | 否    | 无                   |
| protocol          | string  | FTP协议类型，可选：ftp，sftp                                                                                 | 是    | 无                   |
| connectPattern    | string  | 协议为ftp时的连接模式，可选pasv，port，参数含义可参考：[模式说明](https://blog.csdn.net/qq_16038125/article/details/72851142) | 是    | 无                   |
| path              | string  | 文件路径，多个路径用逗号分隔填写                                                                                    | 是    | 无                   |
| fieldDelimiter    | string  | 字段分隔符                                                                                               | 否    | 英文','               |
| encoding          | string  | 字符编码                                                                                                | 否    | UTF-8               |
| isFirstLineHeader | boolean | 文件的第一行是不是表头，设置为true时第一行数据会跳过                                                                        | 否    | false               |
| timeout           | int     | 连接超时时间，单位毫秒                                                                                         | 否    | 5000                |

# 使用示例

### 读取单个文件

```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "parameter": {
                        "path": "/data/ftp/flinkx/file1.csv",
                        "protocol": "sftp",
                        "port": 22,
                        "isFirstLineHeader": true,
                        "host": "localhost",
                        "column": [
                            {
                                "index": 0,
                                "type": "string"
                            },
                            {
                                "index": 1,
                                "type": "string"
                            },
                            {
                                "index": 2,
                                "type": "int"
                            },
                            {
                                "index": 3,
                                "type": "int"
                            }
                        ],
                        "password": "pass",
                        "fieldDelimiter": ",",
                        "encoding": "utf-8",
                        "username": "user"
                    },
                    "name": "ftpreader"
                },
                "writer": {
                    "parameter": {},
                    "name": "streamwriter"
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

### 读取单个目录下的所有文件

```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "parameter": {
                        "path": "/data/ftp/flinkx/dir1",
                        "protocol": "sftp",
                        "port": 22,
                        "isFirstLineHeader": true,
                        "host": "localhost",
                        "column": [
                            {
                                "index": 0,
                                "type": "string"
                            },
                            {
                                "index": 1,
                                "type": "string"
                            },
                            {
                                "index": 2,
                                "type": "int"
                            },
                            {
                                "index": 3,
                                "type": "int"
                            }
                        ],
                        "password": "pass",
                        "fieldDelimiter": ",",
                        "encoding": "utf-8",
                        "username": "user"
                    },
                    "name": "ftpreader"
                },
                "writer": {
                    "parameter": {},
                    "name": "streamwriter"
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

### 读取多个路径下的文件

```json
{
    "job": {
        "content": [
            {
                "reader": {
                    "parameter": {
                        "path": "/data/ftp/flinkx/dir1,/data/ftp/flinkx/dir2",
                        "protocol": "sftp",
                        "port": 22,
                        "isFirstLineHeader": true,
                        "host": "localhost",
                        "column": [
                            {
                                "index": 0,
                                "type": "string"
                            },
                            {
                                "index": 1,
                                "type": "string"
                            },
                            {
                                "index": 2,
                                "type": "int"
                            },
                            {
                                "index": 3,
                                "type": "int"
                            }
                        ],
                        "password": "pass",
                        "fieldDelimiter": ",",
                        "encoding": "utf-8",
                        "username": "user"
                    },
                    "name": "ftpreader"
                },
                "writer": {
                    "parameter": {},
                    "name": "streamwriter"
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
