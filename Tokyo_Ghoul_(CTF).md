Hello, today we wil do Tokyo Ghoul ctf by devalfo and rockyou.txt ( https://tryhackme.com/room/tokyoghoul666 )

After connecting to website's VPN we can start scanning our target machine for open ports.

<img width="755" alt="Screenshot 2023-07-18 at 12 25 39" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/dffc86ed-2f9e-4b93-9149-cb887c8e55a3">
 -sV  is for version of software running on port
 -O for OS running on a target machine

 How we can see we can have open ports: 21 (ftp), 22(ssh) and 80 (http)

 Let's enter target's ip adress into the browser

 
<img width="723" alt="Screenshot 2023-07-18 at 12 31 15" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/fdd7cabf-4df0-48aa-9b35-d7d77ef536de">

We can see the link which redirects us to other page.

<img width="676" alt="Screenshot 2023-07-18 at 12 31 23" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/f5020b82-ba7d-4bbf-9eac-314c446a19af">

So, lets investigate it by viewing page source. We will find there note and intructions for us.

It said that we can connect to ftp server by **Anonymous** user.

After logining we see that we have directory inside.

<img width="792" alt="Screenshot 2023-07-18 at 12 32 12" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/a1a360de-a847-43e8-8a2f-d1c2afa957e2">

After investigating we can find 2 files. Lets download them to our pc. (mget command for multipile download)

<img width="764" alt="Screenshot 2023-07-18 at 12 39 15" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/d0dc3846-5d76-48c9-abd1-835635968d70">

Alright, I used _exiftool_ to check what the files and their metadata.

<img width="380" alt="Screenshot 2023-07-18 at 12 49 12" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/52e34955-9c94-42ce-a114-ff36a512e2b0">

This file is executable so lets give it execute permission (chmod +x)


<img width="539" alt="Screenshot 2023-07-18 at 12 52 01" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/62bce1a6-dbfc-4076-ad4c-3f3b88e9559a">

It;s askink "What we are looking for?" I type random word and it said to look inside of it.

So, how w lnow it's a binary, we can use **strings** command to find humman readable words.

<img width="292" alt="Screenshot 2023-07-18 at 12 52 18" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/dc2d8680-d964-498e-90d5-4b4d2e40eaf6">

Okay, let's enter it to  our program and it will give us the key.

We have also image from pur ftp. So, it can be steganography lets use **steghide** command. It's asking us for password. Try enter key what we got.

<img width="291" alt="Screenshot 2023-07-18 at 12 52 56" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/2c33515e-14da-4970-9e50-6a99d1b9481a">

Okay, it give us a .txt file lets invsetigate it.

<img width="371" alt="Screenshot 2023-07-18 at 12 53 43" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/64d77ea8-5f5d-4aa4-aca9-ad7df1aad897">

This is Morse code and we need to crack it. You can search for any Morse decoder, I'm using CyberChef. 
After decoding we can see Base 64 code lets decode it too.

<img width="765" alt="Screenshot 2023-07-19 at 11 12 54" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/db01b12e-e4b0-421f-90d5-a6256369b819">


Okay, looks like directory for target web server, let go in it.

<img width="994" alt="Screenshot 2023-07-19 at 11 20 21" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/258f0b89-42f0-42b9-bf44-f7629ca04f4d">

It's asking us to scan it. I used **gobuster** for this directory with common wordlist.

It will show us hidden directory. When we enter it it will ask yes or no. It must show us flowe.gif (but for some reason it's not showing on my machine).

If we look on address bar we can notice that server send request to directory where is storing .gif file. we can try common __**local file intrusion attack**__, by putting ../../

<img width="1012" alt="Screenshot 2023-07-19 at 11 31 20" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/f3375241-1e4e-4819-8bb2-ed12166273fe">

It's blocking it, we can try same attack but by encoding our URL request ( I encode ../../../etc/passwd)

We got to password file where is storing username of this machine and password in hash format

<img width="1008" alt="Screenshot 2023-07-19 at 11 32 47" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/87490017-abc8-4a32-831a-b7084e90cfa4">

To crack our hash we can use **JohnTheRipper*

<img width="683" alt="Screenshot 2023-07-19 at 11 47 48" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/fda2688b-964f-4add-a826-79ad0aa47a34">

So we got password and username. Let's try to connect through SSH.
 Yep, we are in. Let's investigate it.

<img width="808" alt="Screenshot 2023-07-19 at 11 49 47" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/c3f2d17e-2c9c-4e87-831d-e78c0c80a48b">

We found user.txt flag. After it I always use sudo -l command to find what privilieges we have.


<img width="952" alt="Screenshot 2023-07-19 at 11 50 43" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/b7312733-a5ba-44a3-8607-6582e0737b0d">

We can use as root python3 and jail.py. Jail.py is not writable so we cant edit it. If we look inside of program we can see that it blocks some of the inputs. 

I didn't know what it means, so I googled it and found about Python Prison method and how to bypass it. 

If we enter this input (__builtins__.__dict__[‘__IMPORT__’.lower()](‘OS’.lower()).__dict__[‘SYSTEM’.lower()](‘cat /root/root.txt’)) to our program it must show to us root flag.

So, thats the end. Thank you. Hope ypu enjoyed it and learned new things. Keep practicing. Cheers.

<img width="989" alt="Screenshot 2023-07-19 at 12 13 31" src="https://github.com/Mak1ma-xx/mak1ma-xx.tryhackme_writeups/assets/106231648/835021cd-cd5c-48d3-8bbe-4af8c335b08f">

