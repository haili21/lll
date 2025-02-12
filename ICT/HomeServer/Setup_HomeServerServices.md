# Enable services

## TODO:

user ...

## remote desktop - ms viewer, vnc or

## FTP

Sure, I can help you with that! Here are the steps to enable FTP and SSH services on an Ubuntu server:

### Enabling FTP Service

1. **Install vsftpd (Very Secure FTP Daemon):**
   ```bash
   sudo apt update
   sudo apt install vsftpd
   ```

2. **Configure vsftpd:**
   - Backup the original configuration file:
     ```bash
     sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.orig
     ```
   - Edit the configuration file:
     ```bash
     sudo nano /etc/vsftpd.conf
     ```
   - Make the following changes:
     ```plaintext
     anonymous_enable=NO
     local_enable=YES
     write_enable=YES
     chroot_local_user=YES
     ```
   - Save and close the file.

3. **Restart vsftpd:**
   ```bash
   sudo systemctl restart vsftpd
   ```

4. **Allow FTP through the firewall:**
   ```bash
   sudo ufw allow 20/tcp
   sudo ufw allow 21/tcp
   sudo ufw reload
   ```

## SSH

### Enabling SSH Service

1. **Install OpenSSH Server:**
   ```bash
   sudo apt update
   sudo apt install openssh-server
   ```

2. **Check SSH Service Status:**
   ```bash
   sudo systemctl status ssh
   ```

3. **Enable SSH to start on boot:**
   ```bash
   sudo systemctl enable ssh
   ```

4. **Allow SSH through the firewall:**
   ```bash
   sudo ufw allow ssh
   sudo ufw reload
   ```

### add user key to login server directly

To add your SSH login key to a remote server for direct login, follow these steps:

