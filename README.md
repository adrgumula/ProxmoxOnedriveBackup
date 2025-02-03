### ProxmoxOnedriveBackup
This is my experience (mostly successful) with Proxmox version 8.3.3 in connecting it to Microsoft OneDrive (e.g., from Microsoft 365) as additional storage for backups, ISOs, templates, and more. 
The solution uses [RCLONE](https://rclone.org) package and covers the installation, configuration, connection to OneDrive, and synchronization process between Proxmox and OneDrive.
I acknowledge that the solution presented is far from being perfect, but it works. If you have any suggestions for improvements, feel free to contact me. 


([1) md files example](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax))

# What you need?
- Proxmox server 8+ (should works on Proxmox 7 as well)
- Proxmox server IP address (local) ***PROXMOX_IP*** for example _192.168.1.140_
- OneDrive (I used Office 365 personal wiht 1TB OneDrive storage)
- [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (PC) or any SSH client able to do the port forwarding (tunelling). I use Putty running under CrossOver 24 for MacOS
> [!NOTE] 
> Understand, with Microsoft things can get complicated.. 

# Let's start
1. RClone - Installing & Configuration part 1
    - open Proxmox shell and enter:
      ```
      curl https://rclone.org/install.sh | bash
      ```
    - once done run the following:
      ```
      rclone config
      ```
    - select ***n*** for remote
    - provide name (for example as below) - in this example I use ***onedrivesync***
      ```
      onedrivesync
      ```
    - use the followings:
        ```
        select 36 (Microsoft OneDrive)
        ```
        ```
        client_id - leave blank/hit enter
        ```
        ```
        client_secret - leave blank/hit enter
        ```
        ```
        client_id - type global
        ```
        ```
        tenant - leave blank/hit enter
        ```
        ```
        Edit advance config? - select No
        ```
        ```
        Use web browser to automatically authenticate rclone with remote? - select Yes (default)
        ```
2. OneDrive autorization voa Putty & WebBrowser
    - you should get an error as below as the rclone cannot resolve localpage under ___http://localhost:53682/___
        ```
        NOTICE: Make sure your Redirect URL is set to "http://localhost:53682/" in your custom config.
        ERROR : Failed to open browser automatically (exec: "xdg-open": executable file not found in $PATH) - please go to the following link: http://127.0.0.1:53682/auth?state=xxxxxxxxxxxxxxxxxx
        NOTICE: Log in and authorize rclone for access
        NOTICE: Waiting for code...
        ```
    - create a tunnel using ***Putty***
    - <img src="https://github.com/user-attachments/assets/09b37dec-934b-40d5-8792-c08df6eda48b" width="50%" height="50%"/>
    - <img src="https://github.com/user-attachments/assets/fb51dccc-16b0-48cd-9586-1ab9311967ed" width="50%" height="50%"/>
    - <img src="https://github.com/user-attachments/assets/98222d6f-7f2c-4775-8b00-eb5a57e166b4" width="50%" height="50%"/>



3. RClone - Installing & Configuration part 2
4. First tests 
5. Mounting OneDrive as /mnt/OneDrive (includes missing dependences installation - fuse3)
6. Final tests



> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.
