---
layout: post
title: Git Configuration to a new computer/user
category: tech
tags: [git,github,ssh]
---

## 1. user/e-mail configuration

```shell
git config --global user.name "Jichao He"
git config --global user.email "jichao.he@student.kit.edu"
```

## 2. make sure if you already have ssh

```shell
ls -al ~/.ssh
```
By defaut, the ssh files are:

- *id_dsa.pub*

- *id_ecdsa.pub*

- *id_ed25519.pub*

- *id_rsa.pub*

## 3. for new computer: generate new SSH keys

```shell
ssh-keygen -t rsa -b 4096 -C "jichao.he@student.kit.edu"
Generating public/private rsa key pair.
Enter a file in which to save the key (/home/you/.ssh/id_rsa): [Press enter]
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```

## 4. Adding your SSH private key to your ssh-agent

```shell
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

## 5. Add the SSH key to your GitHub account.

```shell
sudo apt install xclip
xclip -sel clip < ~/.ssh/id_rsa.pub
```