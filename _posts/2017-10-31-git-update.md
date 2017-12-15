---
layout: post
title: 派生(fork)仓库与上游保持更新
description: "git"
modified: 2017-10-31
tags: [sample post]
image:
  feature: abstract-2.jpg
---

### 派生(fork)仓库与上游保持更新

1. 在组织仓库点击fork后，将仓库组织到自己的账号下，然后在自己的仓库下clone代码
2. 使用`git remote -v`查看当前远程仓库地址，输出如下:

  ```markdown
  origin  http://codeio.dftoutiao.com/yannan/cms.git (fetch)
  origin  http://codeio.dftoutiao.com/yannan/cms.git (push)
  ```

  发现仓库来源就是自己的仓库地址

3. 添加组织仓库来源命令:

  ```markdown
  git remote add upstream http://codeio.dftoutiao.com/SHFE/cms.git
  ```

  这条命令相当于添加了一个别名为upstram上游地址，指向之前fork的原仓库地址。 再次运行`git remote -v`命令发现输出如下:

  ```markdown
  origin  http://codeio.dftoutiao.com/yannan/cms.git (fetch)
  origin  http://codeio.dftoutiao.com/yannan/cms.git (push)
  upstream        http://codeio.dftoutiao.com/SHFE/cms.git (fetch)
  upstream        http://codeio.dftoutiao.com/SHFE/cms.git (push)
  ```

4. 运行更新合并命令:

  ```markdown
  git fetch upstream
  git checkout master
  git merge upstream/master
  ```

5. 推送本地仓库到自己账号的远程仓库

  ```markdown
  git push origin master
  ```

通过以上操作成功将fork仓库与上游仓库保持了更新。
