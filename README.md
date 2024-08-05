# how-to-make-ping-swipper
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

