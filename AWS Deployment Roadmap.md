# ⚙️ AWS-Based Deployment Architecture for MERN

| Component          | AWS Service                                                           | Purpose                             |
| ------------------ | --------------------------------------------------------------------- | ----------------------------------- |
| **Frontend** | S3 + CloudFront                                                       | Static React app, globally cached   |
| **Backend**  | EC2 or AWS Elastic Beanstalk, Basic Linux, Nginx, SSH, PM2, firewalls | Runs Node.js + Express + Socket     |
| **Database** | MongoDB Atlas (preferred)                                             | Managed MongoDB in the cloud        |
| **Media**    | Cloudinary (still best for images)                                    | Or use S3 if needed                 |
| **Auth**     | JWT in your app                                                       | You can integrate AWS Cognito later |
| **Domain**   | Route 53                                                              | Domain + SSL setup                  |
| **Security** | IAM Roles + Security Groups                                           | Protect EC2, S3, and DB access      |

# --------------------------------------------------------------------------------------------------------------

## ----PHASES

✅ Your Full MERN Deployment & DevOps Learning Plan

Each **phase** builds on the previous one. Time estimates are based on a focused developer like you (2–3 hours/day).

#### 🔹 **Phase 1: Manual Deployment (Days 1–5)**

> 🎯 Goal: Deploy your MERN app to AWS manually

| Task                  | Tools                 | Time      |
| --------------------- | --------------------- | --------- |
| Launch EC2 + SSH      | EC2, Key Pair         | 1 day     |
| Host backend          | Node.js, PM2, Express | 1 day     |
| Host frontend         | React, S3, CloudFront | 1–2 days |
| Connect MongoDB Atlas | Mongoose, env vars    | 0.5 day   |
| Configure SSL, Nginx  | Certbot, Nginx        | 0.5 day   |

#### 🔹 **Phase 2: CI/CD + Automation (Days 6–10)**

> 🎯 Goal: Automate your frontend and backend deployments

| Task                 | Tools                             | Time      |
| -------------------- | --------------------------------- | --------- |
| Frontend CI/CD       | GitHub Actions → S3 + CloudFront | 1–2 days |
| Backend CI/CD        | GitHub Actions → EC2 (SSH + PM2) | 1–2 days |
| Logging & Monitoring | PM2 logs, CloudWatch, journalctl  | 0.5 day   |
| Backup + Restore     | MongoDB Atlas backups, exports    | 0.5 day   |
| Zero-downtime deploy | PM2 + Nginx swap config           | 1 day     |

#### 🔹 **Phase 3: Performance & Scalability (Days 11–14)**

> 🎯 Goal: Make your app handle real-world traffic and load

| Task                           | Tools                               | Time    |
| ------------------------------ | ----------------------------------- | ------- |
| Optimize asset delivery        | CloudFront caching, Brotli, Gzip    | 0.5 day |
| Load testing & benchmarking    | ApacheBench, k6, Postman            | 1 day   |
| Caching backend APIs           | Redis, memory-cache                 | 1 day   |
| Lazy loading, code splitting   | React + dynamic imports             | 1 day   |
| CDN tuning for frontend assets | Cache-Control headers, invalidation | 0.5 day |

#### 🔹 **Phase 4: Advanced Security & Reliability (Days 15–18)**

> 🎯 Goal: Harden your app like a production-grade SaaS company

| Task                          | Tools                         | Time    |
| ----------------------------- | ----------------------------- | ------- |
| HTTPS/SSL Auto Renewal        | Certbot + Cron                | 0.5 day |
| Environment secret management | AWS Parameter Store,`.env`  | 1 day   |
| Rate limiting / protection    | Express-rate-limit, Helmet.js | 1 day   |
| File validation & antivirus   | Multer, Cloudinary, ClamAV    | 1 day   |
| Audit logs                    | Winston, custom logger        | 0.5 day |

#### 🔹 **Phase 5: Production-Level Features (Days 19–24)**

> 🎯 Goal: Add features that companies expect in SaaS/eCommerce

| Feature                        | Tools / Libraries            | Time     |
| ------------------------------ | ---------------------------- | -------- |
| Image optimization             | Cloudinary transformations   | 0.5 day  |
| WebSocket auto-reconnect       | Socket.io reconnect options  | 0.5 day  |
| Multi-region S3 hosting        | AWS S3 + CloudFront origins  | 1 day    |
| Email & SMS integration        | Nodemailer, SendGrid, Twilio | 1 day    |
| Payment gateway integration    | Razorpay, Stripe, PayPal     | 1.5 days |
| Audit trails for orders/admins | Custom logger + DB writes    | 0.5 day  |

