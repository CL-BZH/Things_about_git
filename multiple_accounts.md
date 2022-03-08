# Working with 2 GitHub accounts on the same computer

The aim of this tutorial is to show how to set up 2 different GitHub accounts and working with them on the **same computer** in order to be able to simulate some collaboration work (pull request, etc) and excercise some advance git commands.  

## Step 1:
Create 2 GitHub accounts.  
E.g. `<fullname>` and `<dummy-fullname>` accounts.  
You will need 2 different email address but you can use an alias to the same email address (see for example alias in gmail or hotmail).

In each account add a repository. E.g. `Temp`.

## Step 2:
Create 2 [ssh keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

Use 2 different names for saving the keys in `~/.ssh` directory.  
For example, you can save the key for GitHub accound `<fullname>` using `~/.ssh/id_rsa` and the key for GitHub accound `<dummy-fullname>`  using `~/.ssh/id_rsa_dummy`.

## Step 3:
Add your SSH private keys to the ssh-agent.  
Following the previous example, enter the commands below in your terminal: 
```console
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_dummy
```

## Step 4:
Add the public key for user 1 to the GitHub account for machine/user 1.  
Add the public key for user 2 to the GitHub account for machine/user 2.   
Following previous example:  
- Add the public key `~/.ssh/id_rsa.pub` to GitHub account `<fullname>` 
- Add the public key `~/.ssh/id_rsa_dummy.pub` to GitHub account `<dummy-fullname>`


## Step 4:
Create/Update the ssh config file.  
Open `~/.ssh/config` and add identity files for each GitHub accounts.  
With the previous example, this could be the content of the config file:  
```
Host github.com
  HostName github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
Host github-dummy.com
  HostName github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa_dummy 
```
> Note how one host is `github.com` and the other is `github-dummy.com` such that we can attach 2 different identity files.

## Step 5:
You are ready ! 

Let’s say your 2 GitHub accounts are `<fullname>` and `<dummy-fullname>` and in each you created a repository named `Temp`.  
Then in each terminal do the following:

### Terminal 1:
```console
> mkdir Temp_1
> cd Temp_1
> git init
> touch temp.txt
> git add temp.txt
> git commit -m "first commit"
> git branch -M main
> git remote add origin git@github.com:<fullname>/Temp.git
> git push -u origin main
```

### Terminal 2:
```console
> mkdir Temp_2
> cd Temp_2
> git init
> touch temp.txt
> git add temp.txt
> git commit -m "first commit"
> git branch -M main
> git remote add origin git@github-dummy.com:<dummy-fullname>/Temp.git
> git push -u origin main
```

> Note that in Terminal 1 we are using `github.com` and in Terminal 2 we are using `github-dummy.com`.