# Simple `iptables` Firewall Script 🛡️

A straightforward shell script for setting up a basic, secure firewall on Debian-based Linux systems (like Parrot OS, Ubuntu, etc.) using `iptables`.

This script configures the firewall with a "deny by default" inbound policy, which is a fundamental security practice. It blocks all incoming traffic and then selectively allows only the connections you need.

---

## Script Logic

The script performs the following actions in order:
1.  **Flushes all existing rules** to ensure a clean slate.
2.  **Sets the default policies**: It sets the `INPUT` and `FORWARD` chains to **`DROP`**, blocking all unapproved incoming and forwarded traffic. The `OUTPUT` chain is set to **`ACCEPT`** to allow your machine to make outbound connections.
3.  **Adds `ACCEPT` rules**: It adds specific exceptions to the `DROP` policy for essential traffic, such as:
    * Localhost (loopback) traffic.
    * Connections that are already established or related.
    * Ports for common services like SSH (22), DNS (53), HTTP (80), and HTTPS (443).

---

## ⚙️ How to Use

1.  **Save the Code**
    Create a file named **`my_firewall.sh`** and paste the provided shell script code into it.

2.  **Make it Executable**
    In your terminal, navigate to the file's location and run:
    ```bash
    chmod +x my_firewall.sh
    ```

3.  **Run the Script**
    Execute the script with root privileges to apply the firewall rules:
    ```bash
    sudo ./my_firewall.sh
    ```

---

## 💾 Making Rules Persistent

By default, `iptables` rules are cleared on reboot. To make them permanent on Debian-based systems, you need to use the `iptables-persistent` package.

1.  **Install the Package**
    ```bash
    sudo apt-get update
    sudo apt-get install iptables-persistent
    ```
    During the installation, you'll be asked if you want to save the current IPv4 and IPv6 rules. Select **Yes**.

2.  **Saving Future Changes**
    If you ever update your **`my_firewall.sh`** script and re-run it, you must save the new rules so they load on the next boot. Use this command to do so:
    ```bash
    sudo netfilter-persistent save
    ```

---

## 🔧 Customization

You can easily customize this firewall by editing the **`my_firewall.sh`** script.

* **To allow a new service:** Add a new line similar to the others. For example, to allow the standard FTP port (21):
    ```bash
    sudo iptables -A INPUT -p tcp --dport 21 -j ACCEPT
    ```
* **To block a service:** Simply remove or comment out the line for the corresponding port.

**Disclaimer:** Always be careful when configuring a firewall, especially on a remote machine. A misconfiguration could lock you out of your system.
