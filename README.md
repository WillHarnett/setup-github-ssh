# setup-github-ssh
This is always a pain, so this is is going to show you how to setup github ssh.
I need to cleanup the markdown, but for now all the information you need should be here.

First let's generate an ssh key:
1. Open terminal
2. Run the following command (Replace example@example.com with your github email)
ssh-keygen -t rsa -b 4096 -C "example@example.com"
3. You should be prompted with where to put the private and public keys 
"Enter file in which to save the key (/Users/{{your username}}/.ssh/id_rsa):"
4. If it's the default folder path is okay, just click enter (it usually is)
5. To open the .ssh folder, go to Go and then open Home
6. On your keyboard do cmd + shift + . 
7. You should now see a .ssh folder
8. Open the .ssh folder and you should see two files id_rsa and id_rsa.pub <<< The id_rsa.pub is what you'll have to copy over to github
10. I like to rename my keys so I know what they are used for. I usually rename them to something like github_username and github_username.pub
11. Open you .pub file in text edit and copy all of the contents
12. Open a browser and login to your github
13. Click on your icon in the top right hand corner and go to settings
14. Go to SSH and GPG keys (https://github.com/settings/keys)
15. Select create a new key
16. I usually add a name that tells me where the key is generated like: "personal mac ssh key"
17. Paste in the public key, save, and follow the steps below

Follow this for checking your ssh connection
https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection

When it asks you something like "Are you sure you want to continue connecting (yes/no)?",
you can check the key fingerprints against this list:
https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints

You can use the following command to test your connection and get a little more information back about why it's failing:
ssh -vT git@github.com

Here's how an example config file you can put in your .ssh folder to setup multiple accounts on your mac:
# Default GitHub
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa

# Work GitHub
Host work.github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_work

Then check to see if both of the above fingerprints are there with the following command:
$ ssh-add -l

If you don't see both, then you'll have to add them. You can add a fingerprint with the following command:
ssh-add ~/.ssh/id_rsa_work

Then to test both you'd run the following commands:
ssh -T git@github.com
ssh -T git@work.github.com

A common cause is cloning using the default (HTTPS) instead of SSH. You can correct this by going to your repository, clicking "Clone or download", then clicking the "Use SSH" button above the URL field and updating the URL of your origin remote like this:

git remote set-url origin git@github.com:username/repo.git

You can check if you have added the remote as HTTPS or SSH using:

git remote -v
