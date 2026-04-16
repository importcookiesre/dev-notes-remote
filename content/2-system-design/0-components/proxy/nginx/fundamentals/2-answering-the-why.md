


## 1. Why we need a reverse proxy ?


- The **Why ?** Do you want your CEO answering the phone ?
	
	Think of your **Go Application** as the **CEO** of a company. Think of **NGINX** as the **Receptionist/Security Guard**.
	You _could_ publish the CEO's direct phone number to the world.
	- **The Result:** The CEO spends 90% of their time answering wrong numbers, spam calls, and giving directions to the restroom, instead of running the company.
	
	**Why you need NGINX (The Receptionist):**
	1. **Static Files (The "Directions to Restroom"):**
	    - Serving images, CSS, and JS files is "dumb" work. It requires zero logic.
	    - If you force your Go app to serve images, you are wasting expensive CPU cycles that could be calculating business logic.
	    - **NGINX is specialized** to serve static files faster than any application server by using kernel-level optimizations (like `sendfile`).
	2. **SSL/TLS Termination (The "Security Check"):**
	    - Encrypting and decrypting HTTPS traffic is computationally heavy.
	    - Instead of making your Go app handle the encryption, NGINX handles the "handshake" and decryption. It then passes plain HTTP to your Go app (which runs safely inside your private network). This is called **SSL Termination**.
	3. **Security & Buffering (The "Bouncer"):**
	    - **Slowloris Attacks:** If a malicious user sends a request _very slowly_ (1 byte per second), a Go thread/goroutine might stay open waiting for it. NGINX is designed to handle thousands of these slow connections without breaking a sweat, only sending the request to Go when it is fully received.
	    - **Port Binding:** To run on port 80/443, a program needs `root` privileges. You generally don't want your custom application running as root. You let NGINX (highly audited, secure) run as root, and your app runs as a normal user on port 8080.


## 2. Why Nginx is called web server ? 

| Type              | Definition                                                                                                                       | Example                                     |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| **Web Server**    | Software designed primarily to serve **static content** (HTML, CSS, Images) over HTTP. It doesn't usually execute complex logic. | NGINX, Apache, IIS                          |
| **App Server**    | Software designed to execute **dynamic business logic**, talk to databases, and generate data on the fly.                        | Go Binary, Python (Gunicorn), Java (Tomcat) |
| **Reverse Proxy** | A traffic director. It sits in front of other servers and forwards client requests to them.                                      | NGINX, HAProxy, Traefik                     |


Nginx was build to work as a web server, serving static files really fast. However, it is so efficient at handling connections that people realized it could be really good reverse proxy.

