# 设置签名

本地库范围内有效

```
git config user.name zhixiong

git config user.email chenzhixiong.me@gmail.com
```

系统用户级别：登录当前操作系统的用户范围

```
git config --global user.name zhixiong

git config --global user.email chenzhixiong.me@gmail.com
```

# 查看历史记录

```
git log
空格向下翻页
b 向上翻页
q 退出

git log -prety=oneline

git log -oneline

git reflog


```

# 前进后退

```
基于索引值操作[推荐]
git rest -hard [局部索引值]
git rest -hard a6ce91
删除文件并找回,局部索引值用HEAD

使用^符号：只能后退
git rest -hard HEAD^  注：一个^表示后退一步，n 个表示后退 n 步

使用~符号：只能后退
git rest -hard HEAD~n
注：表示后退 n 步

```

# 比较文件差异

```
git diff [文件名]
将工作区中的文件和暂存区进行比较

git diff [本地库中历史版本] [文件名]
将工作区中的文件和本地库历史记录比较
不带文件名比较多个文件
```

# 分支操作

```
创建分支
git branch [分支名]

查看分支
git branch -v  

切换分支
git checkout [分支名]

合并分支
    第一步：切换到接受修改的分支上
	git checkout [被合并分支名]
	
	第二步：执行 merge 命令
	git merge [有新内容分支名]

冲突的解决
 第一步：编辑文件，删除特殊符号
 第二步：把文件修改到满意的程度，保存退出
 第三步：git add [文件名]
 第四步：git commit -m "日志信息" 
注意：此时 commit 一定不能带具体文件名
```

# 创建远程库地址别名

```
git remote -v 查看当前所有远程地址别名
git remote ad [别名] [远程地址]

```

# 推送

```
git push [别名] [分支名]
```

# 拉取

```
pull=fetch+merg
git fetch [远程库地址别名] [远程分支名]
git merge [远程库地址别名/远程分支名]
git pull [远程库地址别名] [远程分支名]
```

# Fork

fork项目后,pull到本地,修改后push到远程仓库.可以发起pull request ,然后等项目管理人同意

# SSH 登录

1. 进入当前用户的家目录
   $ cd ~ 
2. 删除.sh 目录
   $ rm -rvf .sh
3. 运行命令生成.sh 密钥目录
   $ sh-keygn -t rsa -C [GitHub邮箱]
   [注意：这里-C 这个参数是大写的 C]
4. 进入.ssh 目录查看 id_rsa.pub 文件内容并复制到GitHub的SSH登陆管理
   $ cat id_rsa.pub
5. 回到 Git bash 创建远程地址别名
   git remote add [别名] [项目ssh的地址] 

