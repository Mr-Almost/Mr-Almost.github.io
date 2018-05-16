---
title: 配置github
date: 2018-03-23 9:11:05
tags:
配置 GitHub
进入 https://github.com/settings/keys
如果页面里已经有一些 key，就点「delete」按钮把这些 key 全删掉。如果没有，就往下看
点击 New SSH key，你需要输入 Title 和 Key，但是你现在没有 key，往下看
打开 Git Bash
复制并运行 rm -rf ~/.ssh/* 把现有的 ssh key 都删掉，这句命令行如果你多打一个空格，可能就要重装系统了，建议复制运行。
运行 ssh-keygen -t rsa -b 4096 -C "你的邮箱"，注意填写你的邮箱！
按回车三次
运行 cat ~/.ssh/id_rsa.pub，得到一串东西，完整的复制这串东西
回到上面第 3 步的页面，在 Title 输入「我的第一个 key」
在 Key 里粘贴刚刚你你复制的那串东西
点击 Add SSH key
回到 Git Bash
运行 ssh -T git@github.com
输入 yes 回车

然后如果你看到 Permission denied (publickey). 就说明你失败了，请回到第 1 步重来，是的，回到第 1 步重来；如果你看到 Hi FrankFang! You've successfully authenticated, but GitHub does not provide shell access. 就说明你成功了！

一台电脑只需要一个 SSH key
一个 SSH key 可以访问你的所有仓库，即使你有 1000000 个仓库，都没问题
如果你新买了电脑，就在新电脑上重新生成一个 SSH key，把这个 key 也上传到 GitHub，它可以和之前的 key 共存在 GitHub 上
如果你把 key 从电脑上删除了，重新生成一个 key 即可，替换之前的 key
配置 git
git config --global user.name 你的英文名
git config --global user.email 你的邮箱
git config --global push.default matching
git config --global core.quotepath false
git config --global core.editor "vim"
五句话，依次运行。不执行的话，电脑可能会爆炸。