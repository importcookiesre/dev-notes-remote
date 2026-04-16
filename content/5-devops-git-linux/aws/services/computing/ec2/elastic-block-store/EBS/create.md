


## 🔹 **Example: Creating and Attaching an EBS Volume**

1. Create an EBS volume (e.g., 20 GiB, gp3) in same AZ as your EC2 instance.
2. Attach it to your EC2 instance.
3. SSH into EC2 and format it:

```
lsblk # list all storages
```

```
ubuntu@ip-172-31-1-112:~$ lsblk
NAME         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0          7:0    0 27.2M  1 loop /snap/amazon-ssm-agent/11320
loop1          7:1    0 73.9M  1 loop /snap/core22/1981
loop2          7:2    0 50.9M  1 loop /snap/snapd/24505
nvme0n1      259:0    0    8G  0 disk 
├─nvme0n1p1  259:1    0    7G  0 part /
├─nvme0n1p14 259:2    0    4M  0 part 
├─nvme0n1p15 259:3    0  106M  0 part /boot/efi
└─nvme0n1p16 259:4    0  913M  0 part /boot
nvme1n1      259:5    0    8G  0 disk 
```

we attached
```
nvme1n1      259:5    0    8G  0 disk 
```

This is the **new EBS volume** (8 GB) you've just attached. It is currently:
- **Not partitioned**
- **Not formatted**
- **Not mounted**

```bash
sudo mkfs -t ext4 /dev/nvme1n1 # format
sudo mkdir /data # create a mount
sudo mount /dev/nvme1n1 /data # mount the volume
df -h # confirm
```

## next step : 

- make the mount automaticall persist.
- set permissions





# remove :



## 🔧 **A. Unmount the Volume on EC2**

If the volume is mounted (e.g., on `/data`), first unmount it:
```
sudo umount /data
```
To verify it is unmounted:
```
lsblk
```
Make sure `/dev/nvme1n1` has **no mountpoint** listed.


## B. Detach the volume

## C. Delete the volume