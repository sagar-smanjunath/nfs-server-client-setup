# nfs-server-client-setup
Configured NFS (Network File System) for centralized storage sharing between Linux server and client with real-time synchronization.

📌 Project Overview
      -This project demonstrates the configuration of NFS (Network File System) to share storage between two Linux systems.

💡 Scenario
Sagar (Server/Master) has 50GB storage
Praveen (Client) has only 25GB storage
Client needs access to extra storage without copying data
FTP is not suitable since it requires downloading files

👉 Solution: Use NFS, which allows remote directory access as if it's local.

⚙️ Technologies Used

Linux (RHEL/CentOS)
NFS (nfs-utils)
Services:
nfs
rpcbind
Configuration File:
/etc/exports
Port:
2049

🏗️ Architecture

   NFS Server (Master)
   192.168.174.131
   /var/normal_feb19
          │
          │ (NFS Share)
          ▼
   NFS Client
   /var/nfs_feb25
   
🚀 Step-by-Step Configuration

🔹 1. Install NFS Package (Both Servers)
yum install nfs-utils -y
🔹 2. Disable Security (Lab Purpose Only)
setenforce 0
service iptables stop
🔹 3. Start Services
On Server:
/etc/init.d/rpcbind start
service nfs restart
On Client:
/etc/init.d/rpcbind start
service nfs restart
🔹 4. Configure NFS Share (Server)

Edit exports file:

vim /etc/exports

Add:

/var/normal_feb19 *(rw,sync)

Apply configuration:

exportfs -rv
🔹 5. Verify Disk on Server
df -h

👉 Ensure /dev/sdb1 is mounted on /var/normal_feb19

🔹 6. Mount NFS on Client

Create mount directory:

mkdir /var/nfs_feb25

Mount NFS share:

mount 192.168.174.131:/var/normal_feb19 /var/nfs_feb25

Verify:

df -h

📸 Screenshots Explanation (Add these in your README)

🖼️ Image 1 – Server Export Configuration
Shows /etc/exports configuration
NFS share directory: /var/normal_feb19
Permissions: rw,sync
exportfs -rv used to apply changes

🖼️ Image 2 – Client Mounting NFS

Command used:

mount 192.168.174.131:/var/normal_feb19 /var/nfs_feb25
df -h shows remote storage mounted
Confirms successful connection to server

🖼️ Image 3 – Client Accessing Files
Navigating to /var/nfs_feb25
ls -ltr shows all files from server
Demonstrates shared access

🖼️ Image 4 – Real-Time File Sharing

File testing created on server using:

touch testing
Same file visible on client instantly
Confirms real-time synchronization

✅ Key Features

No need to copy files manually
Saves client storage
Real-time file access
Centralized data management

⚠️ Limitations

Not secure by default (used * for all clients)
Requires network connectivity
Performance depends on network speed

📌 Conclusion

NFS provides an efficient way to share storage across systems. It eliminates the need for file transfers and allows seamless access to remote directories as if they are local.
