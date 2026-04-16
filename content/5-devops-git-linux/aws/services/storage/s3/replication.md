

## 🧾 **What is S3 Replication?**

> **Amazon S3 Replication** is a feature that **automatically copies objects from one S3 bucket (source) to another (destination)**, either in the same region or a different region.

- Works **asynchronously** (not real-time)
- Helps with **backup, disaster recovery, cross-region availability**, and **compliance**
- Uses **IAM roles**, **replication rules**, and **bucket versioning**

---

## 🧠 Why Replicate?

|Goal|Description|
|---|---|
|🔁 High availability|Serve content faster globally using a nearby region|
|🛡️ Disaster recovery|If one region fails, the other still has the data|
|📄 Compliance|Some regulations require multi-region backups|
|🎯 Analytics separation|Run analytics in another region to offload compute|
|🚫 Prevent accidental deletes|Use destination bucket as immutable archive|

---

## 🔄 Types of S3 Replication

| Replication Type                   | Description                                                           |
| ---------------------------------- | --------------------------------------------------------------------- |
| **Cross-Region Replication (CRR)** | Source and destination buckets are in **different regions**           |
| **Same-Region Replication (SRR)**  | Buckets are in the **same region**, used for logging, analytics, etc. |

---

## ⚙️ How It Works

1. **Enable versioning** on both source and destination buckets
2. Set up a **replication rule**
3. IAM role is created to allow S3 to replicate on your behalf
4. New or updated objects are copied to the destination bucket

> 🔸 Existing objects are **not replicated automatically**  
> 🔸 You can use the **S3 Batch Replication** to replicate existing data

---