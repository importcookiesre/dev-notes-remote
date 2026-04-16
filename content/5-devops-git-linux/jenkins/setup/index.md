



# If we are using vm and we want to access jenkins outside the vm in our browser then 

**Enable firewall:**

```
sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp

sudo firewall-cmd --reload
```
 