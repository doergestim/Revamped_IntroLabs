![image](https://github.com/user-attachments/assets/068fae26-6e8f-402f-ad69-63a4e6a1f59e)
 

# Honey Share 

# Windows VM

In this lab we will be creating and triggering a honey share.  The goal of this lab is to show how to set up a simple Impacket SMB server that can record attempted connections to it. 

This can be used for detecting lateral movement in a Windows environment.  

One of the cool things about this is it will track the compromised user, the system and the password hash of the compromised user account. 

Let's get started. 

First, we will need to open a Linux Terminal: 

- Open **Command Prompt**

<img width="85" height="103" alt="image" src="https://github.com/user-attachments/assets/b2c7dbad-d57b-40d0-9318-ca8d40176c22" />

- Get the IP of the other VM
```bash
tailscale status
```

<img width="740" height="75" alt="image" src="https://github.com/user-attachments/assets/8ec3aa43-15fc-4a2c-a1e4-5e0caa219ef5" />

>[!IMPORTANT]
>We are looking for the **linux** VM, so grab the IP from the **linux** line
>
>For us it is `100.116.161.87`, **YOUR IP MAY BE DIFFERENT, USE YOURS**

- **SSH** into that machine
```bash
ssh ubuntu@100.116.161.87
```

Password is `metarange`

<img width="247" height="25" alt="image" src="https://github.com/user-attachments/assets/69706053-abe6-4de7-aa48-d9fd739ec4a7" />

Next, we will navigate to the **Impacket** directory: 

```bash
cd ~/ADCD/impacket/examples
```

- Make sure you are in the right place

```bash
ls
```

It should look like this:

<img width="1920" height="146" alt="image" src="https://github.com/user-attachments/assets/f28509be-1912-4be7-b79e-29b0932b5714" />

Now, let's start the SMB server: 

```bash
python3 ./smbserver.py -smb2support -comment 'secret' SECRET /secret
```

It should look like this: 

![image](https://github.com/user-attachments/assets/d1268c27-a141-4a95-96ce-a9482d4b3e56)
 
Next, let's open a Windows Command Prompt: 

![image](https://github.com/user-attachments/assets/0ccc949d-32c3-4d7b-bb18-1bb39ee36dfc)
  
Then, attempt to mount the share from your Windows system: 

- Make sure to use the **Linux IP** from **tailscale**

```bash
net use * \\100.116.161.87\secret
```

>[!IMPORTANT]
>
>Your IP address may be different!!! 
  

We did the most basic level of attempted authentication to the share, and it generated an error.  

![image](https://github.com/user-attachments/assets/8d861109-cd62-4231-946a-98f2284466a6)

However, the trap was triggered! 

Go back to your **Linux terminal** and see the log data. 

It should look like this: 

![image](https://github.com/user-attachments/assets/4b3291a4-fbd1-49a6-968f-f72caefc403a)

***                                                                 
<b><i>Continuing the course? </br>[Next Lab](/IntroClassFiles/Tools/IntroClass/ADHD/honeyuser/honeyuser.md)</i></b>

<b><i>Want to go back? </br>[Previous Lab](/IntroClassFiles/Tools/IntroClass/ADHD/HoneyBadger_files/HoneyBadger.md)</i></b>

<b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

***Finished with the Labs?***

Please be sure to destroy the lab environment!

[Click here for instructions on how to destroy the Lab Environment](/IntroClassFiles/Tools/IntroClass/LabDestruction/labdestruction.md)

---


  

 

  

 

 