#### 🔹 **Phase 6: Resume Polish + Demo Setup (Days 25–28)**

> 🎯 Goal: Make your project *shine* in interviews

| Task                                      | Tools / Services         | Time    |
| ----------------------------------------- | ------------------------ | ------- |
| Write full documentation                  | Markdown + screenshots   | 0.5 day |
| Record a demo video                       | OBS, Loom                | 0.5 day |
| Host live version with domain             | Route 53, S3, EC2        | 0.5 day |
| Include tech stack + deployment in resume | PDF + GitHub             | 0.5 day |
| Add README badges, GitHub info            | Shields.io, GitHub stats | 0.5 day |

#### 📌 Summary: 6 Phases (Total ~28 Days)

| Phase                          | Focus Area                         |
| ------------------------------ | ---------------------------------- |
| **Phase 1**(Days 1–5)   | Manual deployment to AWS           |
| **Phase 2**(Days 6–10)  | CI/CD, monitoring, automation      |
| **Phase 3**(Days 11–14) | Performance & load scaling         |
| **Phase 4**(Days 15–18) | Security, SSL, secret management   |
| **Phase 5**(Days 19–24) | Real-world SaaS/eCommerce features |
| **Phase 6**(Days 25–28) | Resume, live demo, project polish  |

##### 🧠 What You Can Say in Interviews After This

> “I deployed my full-stack MERN eCommerce app on AWS using EC2, S3, CloudFront, Nginx, and PM2. I automated CI/CD with GitHub Actions, implemented SSL, secure headers, logging, real-time features using WebRTC and sockets, and integrated payment systems. I also set up monitoring, zero-downtime deployments, and wrote complete documentation and a demo video for presentation.”

This shows you are **industry-ready** with **DevOps + backend + security + frontend** knowledge — and that’s exactly what **10+ LPA** employers want.

---

## ----PHASE 1

#### 🧠 What You Need to Learn

| Skill                             | Description                                   | Estimated Time |
| --------------------------------- | --------------------------------------------- | -------------- |
| ✅**EC2**                   | Launch instance, SSH, Linux basics, firewalls | 1–2 days      |
| ✅**PM2**                   | Run backend persistently                      | 0.5 day        |
| ✅**Nginx**                 | Reverse proxy to your backend                 | 1 day          |
| ✅**MongoDB Atlas**         | Use with EC2 securely                         | 0.5 day        |
| ✅**S3 + CloudFront**       | Host React frontend with CDN                  | 1–2 days      |
| ✅**Route 53 / SSL**        | Optional: custom domain + HTTPS               | 1–2 days      |
| ✅**Environment Variables** | Securely manage configs                       | 0.5 day        |

#### 🗓️ Total Time Estimate

| Your Profile                               | Estimated Time                                |
| ------------------------------------------ | --------------------------------------------- |
| **Beginner to AWS**                  | **~5 to 7 days** *(2–3 hrs/day)*     |
| **Comfortable with Linux & Servers** | **~3 to 4 days**                        |
| **Super focused (full-time dev)**    | **1–2 days**(possible if focused hard) |

#### 🧭 Suggested Learning Plan (5-Day Sprint)

##### **Day 1: EC2 + SSH**

* Launch Ubuntu EC2
* SSH into it
* Install Node, Git, etc.
* Set up a basic Hello World Express app
* Deploy with `node` or `pm2`

##### **Day 2: Backend Setup**

* Clone your real backend
* Add `.env`, MongoDB Atlas URI
* Secure routes (firewall, port 3000 → 80)
* Test REST API

##### **Day 3: Nginx + HTTPS**

* Install Nginx
* Set up reverse proxy: Nginx → Express
* Add domain + Let’s Encrypt SSL (optional but impressive)
* Test HTTPS + JWT APIs

##### **Day 4: Frontend Hosting (S3 + CloudFront)**

* Build React app
* Upload to S3 bucket
* Set bucket as static site
* Connect with CloudFront
* (Optional: map domain via Route 53)

##### **Day 5: Polish + Document**

* Use PM2 startup script
* Automate deployment via GitHub
* Add security groups
* Write down all steps in a README

