---
title: "🚀Day 10: Mastering SSH, Key-Based Authentication, and Remote Access"
seoTitle: ""Day 10 DevOps: Master SSH, Key-Based Authentication & Remote Access""
seoDescription: "Explore SSH basics, key-based authentication, and secure remote access in Day 10 of my DevOps journey. Learn to generate SSH keys, connect to servers."
datePublished: Tue Jul 15 2025 14:41:49 GMT+0000 (Coordinated Universal Time)
cuid: cmd4n4zoj001e02jy7qlb9hhl
slug: day-10-mastering-ssh-key-based-authentication-and-remote-access
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/szrJ3wjzOMg/upload/b130eb06ae5ee647157de6c8b8a2a147.jpeg
tags: bash, devops, ssh, linux-for-beginners, ssh-keys, ssh-key-pair-for-ec2

---

Welcome to Day 10 of my DevOps journey! Today, I dove into **SSH (Secure Shell)**, a critical tool for securely accessing and managing remote systems. From generating SSH keys to locking down servers, I explored how to connect to remote machines like a pro. Here's a detailed breakdown of what I learned, complete with hands-on tasks, commands, and tips for practicing SSH locally—perfect for aspiring DevOps engineers! 🛠️

---

### **SSH = Secure Shell**

SSH lets you securely connect to a remote remote machine / server ( like an AWS EC2 instance ) from your own terminal

### SSH BASICS

🔑 **SSH Key**,  
💻 **SSH Client**,  
🖥️ **SSH Server**, and  
🚪 **SSH Port**

---

### 📊 SSH Components Table

