## Step 1: Boot into Ubuntu _Live_ mode

1. Boot from the Ubuntu USB
    
2. When prompted, select **“Try Ubuntu”**
    
3. Wait until the desktop loads
    

⚠️ **Do NOT install Ubuntu**

---

## Step 2: Open a Terminal

- Press **Ctrl + Alt + T**
    
- Or click **Activities → Terminal**
    

Everything below happens here.

---

## Step 3: Identify the correct disk (VERY IMPORTANT)

Run:

`lsblk`

You’ll see something like:

```
NAME   SIZE TYPE 
sda    931G disk 
├─sda1  32G part 
├─sda2 900G part
```

or maybe:

```
nvme0n1 476G disk 
├─nvme0n1p1 512M part
```

### To help identify and select the drive:

- **Ignore the USB** (usually `sdb`, `sdc`, or ~8–64GB)
    
- The **main drive** is usually:
    
    - `sda` (most common)
        
    - OR `nvme0n1` (newer systems)
        

👉 **Double-check the SIZE** matches the internal drive.

I’ll assume the disk is `/dev/sda`  
(If it’s `nvme0n1`, I’ll note the difference.)

---

## Step 4: Wipe the partition table (this erases EVERYTHING)

Run:

`sudo wipefs -a /dev/sda`

If it asks for a password:

- Just press **Enter** (live Ubuntu has no password)
    

This removes old filesystem signatures from Home Assistant OS.

---

## Step 5: Create a new partition table (GPT – best for Windows)

Run:

`sudo parted /dev/sda --script mklabel gpt`

This gives the disk a fresh, modern layout.

---

## Step 6: Create ONE full-size partition

Run:

`sudo parted /dev/sda --script mkpart primary ntfs 0% 100%`

This creates a single partition covering the entire disk.

---

## Step 7: Format the partition (NTFS)

Now format it:

`sudo mkfs.ntfs -f /dev/sda1`

⚠️ If the disk was `nvme0n1`, the partition name would be:

`sudo mkfs.ntfs -f /dev/nvme0n1p1`

---

## Step 8: Verify everything looks clean

Run:

`lsblk`

You should now see:

- **One disk**
    
- **One partition**
    
- Type shows `ntfs`
    

Example:

`sda    931G disk └─sda1 931G part`

✅ That’s exactly what we want.

---

## Step 9: Shut down and install Windows
