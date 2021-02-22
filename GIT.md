# GIT

### git和svn

svn：集中式版本控制工具，版本库集中放在中央服务器。必须联网才能工作

git：分布式版本控制工具，没有中央服务器，每个人的电脑就是一个完整的版本库

<img src="C:\Users\yk\AppData\Roaming\Typora\typora-user-images\image-20201221154047874.png" alt="image-20201221154047874" style="zoom: 80%;" />

### git的常用命令流程图

![image-20201221154522610](C:\Users\yk\AppData\Roaming\Typora\typora-user-images\image-20201221154522610.png)

### 创建ssh密钥和公钥

在git命令行窗口输入

```
ssh-keygen -r rsa
```

生成在当前用户下的.ssh目录

**id_rsa:**私钥

**id_rsa.pub**:公钥

### HTTPS和SSH

https将本地仓库推送到远程使用用户名和密码的方式验证

ssh使用私钥和公钥验证，私钥加密，公钥解密

### 将本地文件提交到GitHub

1.将文件夹变成git可管理的仓库

```
git init
```

2.将文件夹中的所有文件提交到暂存区

```
git add .
```

3.将暂存区的文件提交到本地仓库

```
git commit -m "注释"
```

4.连接远程仓库

```
git remote add origin 
```

5.将本地仓库中的代码全部推送到远程仓库

```
git push -u origin master
```

