



## 1. Forward Proxy vs Reverse Proxy :

### 1. Forward Proxy : 

 In a traditional HTTP connection, a client sends a request directly to a server. However, with a forward proxy (such as a VPN), the client sends the request to the proxy, which then forwards it to the server. The server is unaware of the original client; it only interacts with the proxy. This setup is useful for privacy and bypassing geo-restrictions.

![[1_lu9bZvs5QTQI16uQEpxYWw.webp]]
### 2. Reverse Proxy : 

 In contrast, a reverse proxy sits between the client and multiple servers. The client sends a request to the reverse proxy, which then decides which server should handle the request. The client remains unaware of which specific server processes the request. Nginx is a popular choice for a reverse proxy because of its efficiency and flexibility.

![[1_Hv39F4Fj4oxgZXnlBkYfLA.webp]]