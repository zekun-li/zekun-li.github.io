---
title: "Two Github Accounts with Two SSH Keys"
date: 2023-03-27
description: Two Github Accounts with Two SSH Keys
menu:
  sidebar:
    name: Two Github Accounts with Two SSH Keys
    identifier: github
    weight: 13
tags: ["linux", "command"]
categories: ["linux"]
---

To use two different GitHub accounts, one for work and one for personal use, you will need to create two separate RSA keys.

### Create Two RSA keys

Here are the steps to create two RSA keys:

1. Open your terminal.
2. Input the following command, replacing "[email@example.com](mailto:email@example.com)" with your email address:
    
    `ssh-keygen -t rsa -C "[email@example.com](mailto:email@example.com)"`
    
3. When asked to enter a file in which to save the key, type in a unique name for each key. For example, `id_rsa_work` and `id_rsa_personal`.
4. Enter a passphrase for each key when prompted.
5. Delete previously cached keys
    
    `ssh-add -D`
    
6. Add your public keys to your corresponding GitHub accounts.

```
ssh-add ~/.ssh/id_rsa_work
ssh-add ~/.ssh/id_rsa_personal
```

### Modify ssh config file

To modify the ssh config file, use the following command:

`vim ~/.ssh/config`

If the config file does not exist, running this command will create one for you.

After opening the config file, add the necessary configuration settings.

```
# work account
Host github-work
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_work

# personal account
Host github-personal
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_personal
```

### Set Remote URL

If you have not added a remote repository to your local repository yet, you should add the origin by typing:

```
git remote add origin git@github-personal:username/example.git
```

Note that previously you would type `git@github.com`, but now it should be replaced with `git@github-personal`.

If you have already added a remote repository, check the remote URL by typing:

`git remote -v`

If it shows `git@github.com` instead of `git@github-personal`, delete the remote repository by typing `git remote rm origin` and add it again.

### Final

Now, when you want to push to a repository, SSH will select the correct key to use for the appropriate GitHub account. You can now commit, pull, and push as usual.

Related doc: 

[https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories)