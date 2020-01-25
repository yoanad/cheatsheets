# Multiple SSH keys on Mac

## Generate ssh keys
`ssh-keygen -t rsa -C "your_email@youremail.com"`

## Save keys in
``~/.ssh/id_rsa
~/.ssh/id_rsa_exp``

## Add these two keys as following

``ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_exp``

## Delete all cached keys before
``$ ssh-add -D``


## Check your saved keys
``ssh-add -l``

## Modify the ssh config
``cd ~/.ssh/
nano config``

## Then add

```#private account
Host github.com
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa

#company account
Host exp
	HostName exp.gitlab.com
	User git
	IdentityFile ~/.ssh/id_rsa_exp
```

## Clone you repo and modify your Git config clone your repo
``git clone git@exp.gitlab.com:exp/folder.git``

## cd folder and modify git config

``git config user.name "yoanad"
git config user.email "your_email1@youremail.com"``

``git config user.name "yoanad2"
git config user.email "your_email2@youremail.com"``

or you can have global git config
``git config --global user.name "jexchan"
git config --global user.email "jexchan@gmail.com"``

## use normal flow to push your code
``git add .
git commit -m "your comments"
git push``
