
NMAP SCAN RESULTS

![](imagenes/Pasted%20image%2020240116193959.png)

Checking the web pages:

![](imagenes/Pasted%20image%2020240116194105.png)

![](imagenes/Pasted%20image%2020240116194115.png)

Both are apache default pages, lets enumerate with gobuster just to be sure, while the scans run , lets check what redis is:

![](imagenes/Pasted%20image%2020240116194428.png)

Apparently, we are dealing with a database, let's check how we can login with the command *redis-cli*:

![](imagenes/Pasted%20image%2020240116194554.png)

Lets try to login without a username:

![](imagenes/Pasted%20image%2020240116194642.png)

We are logged in! Lets search for the commons commands for redis:

![](imagenes/Pasted%20image%2020240116194756.png)

Before that, we run a nmap enumeration command to see what more info we can find(*it seems, there's no databases in this server...):

![](imagenes/Pasted%20image%2020240116195616.png)

*We found nothing of interest in the webpages*

![](imagenes/Pasted%20image%2020240116195716.png)

I find this excerpt in the [hacktricks](https://book.hacktricks.xyz/network-services-pentesting/6379-pentesting-redis#ssh) page:

![](imagenes/Pasted%20image%2020240116221503.png)

And in redis, I find the following directories:

![](imagenes/Pasted%20image%2020240116222305.png)

Let's try doing those steps using the directory /root/.ssh

![](imagenes/Pasted%20image%2020240116223235.png)

We export the key to the redis server and follow the steps above:

![](imagenes/Pasted%20image%2020240116223341.png)

We try to log into the ssh session:

![](imagenes/Pasted%20image%2020240116223413.png)

And we have root! We install unzip in this machine to extract the content of the zip:

![](imagenes/Pasted%20image%2020240116224147.png)

We set up a simple http server the target machine with the following command `python3 -m http.server -b <TARGET-MACHINE-IP> <PORT>`

We download the file in our machine with `wget <TARGET-MACHINE-IP>:<PORT>/root.zip´

![](imagenes/Pasted%20image%2020240116224535.png)

Now we use `zip2john` to print the hash of zip into a file and then crack it:

![](imagenes/Pasted%20image%2020240116224602.png)

We extract the contents of the file:

![](imagenes/Pasted%20image%2020240116224648.png)

We find the root flag, now me move on to the user flag:

![](imagenes/Pasted%20image%2020240116224853.png)

With this, we have pwned the machine.

# What I learned from this machine
- Privesc with redis


