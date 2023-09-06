## 一、本地已有文件，推到远程git仓库

在本地文件夹里初始化

``` shell
git init
git add .
git commit -m ""
git remote add origin <地址>
git pull origin master
# 或者
git pull origin master --allow-unrelated-histories
# pull远程仓库内容，更新本地仓库，使用--allow-unrelated-histories忽略本地仓库和远程仓库的无关性，强行合并（关键）
git push -u origin master
```

## 二、拉取远程新建分支

```shell
git checkout -b <本地分支名> origin/<远程分支名>
```

或者

```shell
git chechout -b <本地分支名>

git pull origin <远程分支名>
# 或
git branch --set-upstream-to=origin/<远程分支名> <本地分支名>
```

## 三、查看本地分支与远程分支关系

```shell
git branch -vv
```

## 四、删除分支

先切到别的分支上：

```bash
git branch -d <分支名称>
```

如果删不了，强行删除

```bash
git branch -D <分支名称>
```

## 五、用户名+密码管理

- 重置用户名+密码

```bash
git config --system --unset credential.helper
```

- 存储用户名+密码

```bash
git config --global credential.helper store
git pull
然后就会要求输入用户名(邮箱)+密码，git就会保存了
```



## 六、回退代码

### 1.代码回退

- 查询版本

```bash
git log
```

- 执行回退到上一版本

```bash
git reset --hard <commitID>
```

git pull，重新回到最新版本。

- 强制将本地代码推到远程仓库

```bash
git push -f origin <分支名称>
```

### 2.提错分支

切到错误分支

~~~bash
git checkout <错误分支>
~~~

提交记录查询

~~~bash
git log
~~~

回退代码，准备提交的代码还在本地编辑器里

~~~bash
git reset <commitId>
~~~

暂存

~~~bash
git stash
~~~

切换正确分支上

~~~bash
git checkout <正确分支>
~~~

取出暂存后，便可提交

~~~bash
git stash pop
~~~



## 七、stash 暂存

### 1. 在A分支修改文件后，切换到B分支

- 在A分支暂存：

~~~bash
git stash
~~~

- 切到B分支，操作完成后，切换A取出：

~~~bash
git stash pop
~~~

### 2. 在A分支修复了文件，提交到B分支

前提：例如在master分支中修改了部分文件， 这时候不想提交到该分支， 想要提交到dev分支， 这时：

- 在master分支中：

~~~bash
 git stash
~~~

- 切换到dev分支：git checkout dev

- 将修改同步到dev分支：

~~~bash
git stash apply
~~~

## 八、克隆最新记录

只克隆最新的额记录

~~~bash
git clone --depth=1 
~~~

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/git01.1wo915j3trc0.webp" alt="image-20220829140609150" style="zoom:67%;" />

代码拉下来了。但这个分支不一定是你需要的分支，此时你需要切换到你需要的分支上去。
代码如下：

~~~bash
git remote set-branches origin <远程分支名称>
git fetch --depth 1 origin <远程分支名称>
git checkout <远程分支名称>
~~~

但：分支的log，无论是之前的分支，还是新拉取的分支，都只有一条提交记录。想查看多条要怎么办呢？

~~~bash
git fetch --unshallow
~~~

如果源存储库已完成，请将浅存储库转换为完整存储库，从而消除浅存储库所施加的所有限制。
如果源存储库很浅，则尽可能多地获取，以便当前存储库与源存储库具有相同的历史记录。
然而，这个会去拉取所有的记录。可想而知，如果所有的记录可以同步下来，我们也就不用这样折腾了。所以，此时用这个命令解决不了问题。
当然，如果情况是你先获取了远端库的一部分，想全部同步过来，且整个库体积不大。这个方法可以用来同步所有的记录。

~~~bash
git fetch --depth=100
~~~

### 九、代码提交（不产生多余“Merge branch 'dev' of ...” 记录）

方法一：

~~~
 git pull --rebase
~~~

方法二：

如果不想每次都rebase，可以在git bash里执行如下命令，在每次pull前先进行rebase操作。

~~~bash
git config --global pull.rebase true
~~~

