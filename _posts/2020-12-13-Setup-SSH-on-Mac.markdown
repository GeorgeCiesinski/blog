---
layout: single
title:  "Setting up SSH on a Mac"
date:   2020-12-13 23:00:00 -0500
categories: Programming Git
comments: true
typora-root-url: ..
---

In this post, I wanted to outline some instructions on setting up an SSH key for github on a mac. I recently exchanged my 2020 Intel MBA (MacBook Air) for a 2020 M1 MBA, which I will discuss in a separate post! This also means that unfortunately, I had to go through the whole setup again. Aside from all of the apps I use daily for programming, I also had to set the mac up to access my repos from GitHub. 

As you may know, you can clone a repository using several different options. The most commonly used method is probably **HTTPS**, which is currently recommended by GitHub for it's simplicity. There is less configuration needed and it is easier to do. The other option is **SSH** which is more difficult to configure, but is more secure, and more convenient in the long run. I won't get into the details as that isn't the purpose of this post, but for anybody who wants to set up SSH on their Mac, please read below!

# Generate an SSH key

FIrst, go to your home directory:

```shell
cd ~/
```

Once there, you can use the below line to generate the key:

```shell
ssh-keygen -t rsa
```

This will prompt you to enter a filename, and by default it will suggest the default name `/Users/username/.ssh/id_rsa`. You can simply press enter to save it. You will also be prompted for a passphrase, which is basically a password to verify you are the owner of the key whenever you use it. You can make a passphrase and press enter, or you can just press enter for no passphrase. This will result in the generation of a private and public key, the latter ending in `.pub`:

```shell
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

# Save to Github

You will need to copy the public key to save it to github. The easiest way is to run the below line:

```shell
pbcopy < ~/.ssh/id_rsa.pub
```

Next, go to github and follow the below steps:

1. Click the dropdown beside your thumbnail on the top right, and click Settings.
2. Click SSH and GPG keys in the settings menu.
3. Under "SSH Keys" click the **New SSH key** button.
4. Under title, you can name your key. I simply name it after the machine so I can identify it easily.  Under Key, simply paste the value you copied earlier with the `pbcopy` command.

Once all of this is done, you are set! Your private key and public key will ensure a high level of security when interacting with the repo.

Thanks for reading my post! I hope that this was helpful, and that you are looking forward to future posts about **git** in the future.

