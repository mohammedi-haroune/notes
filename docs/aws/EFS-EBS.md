# EFS
Scalable, elastic cloud-native NFS file system. Attach a single file system to multiple EC2 instances. Don't worry about running out or managing disk space

- supports NFSv4 protocol
- You pay arround (starting from ?) 0.3$ GB / month
- Volumes can scale to petabyte size storage
- Volumes will shrink and grow to meet current data stored (elastic)
- Can support thousands of concurrent connections over NFS
- You data is stored across multiple AZs within a region
- Can mount multiple EC2 instance to a single EFS (as long as they are all in the same VPC)
- Creates Mount Points in all your VPC subnets so you can mount from anywhere whithin your VPC
- Provides Read After Write Consistency
- The mount point should have a security group that allows NFS access from the instances's security group


# EBS
Virtual hard drive in the could. Create new volumes attach to EC2 instances, backup via snapshots and easy encryption. Volumes are automatically replicated whithin their AZ to protect from component failure

**IOPS** stands for Inut/Output per second. It is the speed at which non-contiguous reads and writes can be performed  on a storage medium. high I/O = lots of small fast reads and writes.

**Throughput** is the data transfer rate to and from the storage meduim megabytes per second.

**Bandwidth** is the measurement of the total possible speed of data movement along the network

Think of **Bandwidth** as the **Pipe** and **Throughput** as the water

## EBS Types
### SSD
- General Purpouse SSD (gp2)
- Provisionned IOPS SSD (io1)

![[aws-ebs-types.png]]

## HDD
- Throughput Optimized HDD (st1)
- Cold HDD (sc1)
![[aws-ebs-types-hdd.png]]

## Magnetic
- EBS Magnetic (standard)
![[aws-ebs-types-magnetic.png]]

## Moving volumes
**From one AZ to another**
1. Take snapshot of the volume
2. Create an AMI from the snapshot
3. launch new EC2 instance in desired AZ

**From one Region to another** In addition to the steps above we have to copy the AMI to the other region

**Encrypt existing root volume** just select the encryption option when creating the snapshot

## EBS vs Instance Store Volumes
An EC2 instance can be backed (root device) by an **EBS Volume** or **Instance Store Volume**

**Instance Store Volume:** Temporary (Ephemeral), located on disks that are physically attached to a host machine, created from a template stored in S3, Cannot stop instances, only reboot or temrminate, Data is lost in case of health fails or termination

**EBS Volume:** Durable, created from EBS Snapshot, Can start/stop instances, Data persist if you reboot your system.


- Volumes exists on EBS, Snapshots exist on S3.
- Snapshots are incremental, only changes made since the last snapshot are moved to S3.
- If talking Snapshot of a root volume, the EC2 instnace should be stopped before Snapshotting.
- You can take Snapshots while the instance is running (if it's not the root volume ?)
- You can create AMIs from Snapshots or Volumes
- Volumes always exist in the same AZ as the EC2 instance
- By default root volumes are deleted on termination which can be disalbed (termination protection)
- Snapshots or restored encrypted volumes will also be encrypted
- You cannot share a snapshot if it has been encrypted. Unencrpted ones can be shared with other AWS accounts or made public