1. **Generate SSH Key Pair (if you don't have one)**
If you haven't already generated an SSH key pair, you can do so with:
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
This will create a public key (`id_rsa.pub`) and a private key (`id_rsa`) in the `~/.ssh` directory.

2. **Copy the Public Key to the Remote Server**
You can use the `ssh-copy-id` command to copy your public key to the remote server:
```bash
ssh-copy-id user@remote_server_ip
```
Replace `user` with your username on the remote server and `remote_server_ip` with the server's IP address. You'll be prompted to enter your password for the remote server.

3. **Manually Add the Public Key (if `ssh-copy-id` is not available)**
If `ssh-copy-id` is not available, you can manually add the public key:
- 1. **Copy the public key to the remote server:**
   ```bash
   cat ~/.ssh/id_rsa.pub | ssh user@remote_server_ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
   ```
- 2. **Set the correct permissions on the remote server:**
   ```bash
   ssh user@remote_server_ip
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

### 4. **Test the SSH Login**
Now, you should be able to log in to the remote server without a password:
```bash
ssh user@remote_server_ip
```

## Administrator work

### how to set new added user 's default shell is bash

To set the default shell to Bash for a newly added user on an Ubuntu server, you can follow these steps:

#### 1. **Add the New User**
First, add the new user using the `adduser` command:
```bash
sudo adduser new_username
```
Replace `new_username` with the desired username.

#### 2. **Set the Default Shell to Bash**
You can set the default shell to Bash during the user creation process or after the user has been created.

- During User Creation
When using the `adduser` command, you can specify the shell with the `--shell` option:
```bash
sudo adduser --shell /bin/bash new_username
```

- After User Creation
If the user has already been created, you can change their default shell using the `chsh` (change shell) command:
```bash
sudo chsh -s /bin/bash new_username
```

#### 3. **Verify the Default Shell**
To verify that the default shell has been set to Bash, you can check the `/etc/passwd` file or use the `getent` command:
```bash
getent passwd new_username
```
Look for the shell at the end of the output, which should be `/bin/bash`.

These steps should help you set the default shell to Bash for any new user on your Ubuntu server. If you have any more questions or need further assistance, feel free to ask!

### how to set new user profile to use ohmyzsh or other good shell with rich color schema

To set up a new user profile to use Oh My Zsh or another shell with a rich color scheme, follow these steps:

### 1. **Install Zsh**
First, install Zsh if it's not already installed:
```bash
sudo apt update
sudo apt install git
sudo apt install curl
sudo apt install zsh
```

### 2. **Install Oh My Zsh**
Oh My Zsh is a popular framework for managing Zsh configuration. Install it with the following command:
```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### 3. **Set Zsh as the Default Shell**
Set Zsh as the default shell for the new user:
```bash
sudo chsh -s /bin/zsh new_username

### 4. **Install a Powerline Font**
Powerline fonts are recommended for a better visual experience. Install them with:
```bash
sudo apt install fonts-powerline
```

### 5. **Choose and Configure a Theme**
Oh My Zsh comes with many themes. One of the most popular and customizable themes is Powerlevel10k. Install it with:
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```
Then, set the theme in the `.zshrc` file:
```bash
sed -i 's/ZSH_THEME=".*"/ZSH_THEME="powerlevel10k\/powerlevel10k"/' ~/.zshrc
```

### 6. **Apply the Configuration**
Reload the Zsh configuration to apply the changes:
```bash
source ~/.zshrc
```

### 7. **Customize Powerlevel10k (Optional)**
For further customization, you can run the Powerlevel10k configuration wizard:
```bash
p10k configure
```

## install app

### install Google Chrome

To install Google Chrome on your Ubuntu system, follow these steps:

### 1. **Download the Chrome .deb Package**
1. Open your current web browser (e.g., Firefox).
2. Go to the Google Chrome download page.
3. Click on the **Download Chrome** button.
4. Select the **64 bit .deb (for Debian/Ubuntu)** option and click **Accept and Install**.
5. Save the file to your Downloads folder.

### 2. **Install the Chrome .deb Package**
1. Open a terminal window (you can use `Ctrl+Alt+T`).
2. Navigate to the Downloads folder:
   ```bash
   cd ~/Downloads
   ```
3. Install the downloaded package using the following command:
   ```bash
   sudo apt install ./google-chrome-stable_current_amd64.deb
   ```
4. If there are any dependency errors, fix them with:
   ```bash
   sudo apt install -f
   ```

### 3. **Launch Google Chrome**
Once the installation is complete, you can launch Google Chrome from the applications menu or by typing `google-chrome` in the terminal.

### Additional Tips
- **Set Chrome as Default Browser:** When you first launch Chrome, you can choose to set it as your default browser.
- **Sync with Google Account:** Sign in with your Google account to sync bookmarks, history, and settings across devices.


### How to install Edge

The warning you're seeing indicates that `apt-key` is deprecated. Instead, you should manage keyring files in the `/etc/apt/trusted.gpg.d` directory. Here's how you can do it:

1. **Download the Microsoft GPG key**:
   ```bash
   curl -sSL https://packages.microsoft.com/keys/microsoft.asc -o microsoft.asc
   ```

2. **Move the key to the trusted.gpg.d directory**:
   ```bash
   sudo mv microsoft.asc /etc/apt/trusted.gpg.d/microsoft.asc
   ```

3. **Add the Microsoft Edge repository**:
   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/edge stable main"
   ```

4. **Update your package list**:
   ```bash
   sudo apt update
   ```

5. **Install Microsoft Edge**:
   ```bash
   sudo apt install microsoft-edge-stable
   ```

This method ensures that the key is managed in a more secure and recommended way¹(https://itsfoss.com/apt-key-deprecated/)²(https://www.linuxuprising.com/2021/01/apt-key-is-deprecated-how-to-add.html).

Let me know if you need any further assistance!

Source: Conversation with Copilot, 18/01/2025
(1) [Fixed] apt-key is deprecated. Manage keyring files in trusted.gpg.d. https://itsfoss.com/apt-key-deprecated/.
(2) apt-key Is Deprecated. How To Add OpenPGP Repository Signing Keys .... https://www.linuxuprising.com/2021/01/apt-key-is-deprecated-how-to-add.html.

## NAS
