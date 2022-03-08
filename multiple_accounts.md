# Working with 2 GitHub accounts on the same computer

The aim of this tutorial is to show how to set up 2 different GitHub accounts and working with them on the **same computer** in order to be able to simulate some collaboration work (pull request, etc) and excercise some advance git commands.  

## Step 1:
Create 2 GitHub accounts.  
E.g. `<fullname>` and `<dummy-fullname>` accounts.  
You will need 2 different email addresses but you can use an alias to the same email address (see for example alias in gmail or hotmail).

In each account add a repository. E.g. `Temp`.

## Step 2:
Create 2 [ssh keys](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

Use 2 different names for saving the keys in `~/.ssh` directory.  
For example, you can save the key for GitHub account `<fullname>` using `~/.ssh/id_rsa` and the key for GitHub account `<dummy-fullname>`  using `~/.ssh/id_rsa_dummy`.

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
With the previous example, the content of the config file would be:  
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
Then, you could, for example, open 2 terminals and in each of them do the following:

### Terminal 1:
```console
> mkdir Temp_1
> cd Temp_1
> git init
> git config user.name "<user_1>" 
> git config user.email "<user_1@example.com>"
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
> git config user.name "<user_2>" 
> git config user.email "<user_2@example.com>"
> touch temp.txt
> git add temp.txt
> git commit -m "first commit"
> git branch -M main
> git remote add origin git@github-dummy.com:<dummy-fullname>/Temp.git
> git push -u origin main
```

> Note that in Terminal 1 we are using `github.com` and in Terminal 2 we are using `github-dummy.com`.

> In each repository we set the `user.name` and the `user.email` to prevent that the default user name and email address (set in `~/.gitconfig`) is used.  
If we don't do that we would have the same user id for the commits pushed in the different GitHub accounts which would be quite confusing.

## Important
If you want to simulate collaboration of `<user_2>` to the repository `<repo>` of `<user_1>` you have to follow the steps below:
- In the GitHub remote repository `<repo>` of `<user_1>`, add `<user_2>` as collaborator.
- Open a terminal for `<user_2>` and do:
    - Remove all identities: `ssh-add -D`
    - Add the ssh key for user_2: `ssh-add ~/.ssh/id_rsa_dummy` 
    - Clone `<repo>` from `<user_1>` using `git@github-dummy.com` (not `git@github.com`).  
    So, let's say that the `<user_1>` account is named `<fullname>` and the repository is `<repo>`, then the command is:  
    `git clone git@github-dummy.com:<fullname>/<repo>.git`
    - Now `<user_2>` can push modifications in `<user_1>` repository `<repo>`.