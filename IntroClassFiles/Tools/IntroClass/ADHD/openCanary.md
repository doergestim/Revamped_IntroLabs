![image](https://github.com/user-attachments/assets/068fae26-6e8f-402f-ad69-63a4e6a1f59e)


# For the Ubuntu VM


# OpenCanary 

**Goal:** Deploy a simple **OpenCanary** honeypot, trigger a few attacks (port scan, **SSH/SMB probe**, simple **HTTP request**), and observe **alerts**

---

# Setup

- Install mysql client for the use in the lab

```bash
sudo apt install mysql-client-core-8.0
```

- Go to its directory

```bash
cd ~/ADCD/openCanary
```

- Activate the **Virtual Environment**
```bash
source env/bin/activate
```

### Create and edit the config
Still inside your virtualenv:

- Create the default config (this prints the location)
```bash
opencanaryd --copyconfig
```

<!-- <img width="1627" height="138" alt="img0" src="https://github.com/user-attachments/assets/927fb7d9-526f-4099-aee4-2b1aa0a254f7" /> -->
<img width="1632" height="132" alt="img_01" src="https://github.com/user-attachments/assets/ab78d32a-f45d-4b5c-a811-4eaead4d0659" />

- Make sure it is there

```bash
sudo ls -l /etc/opencanaryd/opencanary.conf
```

<!-- <img width="753" height="26" alt="2026-03-08_12-10" src="https://github.com/user-attachments/assets/f6a107e2-d44c-47da-b86f-2a706cd71489" /> -->
<!-- <img width="1146" height="55" alt="img0_1" src="https://github.com/user-attachments/assets/7a4c4ad5-98ef-4fcc-b678-08f5d5352808" /> -->
<img width="1147" height="57" alt="img_02" src="https://github.com/user-attachments/assets/34f5b53c-fa09-4d05-bf74-26d657e9e674" />

- Now open the config and make small edits. Example uses `nano` (or `vi`):

```bash
sudo nano /etc/opencanaryd/opencanary.conf
```

- Inside the JSON config make these **minimal** changes to enable a few services and a log file:

1. Locate the `"device.node_id"` and set a friendly name like `"opencanary-lab"`

<!-- <img width="346" height="30" alt="2026-03-08_12-12" src="https://github.com/user-attachments/assets/b502f101-cb18-488b-9568-1f64adacf8c1" /> -->
```
"device.node_id": "opencanary-lab"
```

2. In the `"modules"` (or top-level service entries) enable the following:

```json
"ssh": {"enabled": true},
"ssh": {"port": 222},
"http": {"enabled": true},
"http": {"port": 8082},
"ftp": {"enabled": true},
"mysql": {"enabled": true},
"mysql": {"log_connection_made": true},
"telnet": {"enabled": true},
"portscan": {"enabled": true}
```

<!-- <img width="259" height="23" alt="2026-03-08_12-18" src="https://github.com/user-attachments/assets/f2a95943-cf22-45b8-96f8-5003d42d7389" /> -->
<!-- <img width="210" height="26" alt="2026-03-08_12-17" src="https://github.com/user-attachments/assets/1c3bbda9-f8e2-40b5-add3-ae09339a4de1" /> -->
<!-- <img width="219" height="27" alt="2026-03-08_12-16" src="https://github.com/user-attachments/assets/9b6f28ac-280b-47d6-a163-f933bd971d14" /> -->
<!-- <img width="219" height="42" alt="2026-03-08_12-15" src="https://github.com/user-attachments/assets/b720a41d-0c3f-43f3-96ec-486baa07bf8a" /> -->
<!-- <img width="204" height="49" alt="2026-03-08_12-14" src="https://github.com/user-attachments/assets/2831c467-4c46-4716-aa9c-005c144becd1" /> -->

<!-- <img width="1192" height="695" alt="img1" src="https://github.com/user-attachments/assets/7b99515d-bbd2-4585-bb94-53fea67e7a3c" /> -->
<!-- <img width="1185" height="492" alt="img2" src="https://github.com/user-attachments/assets/8775b8f4-f84a-42f7-a72e-0ea7a2810d86" /> -->

<img width="1200" height="722" alt="img_03" src="https://github.com/user-attachments/assets/4c648111-5328-4265-8d85-3ba704708235" />
<img width="1170" height="495" alt="img_04" src="https://github.com/user-attachments/assets/5e51e170-1986-4411-8b6f-c04c3598969f" />
<img width="1172" height="217" alt="img_05" src="https://github.com/user-attachments/assets/2e846bd7-02eb-478f-a6a6-96ba86691c77" />


- Save and exit with `Ctrl + x` and `y` and `Enter`


---

## Start

- Run it

>[!NOTE]
> Make sure you are in **~/ADCD/openCanary** with **venv** activated

```bash
opencanaryd --start
# To stop:
opencanaryd --stop
```

<img width="1898" height="541" alt="img_06" src="https://github.com/user-attachments/assets/9ad67aff-2d3f-429f-aebd-805a0e682931" />


- If you configured file logging as above, check the log:

```bash
sudo tail -n 50 /var/tmp/opencanary.log
```

<!-- <img width="1920" height="804" alt="image" src="https://github.com/user-attachments/assets/d2cf3738-0bd5-483f-9355-17d5b261a086" /> -->
<img width="1900" height="337" alt="img_07" src="https://github.com/user-attachments/assets/71514b52-d90a-4bd4-a1ed-c7a28429274a" />


---

## Simple attacker 
Perform these actions from a second terminal (or another device on the same network). Replace `<CANARY_IP>` with the IP address of the VM.

1. Port scan (nmap)
```bash
sudo nmap -sS -Pn -p 222,445,8082 localhost
```

<img width="139" height="74" alt="image" src="https://github.com/user-attachments/assets/303a017f-520f-4032-94bf-5a34548067f5" />


- **OpenCanary's** `portscan` module should flag the scan, so let's check!

```bash
sudo tail -n 50 /var/tmp/opencanary.log
```

<img width="1920" height="126" alt="image" src="https://github.com/user-attachments/assets/f07186ed-337b-4f92-b620-abaed7a41aab" />

BOOM!

2. SSH probe (attempt to connect)
```bash
ssh -o ConnectTimeout=5 fakeuser@localhost
```
This triggers the `ssh` canary

3. HTTP request
```bash
curl -I http://localhost/
```
This triggers the `http` canary logs

4. SMB enum
```bash
smbclient -L localhost -N
```

- After each action, check the canary log or journal on the honeypot host to see alerts:

```bash
sudo tail -n 50 /var/tmp/opencanary.log
```



***                                                                 
<b><i>Continuing the course? </br>[Next Lab](/IntroClassFiles/Tools/IntroClass/ADHD/Beelzebub.md)</i></b>

<b><i>Want to go back? </br>[Previous Lab](/IntroClassFiles/Tools/IntroClass/ADHD/HoneyPorts/HoneyPorts.md)</i></b>

<b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

***Finished with the Labs?***

Please be sure to destroy the lab environment!

[Click here for instructions on how to destroy the Lab Environment](/IntroClassFiles/Tools/IntroClass/LabDestruction/labdestruction.md)

---
