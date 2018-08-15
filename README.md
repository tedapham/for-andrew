
# Automatically Login to the Data Science Server SSH

While it's possible to login to the Data Science server by entering specific details, e.g.:


	$ ssh "annalectww\USERNAME"@10.5.227.152


... it's by no means an exciting and rewarding use of ones' time.

To avoid doing that, we can first add an alias to our `~/.zshrc` ( or `~/.bash_profile`) to move away from typing the full command.

Remember to change out first.last to your own first and last name.

	$ echo "alias dss='ssh \"annalectww\first.last\"@10.5.227.152'" >> ~/.bash_profile
You may want to verify that this was done properly via either a tail or by exploring in vim:

	$ vi ~/.zshrc

Now, we can start the login by opening a **new** Terminal window and typing ```dss``` (or your preferred alias). However, that still forces us to enter a password. 

To 'fix' the problem of needing to enter a password, we can use an Authorized Keys file. 

On your **local** machine, let's start by generating a keypass.

	$ ssh-keygen

You can simply **accept all the defaults** (default name, no password). It might look like this:

```
$ ssh-keygen
Generating public/private rsa key pair.
$ Enter file in which to save the key (/Users/first.last/.ssh/id_rsa): 
Created directory '/Users/first.last/.ssh'.
$ Enter passphrase (empty for no passphrase): 
$ Enter same passphrase again: 
Your identification has been saved in /Users/first.last/.ssh/id_rsa.
Your public key has been saved in /Users/michael.griffiths/.ssh/id_rsa.pub.
```

We're going to assume that you accepted the defaults and have your public key saved in `~/.ssh/id_rsa.pub`

First let's copy the contents of that file, and then we'll push into the server.

	$  cat ~/.ssh/id_rsa.pub
	
Copy the output of the cat command.

Login to the server.

	$ dss
		
Enter your password - last time!

Once you're logged in, modify or create the authorized_keys file.
Now, the .ssh directory may not exist -- so try to create it:

	$ mkdir ~/.ssh
	
And then edit the file.

	$ vi ~/.ssh/authorized_keys
	
Paste your SSH key in. Save.

Now you need to modify the permissions -- otherwise it won't work.

	$ chmod 0600 ~/.ssh/authorized_keys


Now you should be able to login to the server simply by typing `dss` into the command line!

	$ dss
	Ubuntu 12.04.5 LTS
	ANNALECTWW\first.last@datascience-we-z1b-de:~$ 



### Connect to DSDK from the Data Science Server

If you're working remotely, then you may discover that you can't connect to DSDK directly. That's because firewall rules exclude anything outside of the subnet that our office is in. 

A recommended way of dealing with this problem is to connect to the data science server via SSH, and then to connect to DSDK from the data science server.

However, it seems silly to type `dss` to log into the data science server (from above) then type `dsdk` to log into the DMP. Why must there be two steps?

Well, there doesn't need to be.


	$  vi ~/.zshrc
	alias dmp='ssh "annalectww\USERNAME"@10.5.227.152 -t "psql -h dsdk-v0p1-annalect.csktdopmxkxl.us-east-1.redshift.amazonaws.com -U USERNAME -d dsdk -p 5439"'

`:wq`


Please ensure that you have `.pgpass` setup in your home directory on the data science server (Repeat pgpass process above, but within the remote server).

### Final Note: Screen

Using `screen` is recommended to manage multiple sessions, including where you might lose connection. 
