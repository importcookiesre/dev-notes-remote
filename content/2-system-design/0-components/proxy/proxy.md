


# create a proxy server 

- create a proxy server using 
	- Amazon EC2 machine
	- squid software


## what is squid ? 

- Squid is a **proxy caching server** that forwards client requests to the internet.
- We can configure it for **forward proxy** (outbound requests, e.g., web scraping, security filtering) or **reverse proxy** (load balancing).
- Here, we want **forward proxy**.

## Why EC2 + Squid?

- We get a **dedicated IP** from EC2 → avoids rate limiting/IP bans.
- Squid lets us **control access** (whitelisting, authentication).
- Scales well → we can add multiple EC2s and rotate IPs.

# steps : 

### 1. spin an ec2 instance : 

- Use **Ubuntu 22.04** or **Amazon Linux 2**.
- Instance type: `t2.micro` (free tier) or bigger if high load.
- Open **security group** inbound rules:
    - Allow **SSH (22)** → your IP (or from anywhere) 
    - Allow **Custom TCP Proxy (3128 by default)** → your IP (or 0.0.0.0/0 avoid for security).

## 2. Install squid:

```bash
sudo apt update -y
sudo apt install -y squid apache2-utils 
```

## 3. configure squid : 

```bash
cd /etc/squid
sudo htpasswd -c ./passwords username
# enter password
sudo nano conf.d/debian.conf
```

- add these four lines : 
```bash
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwords
auth_param basic realm proxy

acl authenticated proxy_auth REQUIRED
http_access allow authenticated
```

- restart the server
```bash
sudo systemctl restart squid
sudo systemctl status squid # check status
```


## 4. start using : 

```
http://username:password@ec2_public_id:3128
```

check if it is working
```bash
curl -x http://myuser:mypassword@EC2_PUBLIC_IP:3128 https://api.ipify.org
```

## 5. Security : 

- **Don’t leave open proxy** → always whitelist IPs or enable authentication.
- Rotate **access logs**: `/var/log/squid/access.log` to monitor.
- Use **Elastic IP** → your proxy keeps same IP even if instance restarts.
- Dont allow access from everywhere : whitelist your ip and use.


# learn to scale proxy



# article 

Proxies are everywhere. We use it for web scraping, controlling and monitering internet access in an organization, or bypassing rate limits. Instead of pay
### 