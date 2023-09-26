# SSH Key Pair

## Creating the Public / Private Key

Lets do a step-by-step guide on getting SSH Keys and how we can use them to push our files on to GitHub!

1. Open Git Bash, and check you are in your User folder if not use `cd` to get back there<br>

![Screenshot 2023-09-25 160356.png](Screenshot%202023-09-25%20160356.png)

2. Type in `ls` to see a list of directory's in your folder, you should see something like this: <br>

![Screenshot 2023-09-25 160729.png](Screenshot%202023-09-25%20160729.png)

3. We can't see hidden folders and the .ssh folder we need is hidden since it has a . (dot) in front it. To see hidden folders aswell as the usual folders we're going to use `ls -a` and then locate the .ssh folder <br>

![Screenshot 2023-09-25 161146.png](Screenshot%202023-09-25%20161146.png)

4. To go into the .ssh folder we use `cd` followed by the folder name so `cd .ssh` <br>
<br>
5. Type the following command:
```python
$ ssh-keygen -t rsa -b 4096 -C "(USUALLY YOUR EMAIL)"
```
- `-t rsa`: Specifies the type of key to create in this case RSA
- `-b 4096`: Specifies the key length in bits
- `-C`: is a comment people usually input their e-mails in here

6. You will be prompted to enter a file in which to save the key. You can press Enter to accept the default location <br> 
Then you'll be prompted to enter a passphrase. (Press enter if you don't want to add a passphrase) <br> This adds an extra layer of security. Even if someone gains access to your private key, they would still need the passphrase to use it. <br>

Your Git Bash screen should look like this, I've called my file github_test (Note:I did note use a passphrase)

![Screenshot 2023-09-25 162020.png](Screenshot%202023-09-25%20162020.png)

7. You should get given a key fingerprint and a randomart image like this: <br>

![Screenshot 2023-09-25 162256.png](Screenshot%202023-09-25%20162256.png)

8. The two keys (Public & Private) should now be in your .ssh folder the one that is public will end in .pub and the other is your private **(DO NOT SHARE YOUR PRIVATE)** <br>
We can use `ls` to again check the files in our current directory and we see the two keys are in there

![Screenshot 2023-09-25 162448.png](Screenshot%202023-09-25%20162448.png)

9. We need to see the contents of our public key so we can assign it to places to do this we use:
```bash
$ cat github_test.pub
```

and this will print out your public key so should look something like this: 

![Screenshot 2023-09-25 163120.png](Screenshot%202023-09-25%20163120.png)

## Now lets link our public key to GitHub:

10. Add Public Key to GitHub:

- Log in to your GitHub account.
- Go to Settings > SSH and GPG keys.
- Click on "New SSH Key" and paste the contents of your public key 

![Screenshot 2023-09-25 163710.png](Screenshot%202023-09-25%20163710.png)

- Click "Add SSH Key"

11. Next type in the command:
```bash
$ eval `ssh-agent`
```
#### Now what does this do? <br>
The command `eval $(ssh-agent)` is used to start the SSH agent in your current shell session
<br>
12. After running this command, you can add your SSH private key to the agent using ssh-add, and it will handle authentication for you when you connect to remote servers over SSH
```bash
$ ssh-add ~/.ssh/github_test
```
13. To test if the connection is successful we can use: 
```bash
$ ssh -T git@github.com
```
This will give the output:
```bash
Hi shaluomehra! You've successfully authenticated, but GitHub does not provide shell access.
```
<br>

14. We need to change the repository URL to use SSH, you can do this with the following command, replacing username with your GitHub username and repository with the repository name:
```bash
git remote set-url origin git@github.com:username/repository.git
```
Once you've done this you can follow the usual steps to add and commit the file to GitHub and once you `push` the updated files you should get a different message to the one we've been getting previously:

![Screenshot 2023-09-25 164901.png](Screenshot%202023-09-25%20164901.png)

15. This should now be done, we can use `git remote -v` just to confirm we have used SSH to push the file:
<br>
Output: 

`origin  git@github.com:shaluomehra/tech_254_python.git (fetch)`
`origin  git@github.com:shaluomehra/tech_254_python.git (push)`
