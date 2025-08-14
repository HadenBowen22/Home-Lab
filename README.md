
-----

# Active Directory Lab Setup using VirtualBox

This guide outlines the steps to create a full-blown Active Directory lab on your personal computer using Oracle VirtualBox.

-----

## 1\. Download and Install Software

Download and install the necessary software:

  * **Oracle VirtualBox**: The virtualization software.
  * **VirtualBox Extension Pack**: Essential for additional features.
  * **Windows 10 ISO**: For your client machine.
  * **Server 2019 ISO**: For your domain controller.

-----

## 2\. Create and Configure the Domain Controller (DC) Virtual Machine

This section details the setup of the **Domain Controller**.

### 2.1. Initial VM Creation

1.  Create a new virtual machine in VirtualBox named `DC`.
2.  Select **Server 2019** as the operating system.

### 2.2. Network Configuration

1.  Configure two network adapters for the `DC` VM:
      * **Adapter 1**: Set to **NAT** for internet access.
      * **Adapter 2**: Set to **Internal Network** (e.g., `ADLabNet`) for communication with client machines.

### 2.3. Server 2019 Installation & Basic Setup

1.  Install **Server 2019** on the `DC` VM.
2.  Once installed, assign a **static IP address** to the internal network adapter (e.g., `192.168.1.1` with subnet mask `255.255.255.0`).
3.  Rename the server to `DC`.

### 2.4. Active Directory Domain Services (AD DS) Configuration

1.  Install the **Active Directory Domain Services** role.
2.  Promote the server to a domain controller and create a new forest (e.g., `mydomain.com`).

### 2.5. Network Services Configuration

1.  Configure **Routing and Remote Access Service (RRAS)** with **Network Address Translation (NAT)** on the `DC`. This allows clients on your internal network to access the internet through the `DC`.
2.  Set up a **DHCP server** on the `DC` to automatically assign IP addresses to client machines on your internal network.

### 2.6. Create Active Directory Users (Optional)

1.  (Optional) Run a PowerShell script to create a large number of users (e.g., 1000) in Active Directory for testing purposes.

    ```powershell
    # Example PowerShell script to create 1000 users
    # Note: Modify OU and other parameters as needed
    for ($i = 1; $i -le 1000; $i++) {
        $username = "user$i"
        $password = ConvertTo-SecureString "Password123!" -AsPlainText -Force
        New-ADUser -Name $username -SamAccountName $username -UserPrincipalName "$username@mydomain.com" -AccountPassword $password -Enabled $true -DisplayName "User $i"
    }
    ```

-----

## 3\. Create and Configure the Client Virtual Machine

This section details the setup of your **Client Workstation**.

### 3.1. Initial VM Creation & OS Installation

1.  Create a new virtual machine and install **Windows 10** on it.

### 3.2. Network Configuration & Domain Join

1.  Connect the Windows 10 VM to the same **Internal Network** (e.g., `ADLabNet`) as your `DC`.
2.  Rename the client machine to `client1`.
3.  Join the `client1` machine to your newly created domain (e.g., `mydomain.com`).

### 3.3. Test Domain Login

1.  Log in to the `client1` machine using one of the domain accounts you created in Active Directory.

-----

## Conclusion

Once these steps are completed, you will have a functional Active Directory lab environment within VirtualBox, allowing you to experiment with various Windows networking and domain functionalities.