---

## ----Networking knowledge

Because knowing **computer networking and internet concepts** is *critical* for backend, full-stack, DevOps, and cloud jobs — especially when you're deploying a MERN project on AWS with WebRTC, sockets, JWT, and security in mind.

### ✅ Why Networking Knowledge Matters for You:

* You'll  **deploy APIs** , open  **ports** , configure **firewalls**
* You'll need to know how **clients (frontend)** talk to **servers (backend)**
* You'll troubleshoot  **EC2 access** ,  **WebRTC peer connections** ,  **Cloudinary uploads** , and **JWT security**
* You’ll sound  **very solid in interviews** , especially system design and DevOps rounds

### 🧠 Let’s Break It Down by Phases

#### 🟩 **Phase 1 – Developer-Level Networking**

📅 Time: 3–4 days

🎯 Focus: Enough to **deploy, secure, and debug** MERN apps

| Topic                                 | Why It’s Important                  |
| ------------------------------------- | ------------------------------------ |
| What is an IP address (IPv4 vs IPv6)  | Every server/device needs one        |
| DNS – Domain Name System             | How `fitlab.in`maps to an IP       |
| HTTP vs HTTPS                         | How browsers talk to servers         |
| Ports (80, 443, 3000, 27017)          | You’ll open/close ports on EC2      |
| TCP vs UDP                            | WebRTC = UDP, APIs = TCP             |
| HTTP methods (GET, POST, PUT, DELETE) | RESTful API basics                   |
| Status codes (200, 404, 500, etc.)    | Crucial for API debugging            |
| Firewalls & security groups           | Control access to EC2/Sockets        |
| NAT & public/private IP               | Why EC2 has 2 IPs, WebRTC needs STUN |
| What is a Gateway?                    | Default route from private network   |
| How DNS resolution works              | Custom domains & SSL                 |
| Ping, traceroute, netstat, curl       | CLI tools to test connectivity       |

> 🔧  **Skills you gain** :
>
> * Can deploy EC2 securely
> * Can debug frontend ↔ backend issues
> * Can configure sockets and CORS

#### 🟨 **Phase 2 – Backend/Cloud-Level Networking**

📅 Time: 5–7 days

🎯 Focus: Required for **AWS, production-grade MERN, CI/CD**

| Topic                           | Why It’s Important                              |
| ------------------------------- | ------------------------------------------------ |
| OSI & TCP/IP models             | Understand how data flows between systems        |
| TCP 3-way handshake             | Learn what "connection established" really means |
| CIDR, Subnetting, VPC           | For setting up VPCs & EC2 firewalls              |
| Private Subnet vs Public Subnet | Where to place MongoDB, backend                  |
| NAT Gateway & Bastion Host      | Access private EC2s securely                     |
| Load Balancer (ELB)             | Handle multiple backend instances                |
| DNS (Route 53)                  | Host your domain and point it to EC2             |
| SSL/TLS (HTTPS)                 | Encrypt frontend ↔ backend traffic              |
| STUN & TURN Servers             | Used in WebRTC for peer-to-peer                  |
| IP Tables & UFW                 | Local firewall configuration on Linux            |
| Network Latency, Bandwidth      | Debug speed issues in your app                   |

> 🔧  **Skills you gain** :
>
> * Can set up VPCs, subnets, and secure backend architecture
> * Can explain what goes wrong in networking interviews
> * Can build truly cloud-native apps

#### 🟥 **Phase 3 – DevOps / System Design / SRE Level**

