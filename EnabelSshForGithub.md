# Enabling you to be able to clone projects into your computer from your account on github using ssh (windows only)

1. Create key pair from your computer
``` bash
ssh-keygen -t rsa -b 4096 -C "tr.sar77an78@gmail.com" 
```

2. Then add that the private key to your `ssh-agent`
```bash
Get-Service ssh-agent # First check whether the agent is running or not
# if it is not running ! start it 
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent
ssh-add C:\Users\<yourusername>\.ssh\id_rsa
```  

3.  Finally copy the public key to your github account
```bash
Get-Content ~/.ssh/id_rsa.pub | Clip
```
Then go to github > settings > ssh&gpg > then add the key


3