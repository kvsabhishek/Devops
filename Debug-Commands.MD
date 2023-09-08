* **Check if port is open:**
```
netstat -lntp 
```

* **Check the service status:**
```
systemctl status service_name
```
```
service service_name status
```

* **Check if host is able to communicating with another host port:**
```
telnet ip-address port 
```

* **Check if the process is running:**
```
ps aux | grep <service_name>
```