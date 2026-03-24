![image](https://github.com/user-attachments/assets/068fae26-6e8f-402f-ad69-63a4e6a1f59e)
 

# Web Honeypot 

- In this lab we will be running a very simple web honeypot.  Basically, it runs a fake Outlook Web Access page and logs the attacks.  

- This is a good approach as attackers constantly go after anything that looks like an authentication portal. 

- Let's get started. 

- First we will need to open a Linux Terminal:




- Open **Ubuntu Shell**

<img width="90" height="104" alt="Screenshot From 2026-02-23 10-28-37" src="https://github.com/user-attachments/assets/ae6d408b-7622-4545-b849-aef3d8fa0cb4" />


- Now, let's start the honeypot: 

```bash
sudo docker run --rm -it -p 80:80 owa-honeypot
```

- It should look like this: 

<img width="1047" height="273" alt="docker_run" src="https://github.com/user-attachments/assets/bed618fa-177a-4f02-b3b0-0613f9df2a03" />

- Now, let's start another Linux Terminal. 




- Open **Ubuntu Shell**

<img width="90" height="104" alt="Screenshot From 2026-02-23 10-28-37" src="https://github.com/user-attachments/assets/ae6d408b-7622-4545-b849-aef3d8fa0cb4" />



- Let's get your Linux IP address. 

```bash
ifconfig
```

- Then, gain a shell to the **owa-container** container. Take its CONTAINER ID with the following command.

```bash
sudo docker ps
```

<img width="1860" height="138" alt="container_id" src="https://github.com/user-attachments/assets/6a804ae5-6a6d-4548-b852-5da180485d95" />

- Take shell at the container.

```bash
sudo docker exec -it <CONTAINER-ID> bash
```

<img width="1862" height="165" alt="docker_shell" src="https://github.com/user-attachments/assets/ac1bd2e7-5ad7-4692-8624-cd2cfaeec576" />


Now, lets tail the **dumppass log**. 

```bash
tail -f dumpass.log
``` 

![image](https://github.com/user-attachments/assets/1877a55c-9717-4428-a08b-38c6ea40af2f)

- Now, let's open a browser window and surf to the **honeypot**: 

```bash
http://YOURLINUXIP
```

<img width="1280" height="826" alt="web_portal" src="https://github.com/user-attachments/assets/c1073b63-d0f9-49c9-bbed-adaea177e39d" />


- Now, try a bunch of **User IDs** and **passwords**. 

- Now, go back to the Ubuntu **Terminal** with the log and you should see the **IP address** and **UserID/Password** of the attempts. 

<img width="1371" height="283" alt="web_logs" src="https://github.com/user-attachments/assets/2fe787e3-3f2d-4e35-b17f-18f1b7b7a6de" />


- Now, let's attack it. 

- Select **OWASP ZAP** on your desktop. 

![image](https://github.com/user-attachments/assets/6493b57a-bb9d-4886-8e15-735ce63a93c7)

- Once **ZAP!** opens, select **Automated Scan**: 

![](attachment/Clipboard_2021-03-12-11-48-15.png) 

- When Automated Scan opens, please put you Kali Linux **IP** in the URL to attack box and select **Attack**. 

- It should look like this: 

![image](https://github.com/user-attachments/assets/c291f9f9-3730-49a6-a874-7d7df5dc5d8e)

- After a while, you should see some attack strings in your Logs.

<img width="1372" height="768" alt="requests_logs" src="https://github.com/user-attachments/assets/5f9c35ed-921b-42c8-b0e2-02c779cabca3" />


Yes...  Some attack tools are as obvious as **ZAP:ZAP**. 

***                                                                 
<b><i>Continuing the course? </br>[Next Lab](/IntroClassFiles/Tools/IntroClass/ADHD/Glastopf.md)</i></b>

<b><i>Want to go back? </br>[Previous Lab](/IntroClassFiles/Tools/IntroClass/ADHD/pcap/AdvancedC2PCAPAnalysis.md)</i></b>

<b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

***Finished with the Labs?***

Please be sure to destroy the lab environment!

[Click here for instructions on how to destroy the Lab Environment](/IntroClassFiles/Tools/IntroClass/LabDestruction/labdestruction.md)

---

  

  

  

  

  

  

  

  

  

  

  

 

 





