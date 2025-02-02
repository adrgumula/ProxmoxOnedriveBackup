### ProxmoxOnedriveBackup
This is my stuggles (successful) with Proxmox version 8.3.3 to connect it with Microsoft OneDrive disk (e.g., from Microsoft 365) as additional storage for backups, ISOs, templates, etc. 
The solution uses RCLONE package its installation, configuration, connection wth OneDrive, sychronization between Proxmox <-> OneDrive.
I acknowledge that the solution presented is far from being perfect, but it works. If you have any suggestions on how to improve it, feel free to share. 


([1) md files example](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax))

# What you need?
- Proxmox server 8+ running(did not check it this work on the 7th version)
- Proxmox'S server IP adress (local)
- OneDrive (I used Office 365 personal wiht 1TB OneDrive storage)
- Putty (PC/Mac-Crossover) or any SSH client able to do the port forwarding (tunelling)
- Understaning that with Microsoft thinks can be complicated... 


1. RClone - Installing & Configuration part 1
2. OneDrive autorization voa Putty & WebBrowser
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