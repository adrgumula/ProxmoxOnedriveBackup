# Proxmox OneDrive Backup
This is my experience (mostly successful) with Proxmox version 8.3.3 in connecting it to Microsoft OneDrive (e.g., from Microsoft 365) as additional storage for backups, ISOs, templates, and more. 
The solution uses [RCLONE](https://rclone.org) package and covers the installation, configuration, connection to OneDrive, and synchronization process between Proxmox and OneDrive.
I acknowledge that the solution presented is far from being perfect, but it works. If you have any suggestions for improvements, feel free to contact me. 


([1) md files example](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax))

## What you need?
- Proxmox server 8+ (should works on Proxmox 7 as well)
- Proxmox server IP address (local) ***PROXMOX_IP*** for example _192.168.1.140_
- OneDrive (I used Office 365 personal wiht 1TB OneDrive storage)
- [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (PC) or any SSH client able to do the port forwarding (tunelling). I use Putty running under CrossOver 24 for MacOS

> [!NOTE] 
> Understand, with Microsoft things can get complicated.. 

## Let's start
1. RClone - Installing & Configuration part 1
    - open Proxmox Node's shell and enter:
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
    - create a tunnel using ***Putty***:
        - Enter ***Host name (or IP address):*** for example: ```192.168.1.140 port:22``` and go to ```Connection -> SSH -> Tunnels```
            - <img src="https://github.com/user-attachments/assets/09b37dec-934b-40d5-8792-c08df6eda48b" width="35%" height="35%"/>
        - Enter ```53682``` as ***Source Port*** and ```127.0.0.1``` as ***Destination*** then click ***Add***
            - <img src="https://github.com/user-attachments/assets/fb51dccc-16b0-48cd-9586-1ab9311967ed" width="35%" height="35%"/>
        - Click ***Open*** at the buttom of the window
        - Click ***Accept*** of ***Putty Security Alert*** Window
            - <img src="https://github.com/user-attachments/assets/98222d6f-7f2c-4775-8b00-eb5a57e166b4" width="35%" height="35%"/>
        - Here you should see the login window to Microsoft 365 _(or whatever it's called at the moment you're reading this ;) )_
        - Do login to Microsoft 365
        - Click ***Allow*** on the ***Allow related Microsoft websites to share the cookies and website data?***
            - <img src="https://github.com/user-attachments/assets/837c3768-7de5-4022-abc9-1562550c9faf" width="35%" height="35%"/>
        - ***Accept*** on the popup windows ***Let this app access your info? (1 of 1 apps)***
            - <img src="https://github.com/user-attachments/assets/ee9dca23-3ad2-493f-9632-712088b03edf" width="25%" height="25%"/>
        - Close the ***Putty*** (tunel) & return to main Proxmox shell



3. RClone - Installing & Configuration part 2
   - Select `1`
       - <img src="https://github.com/user-attachments/assets/ffc6c1fb-c698-4387-9ec8-8c3082852e5a" width="35%" height="35%"/>
   - Select `7`
       - <img src="https://github.com/user-attachments/assets/f536b803-44a4-4da5-861f-33deac1fa1a3" width="35%" height="35%"/>
   - Select `3`, Select "y" ***Yes - default***
       - <img src="https://github.com/user-attachments/assets/d095ee1e-554a-4ae5-9578-52e7b14ca474" width="35%" height="35%"/>
   - Select `y` ***Yes***
       - <img src="https://github.com/user-attachments/assets/79aa228e-e07f-454a-9b76-96f78579c38e" width="35%" height="35%"/>
   - Select `q` ***Quit config***
       - <img src="https://github.com/user-attachments/assets/74528819-2af4-4d6b-8062-71b2c4a8be49" width="35%" height="35%"/>


5. First tests
    - go to ***Proxmox Node's shell***
    - To list the contecnt of the OneDrive's root folder type following (dont forget to add : at the end!):
        - ```
          rclone lsd onedrivesync:
          ```
7. Mounting OneDrive as /mnt/OneDrive (includes missing dependences installation - fuse3)
    - It time to create a folder where you'll your OneDrive content:
        - ```
          cd /mnt
          ```
        - ```
          mkdir onedrive
          ```
        - ```
          cd onedrive
          ```
    - the next step requires ***fuse*** to be installed. To do so just type
        - ```
          apt-get install fuse3 libfuse2
          ```
    - let connect ```/mnt/onedrive``` with online ***OneDrvie*** by entering following:
        - ```
          rclone mount onedrivesync: /mnt/onedrive --vfs-cache-mode writes --daemon --poll-interval 5m
          ```
    - Type following to check if all was connected properly (if ok you should get folder OneDrive content displayed):
         - ```
           ls /mnt/onedrive
           ```
8. Addeding OneDrive drive to Proxmox:
    - In Proxmox go to your ***DataCenter*** -> ***Storage*** and select ***Add -> Directory***
    - Add ***ID***, ***Directory*** path and select ***Content*** as shown below:
    - <img src="https://github.com/user-attachments/assets/6bf41682-791c-4b58-82c8-29e0b0fad89e" width="35%" height="35%"/>

9. Automoving files from the localdrive to OneDrive with 5 minutes intervals:
    - Go to Promox shell and enter:
      - ```
        nano /usr/local/bin/rclone_auto_move.sh
        ```
   - past there following script (you can tune ***WATCH_DIR*** & ***DEST_DIR*** folders as well as ***sleep 300*** if you've picked up something different). After editing press Ctrl+X (Windows) or Control+X (Mac) then Y, then Enter to save & exit.
      - ```
        #!/bin/bash

        WATCH_DIR="/mnt/onedrive"    # Change to your source folder
        DEST_DIR="opendrivesync:"    # Change to your RClone destination

        inotifywait -m -e close_write --format "%w%f" "$WATCH_DIR" | while read FILE
        do
            sleep 300                              # Wait 300s = 5 minutes
            rclone move "$FILE" "$DEST_DIR" --progress
        done
        ```     
    - Install missing dependeces ***inotifywait***
      - ```
        apt install -y inotify-tools
        ```
    - Make above script executable:
      - ```
        chmod +x /usr/local/bin/rclone_auto_move.sh
        ```
    - Run the Script as a Background Service. To ensure the script runs automatically, create a systemd service. Create a new systemd unit file:
      - ```
        nano /etc/systemd/system/rclone-auto-move.service
        ```
    - and past this inside (save & exit)
      - ```
        [Unit]
        Description=Auto move files to cloud via RClone
        After=network-online.target
        
        [Service]
        Type=simple
        ExecStart=/usr/local/bin/rclone_auto_move.sh
        Restart=always
        User=root
        
        [Install]
        WantedBy=multi-user.target
        ```
    - Reload systemd to apply change
      - ```
        systemctl daemon-reload
        ```
    - Enable the service to start on boot:
      - ```
        systemctl enable rclone-auto-move.service
        ```
    - Start the service now:
      - ```
        systemctl start rclone-auto-move.service
        ```
    - Check if it’s running:
      - ```
        systemctl status rclone-auto-move.service
        ```
    - if all went fine you should get simiar message:
      - <img src="https://github.com/user-attachments/assets/da4ccebf-9e15-47dc-84f3-8a3b47aefd2d" width="35%" height="35%"/>

