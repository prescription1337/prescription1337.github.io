---
title: "Cross-Platform Github Repository Management"
date: "2025-01-22 00:00:00 +0900"
categories: [Technology, github]
tags: [GitHub Pages, Github]
---

# SSHを使ったLinuxからgithub pageの管理
1. SSHキーを作成
- `ssh-keygen -t ed25519 -C "prescription1337@gmail.com"`
  - ![alt text](../assets/images/2025-01-22_14-16.png)
- 確認: `cat ~/.ssh/id_ed22519.pub`
  - ![alt text](../assets/images/2025-01-22_14-17.png)
- 
2. Githubにsshキーを登録
3. GithubリポジトリのリモートURLを変更
- `https://`ではなく`git@github.com:`形式へ
- 現在のリモートURLを確認: `git remote -v`
- 