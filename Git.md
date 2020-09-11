### Git

Git和SVN区别

- Git是分布式的版本控制系统，没有一个中央服务器，一台电脑就是一个完整版本库，工作的时候不需要联网。协同的方法：A在自己电脑改了代码，B也在自己电脑改了代码，这时，只需要将各自修改的代码推送给对方，就可以看到对方的修改。git可以直接看到更新了哪些代码
- SVN是集中式的版本控制系统，有中央服务器，工作的时候用的自己电脑，需要从中央服务器进行代码拷贝，然后进行工作，工作完成后，将自己修改的代码推到中央服务器，需要联网

Git Bash：Unix和Linux风格的代码行，使用最多

Git CMD:Windows风格

Git GUI：图形界面的Git

![1595474356132](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1595474356132.png)

git config --system -list 系统配置文件  对应文件  C:\Program Files\Git\mingw64\etc\gitconfig

git config --global -list 用户配置文件    C:\Users\DELL\gitconfig

配置用户：git config --global user.name "用户名"

​					git config --global user.email "邮箱"

#### 基本理论

Git本地有三个工作区域：工作目录、暂存区、资源库、加上远程的git仓库就有4个工作区域

![1595476064333](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1595476064333.png)

每个项目都有一个Git目录，是用来保存元数据和对象数据库的地方，每次克隆镜像的时候，其实就是拷贝其中的数据。

基本的 Git 工作流程如下：

1. 在工作目录中修改某些文件。
2. 对修改后的文件进行快照，然后保存到暂存区域。
3. 提交更新，将保存在暂存区域的文件快照永久转储到 Git 目录中。

提交时记录的是放在暂存区域的快照，任何还未暂存的仍然保持已修改状态，可以在下次提交时纳入版本管理。每一次运行提交操作，都是对你项目作一次快照，以后可以回到这个状态，或者进行比较

#### git项目搭建

![1595477270461](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1595477270461.png)

- 1、本地仓库搭建

  ​	git init :初始化文件（生成隐藏的git文件）

- 2、克隆远程仓库

  ​    git clone 远程目录

  

  

#### git文件操作

**文件四种状态**

Untracked:未跟踪，此文件在文件中，没有加入git库，通过`git add` 状态变为`Staged`

Unmodify:文件入库

Modified:

Staged:暂存状态

```bash
#查看指定文件的状态
git status 文件名
#查看所有文件状态
git status
#git add.        添加所有文件到缓存区
#git commit -m “注释内容”  提交缓存区的内容到本地仓库 -m 提交信息

```

- git commit :Git仓库中的提交记录保存的是目录下的所有文件的快照

忽略文件**

在目录下建立`“.gitignore”`文件，规则：

1、忽略文件中的空行或以“#”开始的行会被忽略

2、使用Linux通配符

![1595487424755](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1595487424755.png)

   

**idea集成git**

**git非分支**

![1595489648935](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\1595489648935.png)

如果同一个文件在合并分支的时候都被修改了则会引起冲突：解决办法是可以修改冲突文件后重新提交，选择保留谁的代码

多个分支并行执行，代码不会产生冲突，会存在多个版本

