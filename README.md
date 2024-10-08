# How-To-Make-Ping-Swipper CyberSecurity projects beginner level
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
ping -c 1 10.0.2.15| grep "64 bytes" | cut -d " " -f 4 | tr -d " : "
```
In next Step, We have to write a for loop for continuosly changing the value of the last octect of ip addresses from 1 to 254
Now, our program will look like :-

```
#!/bin/bash
for ip in `seq 1 254`;do
ping -c 1 10.0.2.15| grep "64 bytes" | cut -d " " -f 4 | tr -d " : "&
done
```
how to run our ping sweep in kali, the cmd will be :-
```
./ipsweep.sh $1 $2 
```
Note:
 1. So, we can provide different arguments value to our program like this.
 2. $0 ->  ./ipsweep.sh  (that's why second operation starts from $1)
 3. On the time of running ping swipper: first three octet will be defined by $1 (10.0.2 -> $1)
 
Updated program will look like :-

```
#!/bin/bash
for ip in `seq 1 254`;do
ping -c 1 $1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d " : "&
done
```
kali Cmd:
```
// this helps specifying multiplle network easily.
./ipsweep.sh 10.0.2
```

Always remenber to change permissions our program must need execution permissions :-
cmd for that will be:
```
chmod +x ipsweep.sh
```
| Commmands | Working |                 
| ------ | ------ |
| chmod | Changing mod - giving or removing read write execute permissions. |
| +x | means granting execution permissions  |
| ipseep.sh  | Name of your ping swipper |



You can search for chmod to learn more things.

```
// to check if our program got the permission or not, we will use
ls -la
```
Now, when you run this bash script it will gives you the list of the active ip addresses :
cmd to run script in kali :
```
sudo ./ipsweep.sh 10.0.2

```
Results will be like :

![image](https://github.com/user-attachments/assets/6dfe11fb-8a8e-45ae-bdc7-e4d8c6ceaed7)

if someone put wrong input , we should give them some error let's see:-
```
// the updated program will be like

#!/bin/bash

# Check if the IP prefix was provided as an argument
if [ -z "$1" ]; then
    echo "You forgot to provide an IP address prefix!"
    echo "Syntax: ./ipsweep.sh 10.0.2"
    exit 1
else
    # Loop through the IP range
    for ip in $(seq 1 254); do
        ping -c 1 $1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d " :" &
    done

    # Wait for all background processes to complete
    wait
fi

```
If ip address prefix is not provided result will be like:

![image](https://github.com/user-attachments/assets/894de7f1-0d9a-460a-94db-b5a0d5ae561a)

Suppose you want to save these ip address for further attack you can use command:-
```
// this command will save the active ip addresses in the ' ip.txt ' file 
sudo ./ipsweep.sh 10.0.2 > ip.txt

```

![image](https://github.com/user-attachments/assets/e1388f45-0e41-472b-ab2b-659b2d18daae)

Usage of " Ping Swipper " :
We can use this program in ' nmap scanning ' This Command below will run the Scanning one by one on every ip address and can help in automation of the process :
```
for ip in $(cat ip.txt); do nmap $ip& done
```
So, this was the whole process to write a bash script to make a PING SWIPPER.


< ----------------------------------------------------------------------------------------------------->



