

## 1.1 git初始化

```javascript
git init
```

## 1.2 git设计签名

项目级别/仓库级别：仅在当前本地库范围有效

```javascript
git config user.name xxx
git config user.email xxx@xx.com
//查看设置保存位置
cat .git/config
```

系统用户级别：登录当前操作系统的用户范围

```javascript
git config --global user.name xxx
git config --global user.email xx@xx.com
//查看设置保存位置
git ~/.gitconfig
```

## 1.3 查看状态

```javascript
git stutas
```

## 1.4 本地文件提交到暂存区

```javascript
//提交单个文件
git add [文件名]

//提交所有文件
git add .
```

##1.5 暂存区提交到本地版本库

```javascript
git commit -m '注释' [文件名]
//或者
git commit -m '注释'
```

## 1.6 查看历史记录

```javascript
1 git log
效果如下：
//commit c5dc521b8699cfbd30a1b88fbb5c42c04280c60a
//Author: lutiantian <cztian1314@163.com>
//Date:   Mon Dec 3 09:57:50 2018 +0800

//    first-commit

2 git log --pretty=oneline
效果如下:
//c5dc521b8699cfbd30a1b88fbb5c42c04280c60a first-commit

3 git reflog
效果如下：
//f97e8c9 (HEAD -> master, origin/master) HEAD@{0}: commit: 测试修改远程库名字
//c5dc521 HEAD@{1}: commit (initial): first-commit

```

## 1.7历史记录的回退前进

方式：

+ 基于索引值操作 [推荐]

  ```javascript
  git reset --hard [索引值]
  ```

+ 使用^符号 （只能后退，不能前进）

  ```javascript
  //一个^表示后退一步，后退几步就写几个^
  git reset --hard HEAD^
  ```

+ 使用~符号（只能后退，不能前进）

  ```javascript
  // n表示后退的步数
  git reset --hard HEAD~n
  ```

## 1.8git的分支

在版本控制过程中，使用多条线同时推进多个任务。

#### 1.8.1创建分支

```javascript
//查看所有分支
git branch -v
//新建分支
git branch [分支名]
```

#### 1.8.2切换分支

```javascript
git checkout [分支名]
```

#### 1.8.3合并分支

```javascript
//第一步，切换到接受修改的分支（被合并，增加新内容）
//例如:把fix_bra 分支合并到 master
git checkout [被合并分支名]
例如： git checkout master

//第二步，执行merge命令
git merge [合并分支名]
例如：git merge fix_bra
```

#### 1.8.4冲突

```javascript
//冲突样式
<<<<<<<<HEAD
XXXXXXXXXX
========
XXXXXXXXXX
>>>>>>>>master

//其中HEAD 和 ===之间的内容是当前分支的内容
// ====和master之间的内容是另一个分支的内容
```

## 1.9git远程库

+ 首页及注册账号页  [https://github.com]()

+ 新建仓库

+ 在本地穿件远程库地址别名

  ```javascript
  //1.查看别名
  git remote -v
  
  //2.设置别名
  git remote add origin https://github.com/littledimple123/note.git
  //origin是设置的别名，https://github.com/littledimple123/note.git 是被设置的地址
  ```


## 2.0  push操作

```javascript
//git push -u [别名][分支名]
git push -u origin master
```

## 2.1 pull操作

```javascript
git pull
```

## 2.2  clone克隆项目

```javascript
git clone [仓库远程地址]
```

### 2.3如何在git push之后删除node_modules和dist

**一定要养成在项目开始就创建.gitignore文件的习惯，否则一旦push，处理起来会非常麻烦。 **

新建`.gitignore`文件

```javascript
touch .gitignore
```

进入 `.gitignore`文件

```javascript
vi .gitignore
```

按`i`进行编写

```javascript
node_modules/
dist/
```

按`Esc`退出编辑 ，按 `:wq`保存退出vim  按`:q`不保存退出

在命令行进入仓库目录，删除github仓库上.gitignore上新加的选项 

```javascript
git rm -r --cached .
```

重新提交

```javascript
git add .
```

接着commit，简要说明一下commit的内容 

```javascript
git commit -m 'remove node_modules and dist'
```

最后在git push 到远程仓库上就可以了 

```javascript
git push
```

**话题外1：**

vim编辑器

```javascript
1. 进入文件  vi 文件
2. :wq 保存退出  :q直接退出
3. 光标的移动除了键盘上的上下左右键.也可以是小写模式下,按键盘上的"h,j,k,l",
	"ctrl+b":屏幕往后移动一页
	"ctrl+f":屏幕往前移动一页
	"ctrl+u":屏幕往后移动半页
	"ctrl+d":屏幕往前移动半页
4.输入任何一个整数,然后输入"shift+G" 就可以到这一页的开头了.
  按键盘上的大写"G"移动到文章的最后.
  按"$"符号将光标移动到行尾
  "^"将光标移到行头
  "w":将光标移到下一行头
  "b":跟"w"相反.移到上一行行头
  "e":将光标移到下一行尾.
5.如果想让文本显示行号.就在命令行模式（假如在编辑区，按esc）下,在文本最后输入":set nu"命令
```

## 创建标签

```
// 创建标签名
git tag <name> 
// 查看所有标签
git tag
// push到远程仓库
git push --tags
// push到远程仓库之后，切换tag查看内容
git checkout <name>
// 注意每写一个tag时要回退到原始版本、
git reset --hard <版本序列号>
```



