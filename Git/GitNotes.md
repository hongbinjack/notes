### 第一章 git初始化

#### 1.1 什么是git

Git是一个分布式的版本控制软件

- 软件

- 版本控制

- 分布式

  1.文件夹拷贝

  2.本地版本控制

  3.集中式版本控制

#### 1.2 为什么要做版本控制

要保留之前所有的版本，以便回滚和修改

#### 

## 第二章“东北热”创业史

#### 2.1 第一阶段：单枪匹马开始干

想要Git对一个目录进行版本控制需要以下步骤：

- 进入要管理的文件夹
- 执行初始化命令

```
git init
```

- 管理目录下的文件状态

```
git status
注：新增的文件和修改过后的文件都是红色
```

- 管理指定文件（红变绿）

```
git add 文件名
git add .
```

- 个人信息配置：用户名、邮箱【一次即可】

```
git config --global user.email "you@example.com"
git config --global user.name "My name"
```

- 生成版本

```
git commit -m "描述信息"
```

- 查看版本记录

```
git log
```

#### 2.2 第二阶段：拓展新功能

```
git add
git commit -m "短视频"
```

#### 2.3 第三阶段：“约饭事件”

- 混滚至之前的版本

```
git log
git reset --hard 版本号
```



#### 2.4 小总结

```
git init
git add
git commit
git log
git reset --hard 版本号
```





![image](https://user-images.githubusercontent.com/106834223/218461832-9df67a41-4bac-401d-9419-7114c6e39833.png)





## 2.5 第四阶段：商城&紧急修复bug



#### 2.5.1 分支

分支可以给使用者提供多个环境的可能，意味着可以把我们的工作从开发主线上分离开来，以免影响开发主线。

#### 2.5.2 紧急修复bug方案

![image](https://user-images.githubusercontent.com/106834223/218463810-d0b87a3c-6f78-471e-9aaf-b6123237805f.png)



#### 2.5.3 命令总结

- 查看分支

```
git branch
```

- 创建分支

```
git branch 分支名称
```

- 切换分支

```
git checkout 分支名称
```

- 分支合并(可能产生冲突)

```
git merge 要合并的分支

注意：切换分支再合并
```

- 删除分支

```
git branch -d 分支名称
```



#### 2.5.4 工作流

![image](https://user-images.githubusercontent.com/106834223/218464709-5e99873f-e78f-48f4-a2f5-f6d68080d29e.png)





## 第五阶段：进军三里屯

有钱之后一个人在三里屯买了一层楼做办公室

![image](https://user-images.githubusercontent.com/106834223/218464997-d61d3f59-e23d-4b46-8c77-45cc9a7c9c56.png)



#### 2.6.1 第一天上班前在家上传代码

在家创建一个GitHub的repository

```
1.给远程仓库起别名
   git remote add origin 远程仓库地址
   
2.向远程仓库推送代码
   git push -u origin 分支名
```

#### 2.6.2 初次在公司新电脑下载代码

```
1.克隆远程仓库代码
   git clone 远程仓库地址(内部已实现git remote add origin 远程仓库地址)
   
2.切换分支
   git checkout 分支名
```

在公司下载完代码后，继续开发

```
1.切换到dev分支进行开发
    git checkout dev
2.把master分支合并到dev [仅一次]
    git merge master
3.修改代码
4.提交代码 
    git add .
    git commit -m '本次新代码的描述信息'
    git push origin dev
```

#### 2.6.3 下班回家继续写代码

```
1.切换到dev分支进行开发
    git checkout dev
2.拉代码
    git pull origin dev
3.继续开发
4.提交代码
    git add .
    git commit -m '本次提交代码的描述信息'
    git push origin dev
```



#### 2.6.4 到公司继续开发

```
1.切换到dev分支进行开发
    git checkout dev
2.拉去最新代码(不必再clone，只需要通过pull获取最新代码即可)
    git pull origin dev
3.继续开发
4.提交代码
    git add .
    git commit -m '本次提交代码的描述信息'
    git push origin dev

```



开发完毕要上线

```
1.将dev分支合并到master，进行上线
    git checkout master
    git merge dev
    git push origin master

2.把dev分支也推送到远程
    git checkout dev
    git merge master
    git push origin dev

```



#### 在公司忘记提交代码

```
1.拉代码
    git pull origin dev
2.继续开发

3.提交代码
    git add .
    git commit -m '本次提交代码的描述信息'

注：我忘记push了

```



#### 2.6.6 回家继续写代码

```
1. 拉代码，发现公司写的代码忘记提交.......
    git pull origin dev

2.继续开发其他功能

3.把dev分支也推送到远程
    git add .
    git commit -m '本次代码的描述信息'
    git push origin dev

```



#### 2.6.7 到公司继续写代码

```
1.拉代码，把晚上在家写的代码拉到本地(有合并、可能产生冲突)
    git pull origin dev
    
2.如果有冲突，手动解决冲突

2.继续开发其他功能

4.把dev分支也推送扫远程
    git add .
    git commit -m '本次代码的描述信息'
    git push origin dev

```



#### 2.6.8 其他

```
git pull origin dev
等价于
git fetch origin dev
git merge origin/dev

```



![image](https://user-images.githubusercontent.com/106834223/218470985-2b1a923e-c19c-48ad-ab17-299dbb4f8909.png)



#### 2.6.9 rebase的作用？

rebase可以保持提交记录简洁，不分叉。



#### 2.6.10 快速解决冲突

1.安装beyond compare

2.再git中配置

```
git config --local merge.tool bc3
git config --local mergetool.path '/usr/local/bin/bcomp'
git config --local mergetool.keepBackup false

```

3.应用beyond compare解决冲突

```
git mergetool

```



#### 2.7 小总结

- 添加远程连接(别名)

```
git remote add origin 远程仓库地址

```

- 推送代码






















