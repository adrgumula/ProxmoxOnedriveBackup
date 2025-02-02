### ProxmoxOnedriveBackup
This is my stuggles (successful) with Proxmox version 8.3.3 to connect it with Microsoft OneDrive disk (e.g., from Microsoft 365) as additional storage for backups, ISOs, templates, etc. 
The solution uses RCLONE package its installation, configuration, connection wth OneDrive, sychronization between Proxmox <-> OneDrive.
I acknowledge that the solution presented is far from being perfect, but it works. If you have any suggestions on how to improve it, feel free to share. 


([1) md files example](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax))

# What you need?
- Proxmox server 8+ running(did not check it this work on the 7th version)
- Proxmox'S server IP adress (local) ***PROXMOX_IP*** for example _192.168.1.140_
- OneDrive (I used Office 365 personal wiht 1TB OneDrive storage)
- Putty (PC) or any SSH client able to do the port forwarding (tunelling). I use Putty running under CrossOver 24 for MacOS
- Understaning that with Microsoft thinks can be complicated... 

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
    client_id - leave blank/hit enter
    client_secret - leave blank/hit enter
    client_id - type global
    tenant - leave blank/hit enter
    Edit advance config? - select No
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
    - <img src="https://github.com/user-attachments/assets/09b37dec-934b-40d5-8792-c08df6eda48b" width="400" height="400"/>
    - <img src="https://github.com/user-attachments/assets/fb51dccc-16b0-48cd-9586-1ab9311967ed" width="400" height="400"/>
    - <img src="https://github.com/user-attachments/assets/98222d6f-7f2c-4775-8b00-eb5a57e166b4" width="400" height="400"/>



3. RClone - Installing & Configuration part 2
4. First tests 
5. Mounting OneDrive as /mnt/OneDrive (includes missing dependences installation - fuse3)
6. Final tests

> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.
