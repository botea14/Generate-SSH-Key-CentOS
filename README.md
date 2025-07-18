# How to Set Up SSH Key Authentication from Windows to Linux 🔑

This guide will walk you through generating an SSH key on your Windows machine and adding it to a Linux server. This allows you to connect securely without needing to enter a password every time, which is especially useful for tools like **SSHFS-Win**.

---

## 💻 Step 1: Generate SSH Key Pair on Windows

First, you need to create a "key pair" on your local Windows computer. This pair consists of a **private key** (which you keep secret) and a **public key** (which you can share).

1.  Open **Command Prompt** or **PowerShell**.
2.  Run the following command to start the key generation process:

    ```cmd
    ssh-keygen -t rsa -b 4096
    ```

3.  You will be asked a few questions:
    * **Enter file in which to save the key...**: You can press **Enter** to accept the default location (`C:\Users\YourUsername\.ssh\id_rsa`).
    * **Enter passphrase (empty for no passphrase):**: For use with SSHFS-Win, you must leave this empty. Just press **Enter**.
    * **Enter same passphrase again:** Press **Enter** again.

> [!IMPORTANT]
> Leaving the passphrase empty is required for many automated tools, but it means anyone with access to your private key file can use it. Protect it carefully!

Your keys are now saved in your user's `.ssh` folder.
* `id_rsa` is your **private key**. Never share this file.
* `id_rsa.pub` is your **public key**. This is the one you'll copy to the server.

---

## 🐧 Step 2: Prepare the Linux Server

Next, log in to your Linux server with your password for the last time to set up the necessary directory and file.

> [!CAUTION]
> The following commands assume you are operating as the `root` user. Allowing direct SSH login for `root` is a security risk. It is often better to use a standard user account and `sudo` when necessary.

1.  Create the `.ssh` directory and the `authorized_keys` file. The `&&` ensures the second command only runs if the first is successful.

    ```bash
    mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys
    ```

2.  Set the correct permissions. SSH is very strict about this; it will not work if permissions are too open.

    ```bash
    chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys
    ```
    * `chmod 700 ~/.ssh` ensures only you can read, write, and execute files in this directory.
    * `chmod 600 ~/.ssh/authorized_keys` ensures only you can read and write the authorized keys file.

---

## 📋 Step 3: Add Your Public Key to the Server

Now, you need to copy the content of your **public key** from your Windows machine and paste it into the `authorized_keys` file on the Linux server.

1.  **On your Windows machine**, display the public key using the `type` command. Copy the entire output to your clipboard.

    ```cmd
    type %USERPROFILE%\.ssh\id_rsa.pub
    ```

2.  **On your Linux server**, open the `authorized_keys` file with a text editor like `nano`.

    ```bash
    nano ~/.ssh/authorized_keys
    ```

3.  Paste the public key you copied from your Windows machine into the `nano` editor.
4.  Save the file and exit. (In `nano`, press `Ctrl+X`, then `Y` to confirm, and `Enter`).

---

## ✅ Step 4: Test the Connection

You're all set! Now you can test the passwordless connection.

1.  Open a new **Command Prompt** or **PowerShell** on your Windows machine.
2.  Try to connect to your server.

    ```cmd
    ssh root@linux.intra
    ```

If everything was set up correctly, you should be logged in immediately without being asked for a password. You can now use this key-based authentication with SSHFS-Win.
