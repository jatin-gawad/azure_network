# 🔐 Remove Public IP from VM2 – PostgreSQL Security Hardening

## 📋 Task Summary

To enhance security for the new test environment, the public IP address has been removed from **VM**, as this virtual machine is intended to host the PostgreSQL database. Exposing the database server to the public internet poses a security risk and is not required for this setup.

## 🧾 Background

- **Infrastructure Setup**:
  - Deployed in a **separate Azure subscription and VNet**.
  - Two VMs:
    - `VM1`: App server  ==>ssh port is open only drew have access, adding his ip address in nsg
    - `VM2`: Database server (PostgreSQL)

- **Security Note**: PostgreSQL should only be accessible within the internal network. Public IP was originally assigned during provisioning but is now removed based on best practices.

## ⚙️ Steps Taken (via Azure Portal)

1. Navigated to **VM2** in the Azure portal.
2. Went to **Networking** > selected the **Network Interface**.
3. Opened **IP configurations** > selected `ipconfig1`.
4. Removed the **Public IP address** by setting it to `None`.
5. Clicked **Save** to apply changes.

## 🔒 Post-Change Access Method

- VM2 is now only accessible **via private IP** within the VNet.
- Drew can connect to VM2:
  - Via **VM1** as a jump host (SSH into VM1, then SSH to VM2).
  - Or, using **Azure Bastion** if needed in the future.

## ✅ Status

- Public IP successfully removed from VM2.
- PostgreSQL deployment can now proceed securely.
