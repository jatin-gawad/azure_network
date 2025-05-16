# Configuring NSG Inbound Rules for Specific IP Access on Azure VM

This document provides a step-by-step guide for configuring Network Security Group (NSG) inbound rules in Azure to allow specific IP addresses access to specific ports (80 and 443) on a VM. This setup is useful for restricting access to specific developers or third-party personnel, such as Ron in this case.

## Prerequisites

* Access to the Azure portal.
* A Network Security Group (NSG) associated with the VM.
* The IP addresses of the allowed users (e.g., Ron and yourself).

---

## Steps to Configure NSG Inbound Rules:

### 1. Navigate to the NSG:

* Go to the Azure portal.
* Navigate to **Networking** > **Network Security Groups**.
* Select the NSG associated with your VM.

### 2. Add Inbound Security Rule for Port 80:

* In the NSG settings, click **Inbound security rules**.
* Click **+ Add** to open the Add Inbound Security Rule window.
* Configure the rule as follows:

  * **Source:** IP Addresses
  * **Source IP Addresses/CIDR Ranges:** Enter Ron’s IP address.
  * **Source Port Ranges:** `*`
  * **Destination:** Any
  * **Service:** Custom
  * **Destination Port Ranges:** `80`
  * **Protocol:** TCP
  * **Action:** Allow
  * **Priority:** 380
  * **Name:** `Allow-80-Ron`
* Click **Add** to apply the rule.

**Repeat the above steps to add your own IP address as well:**

* Change **Source IP Addresses/CIDR Ranges** to your IP.
* Change **Name** to `Allow-80-MyIP`.

### 3. Add Inbound Security Rule for Port 443:

* Repeat the above steps for port 443.
* Ensure that the **Destination Port Ranges** is set to `443`.
* Create separate rules for Ron’s IP and your IP.

---

## Example of Configured Rules:

| Priority | Name           | Source IP | Port | Protocol | Action |
| -------- | -------------- | --------- | ---- | -------- | ------ |
| 380      | Allow-80-Ron   | Ron's IP  | 80   | TCP      | Allow  |
| 381      | Allow-80-MyIP  | My IP     | 80   | TCP      | Allow  |
| 382      | Allow-443-Ron  | Ron's IP  | 443  | TCP      | Allow  |
| 383      | Allow-443-MyIP | My IP     | 443  | TCP      | Allow  |

---

## Verification:

To verify the NSG rules are correctly configured, follow these steps:

1. **Review the NSG Rules in Azure:**

   * Go to the Azure portal > Networking > Network Security Groups.
   * Select the specific NSG and navigate to **Inbound security rules**.
   * Confirm that the configured rules for ports 80 and 443 appear in the list with the specified source IPs and priorities.

2. **Testing Access from Allowed IPs:**

   * From Ron’s IP and your IP, attempt to access the VM via HTTP (port 80) and HTTPS (port 443) using a web browser or tools like `curl`.
   * Example:

     * `curl http://<VM_Public_IP>:80`
     * `curl https://<VM_Public_IP>:443`
   * Expected Result: Access should be allowed from the specified IPs and blocked from other IPs.

3. **Testing Access from Unlisted IPs:**

   * Attempt the same requests from an unlisted IP. The connection should be denied if the rules are correctly configured.

4. **Network Watcher - NSG Flow Logs:**

   * Use Azure Network Watcher to monitor the NSG flow logs to confirm the traffic is being allowed/blocked as per the rules.

5. **Network Security Group Diagnostics:**

   * In the NSG, click on **Network security group diagnostics** to run a diagnostic test.
   * Verify that the test results align with the expected behavior of the configured rules.

---

## Notes:

* Ensure that the priority numbers are set in ascending order to prevent unintended blocking by other rules.
* Regularly review the NSG rules to ensure that only intended IPs have access to the specific ports.
