

## 🔹 **EBS Snapshots**

- Snapshots are **point-in-time backups** of EBS volumes.
- Stored in Amazon S3 (not visible as objects).
- Can be used to:
    - Create new volumes
    - Migrate to different regions
    - Restore from failures

---


# create snapshot : 

### 📌 Step-by-Step:

1. Go to the EC2 Dashboard.
2. In the left-hand menu, click on **"Volumes"** under **"Elastic Block Store"**.
3. Identify the volume you want to back up (e.g., the one attached as `/dev/nvme1n1`).
4. Select the volume.
5. Click **Actions** → **Create Snapshot**.
6. In the popup:
    - Add a **description** (e.g., `Backup of myData volume`)
    - (Optional) Name your snapshot using **Tags**
7. Click **Create Snapshot**.

You’ll be redirected to the **Snapshots** page where you can monitor its progress.

### What can i do with the snapshot : 

|Use Case|Description|
|---|---|
|**Restore volumes**|Recover lost/corrupted EBS data|
|**Cross-AZ/Region cloning**|Create volumes in different zones or regions|
|**Create AMIs**|Launch EC2s from snapshot-based AMIs|
|**Share with other accounts**|Public or private sharing|
|**Automated backups**|Schedule recurring snapshots with Lambda or AWS Backup|
|**Disaster recovery**|Use snapshots to restore systems in another region|
|**RAID replication**|Rebuild RAID sets from multiple snapshots|