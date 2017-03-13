#VPN Unlimited to Synology by Barben360

##Created 03-13-2017 - Tested on Synology DS216+ only

These scripts are made to automate the installation of VPN Unlimited OpenVPN configurations on a Synology DiskStation.
It also adds a "P2P" flag in the VPN connection names corresponding to VPN Unlimited servers allowing P2P (US-California 1, Canada-Ontario, Romania, Luxembourg and France).
There is no risk to use the scripts as a backup is done. The worst thing that could happen to you could be loosing previously configured openvpn connections. But that would be your fault since that would mean you have deleted your backup ! ;-)

##Prerequisites:

- A paid VPN Unlimited access.
- Your original OpenVPN files. To get them, email support@keepsolid.com:
"Hello,
In order to use your services, I would like to get OpenVPN configuration files.
Can you send them to me ?
Best regards,
\<yourname\>"
You should receive them quickly (it took 30 minutes when I asked them for myself)
- A Synology DiskStation with Python3 installed. It is not pre-installed so just find it in Synology Package Center.
- Remote SSH connection parametered on your DiskStation
- A computer on your local network able to use SSH (for Windows, use PuTTY)
- UDP port 1194 opened on your internet router
Note: you need to be admin on your DiskStation


##How to use Setup script:
- Put the "VPNUnlimited2Syno" folder somewhere on your DiskStation (e.g. your home folder) the way you want
- Put all the VPN Unlimited ".ovpn" files you want to install in the "./VPNUnlimited2Syno/ovpn_files" folder
- With your SSH-ready computer, connect to your DiskStation ("ssh \<username\>@\<diskstation_name\>"):
	- Go to "VPNUnlimited2Syno" folder
	- Run "./Setup" as an administrator (either the script won't be able to write in ovpn configurations)
	- The installation only takes a few microseconds. You can then exit the SSH connexion.
- Go to your Control Panel on Synology's web interface using your usual web browser.
- Go to "Network/Network interface".
- You should see a new list of VPN connections "VPN - VPNUnlimited_\<location\>" sometimes ending with "_P2P".
- To connect any of these VPN connections, just right click, "connect". You are connected !
Note: You can modify parameters performing right click, "modify". Just remember to leave "Username" and "Password" selections empty.

##How to use Restore script:
When running "Setup" script, a "syno_save" folder is created, and a subfolder is added each time you run the script.
Each subfolder was named by current date and time when you ran the "Setup" script.
If you want to restore the ovpn configuration to any of its previous version:
- Rename the subfolder you want to restore using the name "RESTORE" (case sensitive)
- Using the same process as "Setup" script, run "./Restore" as an administrator

