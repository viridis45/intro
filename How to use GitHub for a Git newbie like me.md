## How do i use github

all done via Git Bash, not cmd

### 1. Commanding repository on your computer

``` bash
git config --global user.name "My UserName"	
```

or travel to desired repository and:

``` bash
git config user.name "My UserName"
```

not sure what happened but can check it via:

``` bash
git config user.name
```

This returned my user name.





### 2. Generating SSH Key

I know for sure that I don't have a existing SSH key so

``` bash
git config --global user.email "my@email.address"
```

and give some memorable words for pass phrase when asked

and pressed enter when asked for a file to save the key for the default file location

then this happened



![](C:\Users\user\Desktop\creatingssn.PNG)







### 3. now add it onto ssh-agent

what does ssh-agent do?



```bash
ssh-add ~/.ssh/id_rsa
```

so the default file that was set via enter

Seems like id-rsa needs to be replaced if different file was created

![](C:\Users\user\AppData\Roaming\Typora\typora-user-images\1562633573476.png)

Not good.





Google says launch the ssh-agent.

```bash
eval $(ssh-agent -s)
```

then this happened:

![tempsnip](C:\Users\user\Desktop\tempsnip.png)

I think it worked.





### 4. Add this to GitHub account

copy the SSH key to clipboard

```bash
clip < ~/.ssh/id_rsa.pub
```

at GitHub website, Setting > SSH and GPG keys > New SSH key 

I think it worked







### 5. Cloning repository

routine goes like this : fork - edit - commit - pull request - merge

Accidentally made a test repository from google colab so there is an empty repository.

took the URL from it and on git bash

```bash
git clone https://ALL THE URL.git
```

says Unpacking objects: 100% , done.

so i think it is done

it created a folder with the repo name on C:/users/user

navigated into the created folder and check if it is correct:

```bash
git remote -v
```



tried $ git push but permission was denied because of the previous user of this desktop. need to change that.

```bash
git remote set-url URLToDesiredRepo
```

it was not successful.



control Panel > User Accounts > Manage your Credentials > Windows Credentials > Generic Credentials

dang this desktop is in korean so it should be

제어판 > 사용자 계정 > 자격 증명 관리자 > 일반 자격 증명

find credential on github to remove.



back to git bash and $ git push

prompted with GitHub login info. yay korean

logged in. Bash says Everything up-to-date

yay



### 6. Committing

```bash
git commit -m 'COMMIT MESSAGE'
```

### 7. Pushing

```bash
git push origin BRANCHNAME
```



### 8. Adding a new repo

at the new directory 

```bash
git init
#add a file
git add TheFile
git commit
```

says 'create mode '

then created a new repo at github page





### * In case stuck at the commit editor

i : inline insert mode

esc : exit insert mode

:x! and enter : save and exit

:q! and enter: exit without save







In case needed

### * Creating a new branch

creating new branch on master

```bash
git checkout -b BRANCHNAME
```

or pick a existing branch

```bash
git checkout -b BRANCHNAME EXISTING_BRANCH
```

good thing i've used sourcetree to kinda know the gist of what they are saying.



### * Staging

moved a test file onto the previously created repo folder.

$ git status tells there is an untracked file.

```bash
git add FILENAME
```

check via

```bash
git status
```



















