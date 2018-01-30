---
title: Git 学习笔记
date: 2017-08-08 21:22:06
tags:
  - Git
categories:
  - 工具
number: 2
---
## 写在前面 ##

 - 本文档是学习 [廖雪峰Git教程][1] 后所做的笔记，教程帮忙很大，非常感谢！
 - 本笔记主要记录了教程中所用的命令，概念内容不包括在内。
 - 建议看完教程后再看Git官网的 [Pro Git][2] 一书。

## 创建版本库 ##

 - 创建仓库并提交

    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git init` | 初始化一个Git仓库 |
    | `git add <file>` | 添加文件到暂存区（-f 可强制添加） |
    | `git commit -m "提交说明"` | 从暂存区提交到版本库|
    | `cat <file>` | 查看文件内容 |

## 时光机穿梭 ##

 - 查看仓库状态、差异
    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git status` | 查看仓库当前状态 |
    | `git diff <file>` |  比较工作区和暂存区的差异 |
    | `git diff --cached <file>` |  比较暂存区和版本库的差异 |
    | `git diff HEAD -- <file>` |  比较工作区和版本库的差异 |

 - 版本回退

    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git log` | 查看提交历史 |
    | `git log -1` | 查看最后一次提交历史 |
    | `git log -num` | 查看最后num次提交历史 |
    | `git log --pretty=oneline` | 单行格式显示提交历史 |
    | `git reflog` | 查看所有操作记录，包括删除的commit记录 |
    | `git reset --hard commit_id` | 切换到指定版本 |
    | `git reset --hard HEAD^` | 回退上一个版本 |

    > HEAD 表当前版本， HEAD^ 表上一版本，HEAD^^ 表上两版本，HEAD~99 表上99版本。

 - 撤销修改
    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git checkout -- file` | 撤销工作区的修改 |
    | `git reset HEAD file` | 撤销暂存区的修改 |

    > 已经提交了不合适的修改到版本库时，想要撤销本次提交，可以版本回退，不过前提是没有推送到远程库。

 - 删除文件
    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git rm <file>` | 删除一个文件 |

    > 如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

## 推送到远程库 ##

 - 关联远程库

    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git remote add origin git@github.com:Jason-lhh/learngit.git`| 关联远程库 |
    | `git remote -v` | 查看远程库信息 |
    | `git push origin master` | 推送并关联指定分支到远程库 |

    > 关联后，使用命令`git push -u origin master`**第一次**推送master分支的所有内容；

 - 从远程库克隆

    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    |   `git clone git@github.com:Jason-lhh/gitskills.git` | 将远程仓库克隆到当前目录 |
    | `git pull` | 拉取远程仓库内容 |

    > 先有远程库，后有本地库。要克隆一个仓库，首先必须知道仓库的地址，然后使用 `git clone git@github.com:账户名/仓库名字.git` 命令。

## 分支管理 ##

 - 创建与合并分支

    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git branch <brach>` | 创建分支 |
    | `git branch` | 查看分支 |
    | `git checkout <brach>` | 切换分支 |
    | `git checkout -b <brach>` | 创建+切换分支 |
    | `git merge <brach>` | 合并某分支到当前分支（fast forward模式） |
    | `git merge --no-ff <branch>` | 禁用快速合并 |
    | `git merge --no-ff -m "提交说明" <branch>` | 普通方式合并，并附提交说明 |
    | `git stash` | 保存当前工作环境（包括工作区和暂存区） |
    | `git stash list` | 查看保存的工作列表 |
    | `git branch -d <name>` | 删除分支 |
    | `git branch -D <name>` | 强制删除分支 |
    | `git log --graph --pretty=oneline --abbrev-commit` | 查看分支合并图 |

    > 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

 - 分支管理策略图

![此处输入图片的描述][3]

 - Bug分支

```flow
st=>start: 修复bug
th=>operation: git stash 保存当前工作环境
op=>operation: 切换master分支创建
切换bug分支修复
op1=>operation: 合并bug分支
删除bug分支
en=>operation: git stash pop 回到工作现场
e=>end: 完成

st->th->op->op1->en->e

```

>   `git stash list`   查看当前工作现场
`git status`  可以用查看工作区是干净的


 - 多人协作

多人协作的工作模式通常是这样：

 1. 首先，可以试图用`git push origin branch-name`推送自己的修改；
 2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
 3. 如果合并有冲突，则解决冲突，并在本地提交；
 4. 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！

如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

## 标签管理 ##

 - 标签

    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git tag` | 查看所有标签 |
    | `git tag <tag_name>` | 用于当前分支新建一个标签 |
    | `git tag <tag_name> <commit_id>` | 给指定commit打标签 |
    | `git tag -a <tag_name> -m "标签说明" <commit_id>` | 给指定commit打标签，并附说明 |
    | `git tag -s <tag_name> -m "标签说明" <commit_id>` | 用gpg私钥签名 |
    | `git tag -d <tag_name>` | 删除标签 |
    | `git show <tag_name>` | 显示标签信息 |
    | `git push origin <tag_name>` | 推送标签到远程库 |
    | `git push origin --tags` | 推送所有未推送的标签到远程库 |
    | `git push origin :refs/tags/<tag_name>` | 删除远程标签（先删除本地，再使用该命令删除） |




## 自定义Git ##

 - 修改git样式

    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git config --global user.name "you_name"` | 设置全局用户名 |
    | `git config --global user.email "email@example.com"` | 设置全局邮箱 |
    | `git config --global color.ui true` | 设置全局颜色显示 |
    | `git config --global alias.<alias_name> <'command_name'>` | 设置别名 |

 - 忽略特殊文件

    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git check-ignore -v <file>` | 查看忽略该文件的规则 |

 - 配置别名列表

    | 命令代码 | 代码含义 |
    | :--------:   | :------:  |
    | `git config --global alias.st status` | `git st` |
    | `git config --global alias.co checkout` | `git checkout <branch_name>` |
    | `git config --global alias.ci commit` | `git ci -m "提交说明"` |
    | `git config --global alias.br branch` | `git br` |
    | `git confg alias.unstage 'reset HEAD'` | `git unstage <file>` |
    | `git confg alias.last 'log -1'` | `git last` 最近一次提交 |


>  `git confg alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`
    `git lg` 查看提交历史


 - [搭建Git服务器][4]

  [1]: https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000
  [2]: https://git-scm.com/book/zh/v2
  [3]: http://otp5qkej4.bkt.clouddn.com/17-8-8/89451458.jpg
  [4]: https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000