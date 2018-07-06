## useful systemctl commands 

__to see running processes__ 
```
systemctl
systemctl | grep <service>
```

__check status__ 
```
systemctl status | grep kube 
```

__tell systemd to start at boot__
```
systemctl enable
```

__edit service__ 
```
systemctl edit kubelet.service
or 
vim /etc/systemd/system/kubelet.service
```

__if you change a systemd service file__ 
```
systemctl daemon-reload
systemctl restart kubelet.service
systemctl status kubelet.service
```

__if the service was not started__ 
```
systemctl enable kubelet
systemctl start kubelet
```

__display unit file__  
```
systemctl cat kubelet.service
```

__show low level properties of a unit__ 
```
systemctl show cron.service 
``` 