| Component | Meaning | Example / Default |
| --- | --- | --- |
| **SSH Key** | A pair of cryptographic keys (public/private) used for secure, password-less login. | Public key (`id_`[`rsa.pub`](http://rsa.pub)) stored on the server, private key (`id_rsa`) stays with the client |
| **SSH Client** | The tool or program that initiates a connection to a remote server over SSH. | `ssh user@hostname`, PuTTY, Git Bash |
| **SSH Server** | The machine or service that listens for and accepts SSH connections. | A Linux server running the `sshd` service |
| **SSH Port** | The network port used by SSH to listen for incoming connections. | Default is `22` (can be changed for security) |

---

### ✅ Brief Explanation of Each:

* **SSH Key**:
    
    * Replaces passwords with a more secure authentication system.
        
    * You generate it using `ssh-keygen` and add the public key to the server.
        
* **SSH Client**:
    
    * It's the command or tool you run on your local machine to start the SSH session.
        
    * Example: `ssh ravi@192.168.1.10`
        
* **SSH Server**:
    
    * A remote machine (like a cloud server or another PC) that allows SSH connections.
        
    * The SSH daemon (`sshd`) must be running.
        
* **SSH Port**:
    
    * By default, SSH listens on port `22`.
        
    * You can change it (e.g., to `2222`) by editing `/etc/ssh/sshd_config`.
        

---

## 🧠 What I Learned Today

* **SSH Basics**: Understood how SSH enables secure communication over insecure networks.
    
* **Key-Based Authentication**: Learned to connect to servers without passwords using public/private key pairs.
    
* **SSH Key Generation**: Practiced creating keys with `ssh-keygen`.
    
* **Remote Server Connection**: Mastered the `ssh username@ip` command.
    
* **Security Best Practices**: Secured SSH access by disabling root login and password authentication.
    
* **Troubleshooting**: Tackled common SSH errors like "Permission denied" and "Connection refused."
    
* **Documentation**: Logged commands and outputs for my DevOps journal.
    

---

## 📖 What is SSH?

**SSH (Secure Shell)** is a cryptographic protocol that allows secure access to remote systems. It’s a DevOps essential for:

* Logging into cloud servers (e.g., AWS EC2, GCP).
    
* Securely transferring files using `scp` or `sftp`.
    
* Automating deployments via terminal access.
    

SSH encrypts communication between a client (your machine) and a server, ensuring data stays safe even on insecure networks.

---

## ✅ Step-by-Step Learning Plan

### Step 1: Generate SSH Key Pair 🔐

I started by generating an SSH key pair on my local machine. Here’s the command I used:

```bash
ssh-keygen -t rsa -b 4096 -C "ritesh@devops.com"
```

* **Options Explained**:
    
    * `-t rsa`: Uses the RSA algorithm.
        
    * `-b 4096`: Sets a strong 4096-bit key.
        
    * `-C "`[`ritesh@devops.com`](mailto:ritesh@devops.com)`"`: Adds a comment to identify the key.
        
* **Output**:
    
    * Private key: `~/.ssh/id_rsa` (⚠️ Never share this!)
        
    * Public key: `~/.ssh/id_`[`rsa.pub`](http://rsa.pub) (Share with servers).
        
* I pressed Enter to accept defaults and skipped the passphrase for simplicity.
    

### Step 2: Connect to a Remote Server 🔗

To connect to a remote server (e.g., an AWS EC2 instance), I used:

```bash
ssh ubuntu@<your-server-ip>
```

If the public key is added to the server, this logs in without a password. I didn’t have an EC2 instance, so I practiced locally (more on that below).

### Step 3: Copy SSH Key to Remote Machine 📤

If password login is required, I used `ssh-copy-id` to add my public key to the server:

```bash
ssh-copy-id username@remote_ip
```

This copies `id_`[`rsa.pub`](http://rsa.pub) to the server’s `~/.ssh/authorized_keys` file.

### Step 4: Secure SSH Access 🔒

To make SSH more secure, I edited the server’s SSH configuration file (`/etc/ssh/sshd_config`):

```bash
sudo nano /etc/ssh/sshd_config
```

* **Disabled root login**:
    
    ```text
    PermitRootLogin no
    ```
    
* **Disabled password login** (optional for high security):
    
    ```text
    PasswordAuthentication no
    ```
    
* Restarted SSH:
    
    ```bash
    sudo systemctl restart ssh
    ```
    

### Step 5: Practice Locally 🧪

Since I didn’t want to spin up a paid cloud server, I practiced SSH between two local Ubuntu users (`ritesh` and `ravi`) on my machine. Here’s how I did it:

#### 1\. Create a Second User

```bash
sudo adduser ravi
```

* Set a password (e.g., `12345678`) and skipped other details.
    

#### 2\. Enable SSH Server

```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
```

* Verified SSH was running:
    
    ```bash
    sudo systemctl status ssh
    ```
    

#### 3\. Generate SSH Key (as ritesh)

```bash
ssh-keygen -t rsa -b 4096 -C "ritesh@localhost"
```

* Accepted defaults and skipped the passphrase.
    

#### 4\. Copy Public Key to ravi

```bash
sudo mkdir -p /home/ravi/.ssh
sudo cp ~/.ssh/id_rsa.pub /home/ravi/.ssh/authorized_keys
sudo chown -R ravi:ravi /home/ravi/.ssh
sudo chmod 700 /home/ravi/.ssh
sudo chmod 600 /home/ravi/.ssh/authorized_keys
```

#### 5\. Test SSH Connection

```bash
ssh ravi@localhost
```

Success! I logged in without a password. 🎉

#### 6\. File Transfer with `scp`

I copied a test file to `ravi`’s home directory:

```bash
scp test.txt ravi@localhost:/home/ravi/
```

### Step 6: Document Everything 📘

I logged all commands, outputs, and notes in [`Day10.md`](http://Day10.md) (this file!) and created an `ssh_`[`cheatsheet.md`](http://cheatsheet.md) for quick reference.

---

## 🧰 Hands-On SSH Tasks I Tried

Here’s a quick rundown of the tasks I completed:

| Task | Command |
| --- | --- |
| Check SSH version | `ssh -V` |
| Generate SSH keys | `ssh-keygen -t rsa -b 4096 -C "`[`ritesh@devops.com`](mailto:ritesh@devops.com)`"` |
| View keys | `ls ~/.ssh/` |
| Copy public key | `cat ~/.ssh/id_`[`rsa.pub`](http://rsa.pub) |
| Add public key to server | Copy to `~/.ssh/authorized_keys` |
| Connect via SSH | `ssh user@ip` |
| Use private key | `ssh -i ritesh-key.pem user@ip` |
| Disable password login | Edit `/etc/ssh/sshd_config`: `PasswordAuthentication no` |
| Restart SSH | `sudo systemctl restart ssh` |

---

## Common Errors & Fixes

I intentionally broke SSH to practice troubleshooting. Here’s what I encountered:

1. **Error**: `Permission denied (publickey)`
    
    * **Fix**: Checked permissions for `~/.ssh/authorized_keys` (`chmod 600`) and ensured the public key was correctly copied.
        
2. **Error**: `Connection refused`
    
    * **Fix**: Verified SSH service was running (`sudo systemctl start ssh`).
        
3. **Error**: Password prompt appeared
    
    * **Fix**: Re-copied the public key using `ssh-copy-id` and checked file ownership (`chown ravi:ravi`).
        

---

## 🧠 Bonus Practice Ideas

* **Break SSH**: I removed `~/.ssh/authorized_keys` and tried reconnecting. This triggered a “Permission denied” error, which I fixed by restoring the key.
    
* **File Transfer**: Successfully used `scp` to copy a file to `ravi`’ s home directory.
    
* **Future Plan**: I’ll explore SSH into Docker containers on Day 11+.
    

## Key Commands Check SSH version: `ssh -V`

* Generate SSH key: `ssh-keygen -t rsa -b 4096 -C "`[`your_email@domain.com`](mailto:your_email@domain.com)`"`
    
* View SSH keys: `ls ~/.ssh/`
    
* Copy public key: `cat ~/.ssh/id_`[`rsa.pub`](http://rsa.pub)
    
* Copy key to server: `ssh-copy-id username@remote_ip`
    
* Connect to server: `ssh username@remote_ip`
    
* Connect with private key: `ssh -i key.pem username@remote_ip`
    
* Secure file transfer: `scp file.txt username@remote_ip:/path/`
    
* Edit SSH config: `sudo nano /etc/ssh/sshd_config`
    
* Disable root login: `PermitRootLogin no`
    
* Disable password login: `PasswordAuthentication no`
    
* Restart SSH: `sudo systemctl restart ssh`
    
* ## File Permissions
    
* SSH directory: `chmod 700 ~/.ssh`
    
* Private key: `chmod 600 ~/.ssh/id_rsa`
    
* Public key: `chmod 644 ~/.ssh/id_`[`rsa.pub`](http://rsa.pub)
    
* Authorized keys: `chmod 600 ~/.ssh/authorized_keys`
    

## Troubleshooting

* **Permission denied**: Check key permissions or re-copy public key.
    
* **Connection refused**: Ensure SSH server is running (`sudo systemctl start ssh`).
    
* **Password prompt**: Verify public key in `~/.ssh/authorized_keys`.
    

---

## 🔗 Let’s Connect

📌 **LinkedIn**: [Ritesh Singh](https://linkedin.com/in/ritesh-singh-092b84340)  
💻 **GitHub**: [ritesh355](https://github.com/ritesh355)

I'm sharing my entire #100DaysOfDevOps journey — one command, one project, one day at a time 🚀  
Feel free to connect, follow, or ask questions!