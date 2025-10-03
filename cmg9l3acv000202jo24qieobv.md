---
title: "Mounting an EBS Volume on EC2"
seoTitle: "Mount EBS Volume on EC2 Instance"
seoDescription: "Learn to attach and mount an EBS volume to an EC2 instance in AWS for persistent storage"
datePublished: Thu Oct 02 2025 15:42:28 GMT+0000 (Coordinated Universal Time)
cuid: cmg9l3acv000202jo24qieobv
slug: mounting-an-ebs-volume-on-ec2
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/Hcfwew744z4/upload/e2447f757c59e473fc74a3ba5a5d496f.jpeg
tags: ec2, aws, devops, ebs, mount-volume

---

# How to Attach and Mount Extra EBS Volume to Linux EC2 in AWS | Mounting EBS Volume

## 🎯 Objective

Learn how to **attach, mount, and use an EBS volume** with an EC2 instance for persistent storage in AWS.

---

## 🛠️ AWS Services Used

* **EC2 (Elastic Compute Cloud)**: Instance to attach volume
    
* **EBS (Elastic Block Store)**: Persistent block storage
    
* **IAM**: Proper permissions for EC2 access
    

---

## 📋 Steps

### 1\. Create an EBS Volume

1. Go to **AWS Console → EC2 → Elastic Block Store → Volumes → Create Volume**
    
2. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759417635503/4bf40ca5-547c-4cea-a239-ad112c5a5369.png )
    
3. Choose **Volume type** (e.g., General Purpose SSD `gp3`)
    
4. Set **Size** (e.g., 1 GB for testing)
    
5. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759417739275/e6aa388e-ec57-40c1-86b9-f2dcc6ace340.png )
    
6. Select the **same Availability Zone** as your EC2 instance
    
7. Click **Create Volume**
    

  

### 2\. Attach Volume to EC2

**firstly launce an ec2 instance in same availability zone**

1. Select the volume → **Actions → Attach Volume**
    
2. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759418050560/6e51bfb9-57ad-47b1-8af8-3748d1a55d07.png)
    
3. Choose the **EC2 instance**
    
4. Click **Attach**
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759418074291/b9c80494-a812-4b68-981a-bb8043c004f3.png )

  

![Attach Volume](images/attach-ebs.png align="left")

---

### 3\. Connect to EC2 & Mount Volume

1. SSH into your EC2 instance:
    

```bash
ssh -i mykey.pem ec2-user@<EC2-Public-IP>
```

![after ssh switch to root user using sudo su -](https://cdn.hashnode.com/res/hashnode/image/upload/v1759418318015/1e9628c6-10df-43b1-a9c2-f0f6e2a93d83.png )

2 . After ssh switch into root user

```plaintext
sudo su -
```

3 . use command

```plaintext
df -h 
```

4 . Format the Volume (if needed)

Check if filesystem exists:

```plaintext
sudo file -s /dev/nvme1n1
```

If output shows `data`, format it:

```plaintext
sudo mkfs -t ext4 /dev/nvme1n1
```

**Note —&gt; replace with your disk name like /dev/xvdf in my case it is not xvdf it is nvme1n1**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759419214413/fe91692b-af15-414b-aecb-b9121b75a15f.png )

### 4\. Mount the Volume

* Create a mount point:
    

```plaintext
sudo mkdir /mnt/myvolume
```

* Mount it:
    

```plaintext
sudo mount /dev/nvme1n1 /mnt/myvolume
```

* Verify:
    
* ```plaintext
    df -h
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1759419506610/7160c0f9-5438-429f-8f06-4e43d488d0ae.png )
    
    ## 🌐 Connect With Me
    
    * 💻 GitHub: [ritesh355](https://github.com/ritesh355)
        
    * 📝 Blog: [ritesh-devops.hashnode.dev](http://ritesh-devops.hashnode.dev)
        
    * 💼 LinkedIn: [Ritesh Singh](https://linkedin.com/in/ritesh-singh-092b84340)
