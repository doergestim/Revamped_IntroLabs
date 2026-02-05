![image](https://github.com/user-attachments/assets/068fae26-6e8f-402f-ad69-63a4e6a1f59e)

Spidertrap
==========

Website
-------

<https://github.com/adhdproject/spidertrap>

Description
-----------

Trap web crawlers and spiders in an infinite set of dynamically
generated webpages.

Install Location
----------------

`~/ADCD/spidertrap`

Usage
-----

```bash
python3 spidertrap.py --help
```
```
Usage: spidertrap.py [FILE]

FILE is file containing a list of webpage names to serve, one per line.
If no file is provided, random links will be generated.
```

## Example 1: Basic Usage

- Let's get started by getting into the proper directory:

```bash
cd ~/ADCD/spidertrap
```

<img width="436" height="50" alt="image" src="https://github.com/user-attachments/assets/d7a0857e-79e1-47f4-a622-3b644b223c0e" />


- Now, lets start Spidertrap by running the following command:

```bash
python3 spidertrap.py
```

<img width="612" height="71" alt="image" src="https://github.com/user-attachments/assets/c16977ac-6f1d-47c0-8f08-69f1811f1593" />


- Then visit the following site in a web browser:

```
http://localhost:8000
``` 

- You should see a page containing randomly generated links. If you click on a link it will take you to a page with more randomly generated links.

![](/IntroClassFiles/Tools/IntroClass/ADHD/Spidertrap/links.png)

![](/IntroClassFiles/Tools/IntroClass/ADHD/Spidertrap/morelinks.png)

## Example 2: Providing a List of Links

- For this example, we are going to start Spidertrap again, but this time, we are going to give it a file to generate its links.

- Let's start Spidertrap again but with the following options:

```bash
python3 spidertrap.py directory-list-2.3-big.txt
```

>[!TIP]
>
>You may need to press `ctrl + c` to kill your existing Spidertrap session.

<img width="851" height="67" alt="image" src="https://github.com/user-attachments/assets/78e53f09-e606-4067-8590-9a9d7b493b51" />


- Then visit the following site in a web browser:

```bash
http://localhost:8000
```
 
- You should see a page containing links taken from the file. If you click on a link it will take you to a page with more links from the file.

![](/IntroClassFiles/Tools/IntroClass/ADHD/Spidertrap/links2.png)

![](/IntroClassFiles/Tools/IntroClass/ADHD/Spidertrap/morelinks2.png)

## Example 3: Trapping a Wget Spider

- For this example, follow the instructions in [Example 1: Basic Usage](#example-1-basic-usage) or [Example 2: Providing a List of Links](#example-2-providing-a-list-of-links) to start Spidertrap. 

- Once Spidertrap starts, open a new Kali Linux terminal. Do this by clicking the icon in the taskbar:

![](/IntroClassFiles/Tools/IntroClass/ADHD/Spidertrap/TaskbarKaliIcon.png)

- We are going to use `wget` to mirror the website. 

>[!IMPORTANT]
>
>`wget` will run until either it or Spidertrap is killed.
>To stop the command output, type `ctrl + c`

- Let's run the following command:

```bash
sudo wget -m http://127.0.0.1:8000
```

![](/IntroClassFiles/Tools/IntroClass/ADHD/Spidertrap/wgetcommand.png)

- When finished, type `ctrl + c` to kill wget.

***                                                                 
<b><i>Continuing the course? </br>[Next Lab](/IntroClassFiles/Tools/IntroClass/ADHD/Cowrie/Cowrie.md)</i></b>

<b><i>Looking for a different lab? </br>[Lab Directory](/IntroClassFiles/navigation.md)</i></b>

***Finished with the Labs?***

Please be sure to destroy the lab environment!

[Click here for instructions on how to destroy the Lab Environment](/IntroClassFiles/Tools/IntroClass/LabDestruction/labdestruction.md)

---
