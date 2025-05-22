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

`/opt/spidertrap/`

Usage
-----

`/opt/spidertrap$` **`python3 spidertrap.py --help`**

        Usage: spidertrap.py [FILE]

        FILE is file containing a list of webpage names to serve, one per line.
        If no file is provided, random links will be generated.


Example 1: Basic Usage
----------------------

Start Spidertrap by opening a terminal, changing into the Spidertrap
directory, and typing the following:

Let's get started by opening a Kali terminal. 
You can do this by right clicking the icon on the desktop by selecting open...



First, let's get your Kali Linux systems IP address by running the following command:

<pre>ifconfig</pre>

Next, let's cd intto the proper directory:

`cd /opt/spidertrap`

Now, lets start it:

`/opt/spidertrap$` **`python3 spidertrap.py`**

        Starting server on port 8000...

        Server started. Use <Ctrl-C> to stop.

![image](https://github.com/user-attachments/assets/e978648f-f43d-4cff-acdb-f524a5e1a571)

        
    

Then visit http://<YOUR_LINUX_IP>:8000 in a web
browser. You should see a page containing randomly generated links. If
you click on a link it will take you to a page with more randomly
generated links.

![](Spidertrap_files/image001.png) ![](Spidertrap_files/image002.png)

Example 2: Providing a List of Links
------------------------------------


Start Spidertrap. This time give it a file to use to generate its links.

You may need to press ctrl+c to kill your existing spidertrap session.

Now, restart it with the following options:

`/opt/spidertrap$` **`python3 spidertrap.py directory-list-2.3-big.txt`**

        Starting server on port 8000...

        Server started. Use <Ctrl-C> to stop.

![image](https://github.com/user-attachments/assets/3cdc8570-9639-4dbd-9348-ed67f4c836a1)


Then visit http://<YOUR_LINUX_IP>:8000 in a web
browser. You should see a page containing links taken from the file. If
you click on a link it will take you to a page with more links from the
file.

![](Spidertrap_files/image003.png) ![](Spidertrap_files/image004.png)

Example 3: Trapping a Wget Spider
---------------------------------

Follow the instructions in [Example 1: Basic Usage] or
[Example 2: Providing a List of Links] to start Spidertrap. Then
open a new Kali Linux terminal and tell wget to mirror the website. Wget will run
until either it or Spidertrap is killed. Type Ctrl-c to kill wget.

`$` **`sudo wget -m http://127.0.0.1:8000`**

        --2013-01-14 12:54:15-- http://127.0.0.1:8000/

        Connecting to 127.0.0.1:8000... connected.

        HTTP request sent, awaiting response... 200 OK

        <<<snip>>>

        HTTP request sent, awaiting response... ^C


![image](https://github.com/user-attachments/assets/8369ef4c-1298-4321-a4b2-40a94cd2de16)


[Return To Lab List](https://github.com/strandjs/IntroLabs/blob/master/IntroClassFiles/navigation.md)


