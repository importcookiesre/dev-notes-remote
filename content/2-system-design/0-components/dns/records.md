

## 2. DNS record types : 

1. **A Record :**
	**What it does:**
	- Maps a domain → **IPv4 address**
	
	**How it works:**
	- Browser receives an IP and directly connects to that server.
	
	**Use-cases:**
	
	- Self-hosted backend
	- VPS (DigitalOcean, AWS EC2, Linode, etc.)
	- Bare-metal server
	- Internal microservice endpoints (in private DNS)

2. **AAAA Record** : 
	**What it does:**
	
	- Maps a domain → **IPv6 address**
	
	**How it works:**
	
	- Same as A record, but for IPv6-enabled networks.
	
	**Use-cases:**
	
	- Modern hosting
	- Dual-stack servers
	- High-performance networks
	- Regions where IPv6 is mandatory

3. **CNAME Record**
	
	**What it does:**
	
	- Maps a domain → **another domain**, not an IP.
	
	**How it works:**
	
	- Browser first resolves the CNAME target domain
	- Then resolves that domain’s A/AAAA record
	- Finally connects to the IP
	
	**Use-cases:**
	
	- Vercel, Netlify, GitHub Pages
	- SaaS subdomain mapping
	- Multi-region load balancers
	- CDN endpoints
	- Auto-rotating IPs
	
	**Key concept:**
	
	- A CNAME cannot exist at the root domain (`example.com`).
	- It can only work on subdomains (`www.example.com`, `api.example.com`).  
	    (Unless using ALIAS/ANAME from DNS providers.)

4. **MX Record**
	
	**What it does:**
	
	- Controls **where emails are delivered** for your domain.
	
	**How it works:**
	
	- Mail servers look at MX records
	- Deliver mail to the listed mail service
	
	**Use-cases:**
	
	- Setting up Gmail Workspace
	- Setting up Outlook/Zoho
	- Self-hosted mail servers

5. **TXT Record**

	**What it does:**
	
	- Holds plain text data for verification and policies.
	
	**How it works:**
	
	- External services read this record to verify domain ownership or security settings.
	
	**Use-cases:**
	
	- Google Search Console
	- SPF (email anti-spam)
	- DKIM / DMARC
	- API integrations
	- Domain verification for SaaS

6. **NS Record**

	**What it does:**
	
	- Defines which **nameservers** hold your domain’s DNS records.
	
	**How it works:**
	
	- The root and TLD servers use NS records to know “where to ask for real DNS data.”
	
	**Use-cases:**
	
	- Pointing a domain to Cloudflare
	- Changing DNS providers
	- Delegating subdomains

7. **SOA Record** : 
	
	**What it does:**
	
	- “Start of Authority” — metadata for the domain.
	
	**How it works:**
	
	- Tells DNS resolvers:
	    
	    - primary nameserver
	    - refresh intervals
	    - retry rules
	    - serial number for zone updates
	
	**Use-cases:**
	
	- Always auto-managed by DNS provider
	- Rarely edited
	- Needed for DNS zone transfers

8. **CAA Record**
	
	**What it does:**
	
	- Restricts **which Certificate Authorities** can issue SSL certificates for your domain.
	
	**How it works:**
	
	- CA checks CAA before issuing your SSL cert.
	
	**Use-cases:**
	
	- Security-hardening
	- Prevent unauthorized SSL certificate issuance
	- Corporate and enterprise infra

8. **ALIAS / ANAME** (Provider-specific records)
	
	**What they do:**
	
	- Allow “CNAME-like” behavior at the **root domain**.
	
	**How they work:**
	
	- Provider resolves the CNAME internally and returns an A/AAAA to the client.
	
	**Use-cases:**
	
	- Root-level CNAME (e.g., `example.com` → Vercel, CloudFront, Netlify)
	- Modern hosting setups

