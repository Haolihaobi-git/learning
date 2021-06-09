# linux的基本操作命令
### 1，如何删除一个目录里除一个文件外的所有文件

- shopt -s extglob      （打开extglob模式）
- rm -fr !(file1) 
- 如果是多个要排除的，可以这样：
-  rm -rf !(file1|file2)   

### 2，如何关闭防火墙

![1572336878080](C:\Users\好里好比\AppData\Roaming\Typora\typora-user-images\1572336878080.png)

### 3，安装jdk

```
yum install java-1.8.0-openjdk* -y
```

### 4，复制

win下使用scp user@ip:/path localpath 前面是linux的用户, ip, 以及文件路经, 后面是win下保存的路径

# 5，免密登录

1，ssh-keygen -t rsa 生成秘钥

2，在.ssh生成的pub复制到authorized_keys