📅 Time: 7–10 days (Optional unless you're targeting cloud/devops/senior roles)

| Topic                               | Why It’s Important                     |
| ----------------------------------- | --------------------------------------- |
| ARP, ICMP, DNS lookup internals     | In-depth troubleshooting                |
| Routing tables & path MTU discovery | Debug advanced networking               |
| VPNs & tunneling                    | Securely access internal infra          |
| Proxy vs Reverse Proxy (NGINX)      | Understand API Gateway, NGINX setup     |
| Content Delivery Networks (CDN)     | Improve frontend load time (CloudFront) |
| WebSockets over TLS (wss://)        | Secure your socket connections          |
| Load balancing strategies           | Round-robin, sticky sessions            |
| DNS Failover & Health Checks        | High availability strategies            |
| Packet capture (tcpdump, Wireshark) | Low-level packet debugging              |
| Multi-region deployments            | High-scale infra readiness              |

> 🔧  **Skills you gain** :
>
> * Can design and defend backend architecture
> * Pass high-level interviews (SDE-2, DevOps, Infra)
> * Prepare for AWS certifications

## 🧭 Summary Table

| Phase      | Goal                       | Topics                                | Time                  |
| ---------- | -------------------------- | ------------------------------------- | --------------------- |
| 🟩 Phase 1 | For MERN + Deployment      | IPs, Ports, DNS, TCP/UDP, HTTP, NAT   | 3–4 days             |
| 🟨 Phase 2 | For AWS + Production apps  | Subnetting, CIDR, SSL, Load Balancers | 5–7 days             |
| 🟥 Phase 3 | For DevOps + System Design | VPNs, Proxies, CDNs, Packet capture   | 7–10 days (optional) |

#### 🛠 Tools & Commands You Should Know

| Tool                         | Purpose                 |
| ---------------------------- | ----------------------- |
| `ping`                     | Test connectivity       |
| `traceroute`or `tracert` | Trace packet path       |
| `curl`/`wget`            | Test API/HTTP endpoints |
| `netstat`/`ss`           | List ports in use       |
| `dig`/`nslookup`         | DNS troubleshooting     |
| `iptables`/`ufw`         | Linux firewall control  |
| `tcpdump`/`Wireshark`    | Packet-level debugging  |

#### 📘 Resources

| Resource                                                                                           | Type                  |
| -------------------------------------------------------------------------------------------------- | --------------------- |
| [Computer Networking by Charles Severance (free)](https://www.coursera.org/learn/computer-networking) | Course                |
| [Net Ninja – Networking Crash Course](https://www.youtube.com/c/TheNetNinja)                         | Video                 |
| [AWS Networking Fundamentals](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html) | AWS Docs              |
| [Roadmap.sh Networking](https://roadmap.sh/networking)                                                | Guide                 |
| `OverTheWire: Bandit`                                                                            | Gamified CLI practice |

---

---

## ----Nginx Topics to Learn

For **Phase 1: Manual Deployment** of your MERN app to AWS, here's **how much Nginx** and  **what concepts you need to learn** :

#### ✅ **Essential Nginx Concepts You Need to Know**

##### 🔹 1. **What is Nginx?**

* High-performance web server and reverse proxy
* Used to route requests to your backend (Node.js) and serve static files

##### 🔹 2. **Basic Nginx Terminology**

* `server`: Defines a virtual server block (e.g., domain + port handling)
* `location`: Route matching rules (e.g., `/api`, `/`, `/static/`)
* `proxy_pass`: Forwards incoming requests to backend servers (Node.js app)
* `listen`: Port the Nginx server listens to (usually `80` for HTTP or `443` for HTTPS)

##### 🔹 3. **Nginx as a Reverse Proxy**

* Routes client requests to your Node.js app (running on port `3000` or any other)
* Improves security and performance
* Hides backend details from users

**Example:**

```nginx
location / {
    proxy_pass http://localhost:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}
```

##### 🔹 4. **Serving React Frontend**

* Serve the `build` folder from your React app as static files

**Example:**

```nginx
location / {
    root /var/www/my-react-app;
    index index.html index.htm;
    try_files $uri /index.html;
}
```

##### 🔹 5. **Handling Multiple Locations**

* Route `/api` to backend, `/` to frontend

**Example:**

```nginx
server {
    listen 80;

    location /api {
        proxy_pass http://localhost:5000;
    }

    location / {
        root /var/www/frontend;
        index index.html;
        try_files $uri /index.html;
    }
}
```

##### 🔹 6. **SSL with Certbot**

* Use Nginx with Let's Encrypt for HTTPS
* `certbot --nginx` handles automatic certificate creation and Nginx config

##### 🔹 7. **Basic Nginx Commands**

```bash
sudo nginx -t                # Test configuration
sudo systemctl restart nginx # Restart Nginx
sudo systemctl status nginx  # Check status
sudo nginx -s reload         # Reload config without downtime
```

##### 🔹 8. **Configuration Files**

* Location: `/etc/nginx/nginx.conf` or `/etc/nginx/sites-available/default`
* You can also create custom config files inside `/etc/nginx/sites-available/` and symlink to `/etc/nginx/sites-enabled/`

#### 🧠 Optional (Advanced Later)

* Load balancing with Nginx
* Rate limiting
* Caching
* Custom error pages
* Gzip compression

#### ✅ Summary: You need to learn Nginx for:

| Use Case               | Feature Used           |
| ---------------------- | ---------------------- |
| Proxy Node.js app      | `proxy_pass`         |
| Serve React build      | `root`,`try_files` |
| Route `/api`vs `/` | `location`blocks     |
| Add HTTPS              | Certbot + Nginx        |
| Manage server          | Basic Nginx commands   |

---

## ----AWS EC2 + Nginx + S3 + Cloudfront

Once you've learned  **EC2, SSH, and PM2** , you're ready to deploy your MERN app manually. Here's the **ideal order** to learn and use **Nginx, S3, and CloudFront** for **Phase 1** deployment:

#### ✅ **Recommended Learning Order for Phase 1:**

##### **1️⃣ Nginx (Next Step)**

**Why:**

* Nginx is essential to expose your app to the internet from your EC2 server.
* It acts as a **reverse proxy** for your Node backend and/or serves your React frontend.
* Required even without S3 or CloudFront.

**Learn and Do:**

* Reverse proxy for your Node backend (e.g., port 3000)
* Serve React frontend directly from EC2 if not using S3
* Basic `location` blocks, `proxy_pass`, `try_files`, and HTTPS (via Certbot)

##### **2️⃣ S3 (After Nginx Works)**

**Why:**

* S3 is used to **host static assets** like your **React frontend** (from `npm run build`).
* This offloads static file serving from EC2 (cheaper + faster).
* You don't need Nginx to serve React if you're using S3.

**Learn and Do:**

* Upload `build/` folder to S3
* Make S3 bucket public (or use CloudFront for better security)
* Enable static website hosting on S3

##### **3️⃣ CloudFront (After S3 is Setup)**

**Why:**

* CloudFront improves performance and **adds SSL/HTTPS** on top of S3.
* Caches React frontend globally for faster loading
* Hides your S3 bucket URL (more secure)

**Learn and Do:**

* Create CloudFront distribution for your S3 bucket
* Set custom domain (optional) with HTTPS via ACM (Amazon Certificate Manager)
* Point your domain's DNS to the CloudFront distribution

##### 🔁 Final Phase 1 Architecture (Basic):

**Option A (Simple):**

```
Client → Nginx (on EC2) → Node.js (via PM2)
           |
           → React (served from same EC2 server)
```

**Option B (Recommended, scalable):**

```
Client → CloudFront → S3 (React frontend)
         |
         → API requests → Nginx (EC2) → Node.js (via PM2)
```

#### ✅ Summary of Order:

| Order | Component  | Purpose                              |
| ----- | ---------- | ------------------------------------ |
| 1️⃣ | Nginx      | Reverse proxy for backend/API        |
| 2️⃣ | S3         | Host static frontend (React build)   |
| 3️⃣ | CloudFront | Global CDN + HTTPS + better security |

---

## ----S3 Topics to Learn

To use **Amazon S3 effectively** — especially with a **MERN stack app** or general web development — you don’t need to learn everything. But you should get comfortable with certain key  **core concepts and tasks** .

#### 🔑 **Essential S3 Topics for MERN Developers**

Here’s a practical breakdown:

##### ✅ 1. **Buckets**

* What they are and how to create them.
* Bucket naming rules (globally unique, no uppercase).
* Bucket **regions** (important for latency and cost).

##### ✅ 2. **Object Storage**

* Objects = file + metadata.
* Object  **key naming** : `images/user1/avatar.png`
* Simulated folder structure using  **prefixes** .

##### ✅ 3. **Permissions & Access Control**

* **Public vs private** files.
* Bucket policies (JSON rules).
* IAM roles and policies (for apps/users to access S3).
* CORS configuration (for browser uploads).

##### ✅ 4. **Static Website Hosting**

* Host static sites (e.g., React frontend) from a bucket.
* Enable static site hosting and set index/error documents.
* Public-read access configuration.

##### ✅ 5. **Presigned URLs**

* Temporarily allow upload/download.
* Useful for secure image uploads from browser/client.
* Generated from your backend (Node.js using AWS SDK).

##### ✅ 6. **Versioning (Optional)**

* Keep multiple versions of the same object.
* Helps in rollback / undelete / backups.
* Costs more, so use only if needed.

##### ✅ 7. **Lifecycle Rules**

* Automatically delete old versions / archive to Glacier.
* Good for managing storage cost over time.

##### ✅ 8. **S3 + CloudFront (Optional)**

* If you need faster global delivery (CDN).
* Useful for product images, frontend apps.

##### ✅ 9. **Uploading Files to S3**

* Via AWS SDK (`@aws-sdk/client-s3`) in your  **Node.js backend** .
* Or directly from the frontend using  **presigned URLs** .

##### ✅ 10. **Security Best Practices**

* Don’t make buckets public unless necessary.
* Use IAM roles for EC2 or Lambda if needed.
* Never hardcode credentials — use `.env` or environment vars.

#### 💡 Optional: For advanced use cases

* **S3 Event Notifications** (e.g., trigger Lambda when file uploaded)
* **Multipart uploads** for large files
* **Cross-region replication**
* **Server access logging**

#### 🔧 TL;DR - What You Should Focus On First

| Use Case                                        | Must Learn                                      |
| ----------------------------------------------- | ----------------------------------------------- |
| Image upload/download (e.g., products, avatars) | Buckets, IAM, Upload APIs, Presigned URLs, CORS |
| Hosting React frontend                          | Static hosting, Public access                   |
| Secure access                                   | IAM, Bucket Policy, Presigned URLs              |
| Storage cleanup                                 | Lifecycle rules (optional)                      |

---

## ----Professional Deployment Options

When deploying a  **MERN stack application** , a professional deployment separates concerns between frontend and backend. Here’s the breakdown of **both options** and what’s recommended in a production-grade environment:

#### ✅ **Recommended Professional Approach**

##### 🔷 **Option 1: Serve React frontend via S3 + CloudFront + Route 53, Backend on EC2 with NGINX**

This is the  **industry-standard and scalable setup** .

* [ ] **Frontend (React)**

1. `npm run build` your React app.
2. Upload build folder to  **S3 bucket** .
3. Enable **Static Website Hosting** on the bucket.
4. Attach **CloudFront** distribution to that bucket for:
   * HTTPS
   * Caching
   * Global delivery

* [ ] **Backend (Node.js)**

1. Launch EC2 (Amazon Linux / Ubuntu).
2. Install Node.js, NGINX.
3. Run your Express app (PM2 recommended).
4. Use NGINX to:
   * Proxy `/api` to your backend.
   * Serve SSL via Let's Encrypt (Certbot).

* [ ] **Domain & HTTPS**

* Use **Route 53** (or any DNS provider) to map your domain.
* CloudFront and NGINX can both provide SSL/TLS (HTTPS).

✅ **Pros**:

* **Faster & cheaper delivery** : Static assets (React build) are served via  **S3 + CloudFront (CDN)** .
* **Separation of concerns** : Backend and frontend are clearly separated.
* **Better scaling** : Backend can scale independently with EC2 or containers.
* **HTTPS + caching** : CloudFront handles HTTPS (SSL) and aggressive caching.

**Architecture:**

```
Browser
   ↓
CloudFront (HTTPS CDN)
   ↓
S3 (Static React Build)
   ↓                            ↑
   →→→ API Calls →→→→→→→→→→→→→→ EC2 + NGINX + Node.js (Express)
                             ↓
                          MongoDB Atlas (or EC2 MongoDB)
```

##### 🟡 **Option 2: Serve both Frontend and Backend via EC2 + NGINX**

* Upload both the `React build` and `Node.js backend` to the  **same EC2 instance** .
* Configure **NGINX** to:
  * Serve static files (`/` routes to `/var/www/my-react-app/build`)
  * Proxy API requests (e.g., `/api/`) to Node.js backend

**Pros:**

* Simpler to deploy.
* Easier to debug (all in one machine).

**Cons:**

* Frontend & backend tightly coupled.
* No CDN, so  **slower global delivery** .
* Less scalable.

| Layer      | Tool             | Why                         |
| ---------- | ---------------- | --------------------------- |
| Frontend   | React in S3 + CF | Fast, cheap, scalable       |
| Backend    | Node.js in EC2   | Dynamic API server          |
| Web Server | NGINX            | Reverse proxy, TLS, routing |
| Database   | MongoDB Atlas    | Managed, scalable DB        |

Let me know if you want the  **exact NGINX config** , deployment steps, or CI/CD guide.
