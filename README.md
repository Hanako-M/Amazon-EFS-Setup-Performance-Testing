# Amazon-EFS-Setup-Performance-Testing
This project demonstrates how to create, configure, and mount an Amazon Elastic File System (EFS) to an EC2 instance, and then benchmark its performance using fio.  The project was implemented step by step using AWS Management Console, EC2, EFS, and CloudWatch, following best practices for security and performance testing.
Amazon EFS Setup & Performance Testing

This project demonstrates how to create, configure, and mount an Amazon Elastic File System (EFS) to an EC2 instance, and then benchmark its performance using fio.

The project was implemented step by step using AWS Management Console, EC2, EFS, and CloudWatch, following best practices for security and performance testing.

## 📌 Project Overview

Amazon EFS (Elastic File System) is a scalable, serverless, fully elastic file system that can be mounted on multiple EC2 instances across multiple Availability Zones.

In this project, I:

Created a security group to allow NFS traffic.

Created an EFS file system and mount targets in a Lab VPC.

Mounted the EFS file system to an EC2 instance.

Verified disk mount and usage.

Performed I/O benchmarking with fio.

Used Amazon CloudWatch to monitor throughput and write performance.

## 🛠️ Technologies & Tools Used

AWS EC2 (for running the client instance)

AWS EFS (for scalable shared storage)

AWS CloudWatch (for monitoring throughput & I/O metrics)

Amazon VPC & Security Groups (for secure network configuration)

Amazon Systems Manager Session Manager (for browser-based EC2 access)

NFS v4.1 (for mounting EFS)

fio (Flexible IO) (for benchmarking I/O performance)

## 🧩 Steps Implemented
1️⃣ Security Group Creation

Created a new security group for EFS Mount Targets.

Allowed TCP inbound traffic on port 2049 (NFS).

Associated the group with the EFS mount targets.

📸 Screenshot:
![Security Group](Screenshot-2025-09-20-005639.png)

2️⃣ EFS File System Creation

Created a new EFS file system in the Lab VPC.

Disabled automatic backups and lifecycle transitions.

Tagged the file system with Name = My First EFS File System.

Detached the default security group from mount targets and attached the custom one.

📸 Screenshot:
![Security Group](Screenshot-2025-09-20-010221.png) 

3️⃣ Connecting to EC2 Instance

Connected via Session Manager using InstanceSessionURL.

4️⃣ Mounting the File System
sudo su -l ec2-user
sudo yum install -y amazon-efs-utils
sudo mkdir efs
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-XXXXXXXX.efs.<region>.amazonaws.com:/ efs

Verified mount using:

sudo df -hT

📸 Screenshot:
![Security Group](Screenshot-2025-09-20-011603.png) 

5️⃣ Performance Testing

Ran fio write performance test:

sudo fio --name=fio-efs --filesize=10G --filename=./efs/fio-efs-test.img \
--bs=1M --nrfiles=1 --direct=1 --sync=0 --rw=write --iodepth=200 --ioengine=libaio


6️⃣ CloudWatch Monitoring

Observed PermittedThroughput and DataWriteIOBytes metrics in CloudWatch.

Calculated write throughput based on 1-minute intervals.


## 📊 Key Observations

Permitted Throughput peaked around 3 GB/s (burstable).

Write Throughput calculated by CloudWatch was around 7.6 GB per minute, confirming the scaling ability of EFS.

EFS throughput automatically scales with storage size, providing consistent baseline performance with burst capacity.

## ✅ Learning Outcomes

Understood how to configure EFS for multi-AZ, secure mounting.

Learned how to connect and manage EC2 using Systems Manager.

Gained experience with performance benchmarking using fio.

Explored CloudWatch metrics for real-time performance monitoring.
