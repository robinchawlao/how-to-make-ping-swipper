# How-To-Make-Ping-Swipper
_This bash script automates the process of IP scanning by simultaneously pinging a range of IP addresses. It sequentially runs Nmap scans on each IP address, streamlining the automation of network scanning tasks._

Take any random ip address , in this case i will take my own ip address which i will get using ifconfig command :

```
ifconfig
```
Now, take your ip address suppose: 
```
10.0.2.15
```
Try and ping your ip address :

```
ping 10.0.2.15
```


It will look like this , if it is not sending icmp requests then ip address must not be valid :

![pingsweep1](https://github.com/user-attachments/assets/2119254d-e1e7-4262-a9a9-640b978d86ea)

Now we don't need that much requests we just need one :
the cmd below will send only one request:

```
ping 10.0.2.15 -c 1
```

We will store this request data in a txt file ' ip.txt '

```
ping 10.0.2.15 -c 1 > ip.txt 
```

Some commands to know before starting scripting :
| Commmands                   | Working |
| ------                      | ------ |
| cat                         | To edit any file or can create new file for ex:- " cat ip.txt "|
| grep                        | It can extract lines with certain keyword . for example:- " grep "64 bytes" " |
| cut -d " " -f 4             | cut - for removing something from the line, d - delimiter , f - count till which we want to go. (this line will remove elements till 4th space) |
| tr -d ":"                   | tr means translate (this line will remove the ' : ' present in the line |
|  pipe operator         | used to give more than one cmd at the same time in one line |

So the final command after combining all these will be:-

```
cat ip.txt | grep "64 bytes" | cut -d " " -f 4 | tr -d " : "
```

This whole command will just get the ip address from the ping command will store it in the ip.txt which will confirm that ip address is active.
now ping your ip address with the above command:-

```
ping -c 1 10.0.2.15| cat ip.txt | grep "64 bytes" | cut -d " " -f 4 | tr -d " : "
```
Now, this whole command will be for one ip address. So, We want to work it for range of ipaddress for using in automation and ease of several tasks.
We will write a for loop in our editor which will be continuosly changing the ip addresses from 1 to 254 So, Our program will look like :-

```
#!/bin/bash
// declaring that it's a bash script

