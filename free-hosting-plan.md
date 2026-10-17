# The Complete Guide to Free Full-Stack Web Hosting

A comprehensive, beginner-friendly guide comparing the best free platforms for hosting both frontend and backend together in a single service.

---

## Table of Contents
1. [What is All-in-One Full-Stack Hosting?](#what-is-all-in-one-full-stack-hosting)
2. [The Big Three Leaders](#the-big-three-leaders)
   - [Render](#1-render)
   - [Koyeb](#2-koyeb)
   - [Vercel](#3-vercel)
3. [Top Alternative Free Full-Stack Providers](#top-alternative-free-full-stack-providers)
   - [Oracle Cloud Always Free (The Hidden Giant)](#4-oracle-cloud-always-free-the-hidden-giant)
   - [Google Cloud Run](#5-google-cloud-run)
   - [Fly.io](#6-flyio)
   - [Zeabur](#7-zeabur)
   - [Glitch](#8-glitch)
   - [Netlify](#9-netlify)
4. [Master Comparison Matrix](#master-comparison-matrix)
5. [The Sleep Problem and The Free Keep-Alive Secret](#the-sleep-problem-and-the-free-keep-alive-secret)
6. [Decision Framework: Which One Should You Choose?](#decision-framework-which-one-should-you-choose)
7. [Recommended Multi-Project Architecture](#recommended-multi-project-architecture)

---

## What is All-in-One Full-Stack Hosting?

A web application usually consists of two main parts:
1. **The Frontend (The User Interface)**: The visual screens, buttons, and styles that users interact with in their web browser (built with tools like React, Vue, HTML, and CSS).
2. **The Backend (The Engine)**: The server process that handles secure API keys, talks to databases, runs business logic, and communicates with external services like AI models.

### Two Ways to Host:
- **Split Hosting**: You put the frontend on a static host (like Cloudflare Pages) and the backend on a server (like a VPS). This requires configuring two deployments, cross-origin permissions (CORS), and separate web domains.
- **All-in-One Full-Stack Hosting**: You run a single server (like an Express or Python server) that does both jobs. It serves the visual webpage and powers the API endpoints simultaneously under one web address.

For beginners, portfolios, and small-to-medium applications, **All-in-One hosting is much simpler, avoids cross-origin errors, and requires only one domain name**.

---

## The Big Three Leaders

### 1. Render
*Best for: Easy GitHub setup, standard Node.js/Python apps, zero credit card requirement.*

Render is one of the most popular platforms for deploying full-stack web applications directly from GitHub.

- **Free Tier Allowance**: 750 free instance hours per calendar month, 512 MB RAM, 0.1 CPU, and 100 GB monthly bandwidth.
- **Full-Stack Capabilities**: Runs Node.js, Python, Go, Rust, Ruby, and Docker. Perfect for single servers serving both static frontend bundles and backend API routes.
- **Custom Domains**: Free custom domain support with automated Let's Encrypt SSL/TLS certificates.
- **The Sleep Rule**: On the free tier, Render automatically spins down (sleeps) your container after **15 minutes of inactivity**. The next visitor experiences a "cold start" wait of approximately **30 to 50 seconds** while the server wakes up.
- **Pro Tip**: Setting up a free 10-minute ping using a service like `cron-job.org` keeps your service awake 24/7 without cold starts.

---

### 2. Koyeb
*Best for: Fast wake-up times, generous 1-hour idle timeout, modern developer experience.*

Koyeb is a modern developer platform designed to run full-stack apps and Docker containers globally with minimal latency.

- **Free Tier Allowance**: 1 free Web Service per account (512 MB RAM, 0.1 vCPU, 2 GB SSD storage), plus 1 free PostgreSQL database.
- **Full-Stack Capabilities**: Native support for Node.js, Python, Go, Ruby, and custom Dockerfiles.
- **Custom Domains**: Free automated custom domains with SSL.
- **The Sleep Rule**: Koyeb allows **1 full hour of inactivity** before sleeping (4 times longer than Render). If it does sleep, its cold start wake-up time is exceptionally fast (typically **1 to 5 seconds**).
- **Pro Tip**: Pinging your Koyeb health endpoint every 45 to 50 minutes keeps your single free service online 24/7 continuously.

---

### 3. Vercel
*Best for: Next.js, frontend-heavy apps with lightweight serverless functions.*

Vercel is the creator of Next.js and the undisputed standard for modern React frameworks.

- **Free Tier Allowance**: 100 GB bandwidth, unlimited personal projects, generous serverless execution units.
- **Full-Stack Capabilities**: Best suited for Next.js, Nuxt, SvelteKit, and serverless API routes (`/api/*`). Traditional long-running Express background servers are not natively recommended here because Vercel uses stateless serverless functions.
- **Custom Domains**: Unlimited custom domains with instant automatic SSL and global edge network routing.
- **The Sleep Rule**: Serverless functions execute on demand with near-zero cold starts (typically under 300ms). However, functions on the free hobby tier have a **10 to 15-second execution timeout**, meaning long-running AI streaming requests or long background jobs will fail if they take longer than 15 seconds.

---

## Top Alternative Free Full-Stack Providers

### 4. Oracle Cloud Always Free (The Hidden Giant)
*Best for: Experienced developers wanting enterprise-grade free compute that NEVER sleeps.*

Oracle Cloud Infrastructure (OCI) offers what is widely considered the most generous free cloud tier in the industry.

- **Free Tier Allowance**:
  - Up to **4 ARM-based Ampere vCPU cores**.
  - **24 GB of RAM** (yes, twenty-four gigabytes, free forever).
  - **200 GB of SSD block storage**.
  - 10 TB of outbound data transfer per month.
- **Full-Stack Capabilities**: You receive a full Linux Virtual Private Server (Ubuntu/Debian). You can run Docker, Node.js, Python, PostgreSQL, Redis, and dozens of full-stack projects simultaneously.
- **Sleep Rule**: **Zero sleep**. It is a dedicated virtual machine running 24/7/365.
- **Trade-off**: Requires a credit card during signup for identity verification (you are not charged), and requires basic Linux command-line knowledge to set up Nginx, firewall rules, and SSL certificates.

---

### 5. Google Cloud Run
*Best for: Containerized full-stack apps with massive free monthly quotas.*

Google Cloud Run is a managed compute platform that takes any Docker container and exposes it securely over HTTPS.

- **Free Tier Allowance**:
  - **2 million free requests per month**.
  - 360,000 vCPU-seconds and 180,000 GiB-seconds of compute free every month.
  - 1 GB outbound networking free.
- **Full-Stack Capabilities**: Builds and runs any language or framework packaged into a Docker container.
- **The Sleep Rule**: Scales down to zero when idle. However, Google's container start time is fast (usually **1 to 3 seconds**).
- **Trade-off**: Requires a Google Cloud account with billing enabled (credit card verification). Costs remain $0 as long as you stay within the 2 million request boundary.

---

### 6. Fly.io
*Best for: Developers who want containerized apps close to global users.*

Fly.io converts Docker containers into lightweight physical micro-virtual machines running on hardware around the world.

- **Free Tier Allowance**:
  - Run up to 3 shared-CPU-1x virtual machines (256 MB RAM each) across global regions.
  - 3 GB of persistent volume storage.
  - 160 GB outbound data transfer per month.
- **Full-Stack Capabilities**: Exceptional support for full-stack Node.js, Python, Go, and Docker applications.
- **The Sleep Rule**: Automatically suspends idle machines to save compute, with fast sub-second resume on incoming HTTP traffic.
- **Trade-off**: Requires a credit card on signup to prevent platform abuse.

---

### 7. Zeabur
*Best for: Effortless one-click full-stack deployments with instant setup.*

Zeabur is a modern cloud deployment platform that automatically recognizes project languages and builds full-stack apps effortlessly.

- **Free Tier Allowance**: Free starter plan providing $5 of recurring monthly platform credits (enough to run lightweight full-stack hobby apps and personal bots).
- **Full-Stack Capabilities**: Excellent automatic detection of Node.js, Python, Java, Go, Rust, and Docker.
- **Custom Domains**: Built-in free subdomains (`*.zeabur.app`) and custom domain support with auto-SSL.
- **The Sleep Rule**: Does not arbitrarily force sleep if you have credit balance remaining.

---

### 8. Glitch
*Best for: Instant experimentation, education, and beginner-friendly Node.js apps.*

Glitch is an interactive, browser-based coding and hosting environment owned by Fastly.

- **Free Tier Allowance**: Free hosting for public apps with 512 MB RAM, 1 GB disk space, and 120 project hours per month.
- **Full-Stack Capabilities**: Ideal for small Node.js full-stack applications. Code edits deploy live instantly with zero build steps.
- **The Sleep Rule**: Apps go to sleep after **5 minutes of inactivity**.

---

### 9. Netlify
*Best for: Static sites with serverless functions and modern Jamstack architecture.*

Netlify is one of the pioneer platforms for modern web development, widely recognized for speed and developer ergonomics.

- **Free Tier Allowance**: 100 GB bandwidth per month, 300 build minutes per month, and 125,000 serverless function invocations per month.
- **Full-Stack Capabilities**: Best suited for static frontends backed by serverless edge functions. Not intended for long-running stateful Express servers.
- **The Sleep Rule**: Serverless functions run on demand with instant response times.

---

## Master Comparison Matrix

| Provider | Free Compute (RAM / CPU) | Sleep Timeout | Cold Start Wait | Best Full-Stack Stack | Credit Card Required? |
|---|---|---|---|---|---|
| **Render** | 512 MB / 0.1 CPU | 15 minutes | 30 to 50 sec | Node.js, Express, Python | **No** |
| **Koyeb** | 512 MB / 0.1 vCPU | 60 minutes | 1 to 5 sec | Python, Node.js, Docker | **No** |
| **Vercel** | Serverless Edge | Instant | < 300 ms | Next.js, SvelteKit, Nuxt | **No** |
| **Oracle Cloud** | 24 GB / 4 ARM vCPUs | **Never** | **0 sec (24/7)** | Docker, Any Linux stack | Yes (verification only) |
| **Google Cloud Run** | 2M requests/mo | Scale-to-zero | 1 to 3 sec | Docker, Containers | Yes (verification only) |
| **Fly.io** | Up to 3x 256MB VMs | Auto-pause | < 1 sec | Docker, Full-stack apps | Yes (verification only) |
| **Zeabur** | $5 monthly credit | Credit-based | Fast | Full-stack, Bots, Node | **No** |
| **Netlify** | Serverless Edge | Instant | < 300 ms | React + Serverless APIs | **No** |

---

## The Sleep Problem and The Free Keep-Alive Secret

### Why Free Hosts Sleep
Free hosting platforms share massive physical servers among thousands of developers. When an application receives no traffic, the host stops its container process to save memory and electricity. When a new user opens the link, the host must start the container from scratch, creating a **cold start delay**.

### The Solution: The "Keep-Alive" Health Check
Most free cloud hosts only put a service to sleep if there are **zero incoming web requests** during their idle window.

You can keep your free server permanently awake using this legal and reliable technique:

1. **Create a lightweight Health Endpoint**:
   In your backend code, create a simple endpoint like `/api/health` that returns a quick JSON response without running heavy database queries or AI tokens:
   ```javascript
   app.get('/api/health', (req, res) => {
     res.json({ status: 'ok', uptime: process.uptime() });
   });
   ```

2. **Use a Free Uptime Scheduler**:
   Sign up for a free service like **[cron-job.org](https://cron-job.org)** or **[UptimeRobot](https://uptimerobot.com)**.

3. **Configure the Schedule**:
   - For **Render**: Schedule an HTTP `GET` request to your health URL every **10 minutes** (before the 15-minute cutoff).
   - For **Koyeb**: Schedule an HTTP `GET` request every **45 minutes** (before the 60-minute cutoff).

Because the platform receives a harmless 1-millisecond check before reaching its idle limit, **it stays awake 24 hours a day, 7 days a week, delivering instant page loads to real users**.

---

## Decision Framework: Which One Should You Choose?

### Scenario A: You want a fast, zero-credit-card full-stack app today
- **Winner**: **Render**
- Connect your GitHub repo, set build and start commands, configure your environment variables, and add a 10-minute ping on `cron-job.org`.

### Scenario B: You want the shortest cold starts and longer idle times
- **Winner**: **Koyeb**
- 1 full hour of idle time and near-instant wake-up times make it ideal for portfolio APIs and Python backends.

### Scenario C: You are building a Next.js or modern React application
- **Winner**: **Vercel**
- Unmatched speed, global edge caching, and seamless serverless routing.

### Scenario D: You want raw power, huge RAM, and zero sleeping forever
- **Winner**: **Oracle Cloud Always Free (OCI)**
- 24 GB RAM and 4 ARM cores give you a virtual powerhouse capable of hosting entire production suites for free.

---

## Recommended Multi-Project Architecture

If you are a developer with multiple portfolio applications to show off, do not put all your apps onto a single free account where they fight over shared instance hours. 

Instead, distribute them strategically:

```text
├── Project 1 (e.g. PromptVault - Node.js Full-Stack) 
│   └── Hosted on: Render (Kept awake via cron-job.org every 10 min)
│
├── Project 2 (e.g. SharpLink - Python Web Service)
│   └── Hosted on: Koyeb (Kept awake via cron-job.org every 45 min)
│
├── Project 3 (e.g. Personal Portfolio / Static Showcase)
│   └── Hosted on: Cloudflare Pages (Unlimited free edge CDN)
│
└── Project 4 (e.g. Next.js SaaS Prototype)
    └── Hosted on: Vercel (Hobby Tier serverless edge)
```

With this architecture:
- Every single application has dedicated compute resources.
- Every portfolio link opens instantly for recruiters without cold start delays.
- Your entire developer presence runs online **24/7/365 for exactly $0**.

---

<div align="center">

**Authored by [Sagar Biswas (@SagarBiswas-MultiHAT)](https://github.com/SagarBiswas-MultiHAT)**  
*Part of the [Developer Second Brain & Engineering Vault](./README.md)*  
⭐ **Star this repository** to help more developers deploy 24/7 web apps for $0!

[Back to Top](#the-complete-guide-to-free-full-stack-web-hosting) • [Vault Catalog (README)](./README.md) • [Contributing](./.github/CONTRIBUTING.md)

</div>

---