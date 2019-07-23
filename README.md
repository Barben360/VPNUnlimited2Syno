# VPN Unlimited to Synology

Installs [VPN Unlimited](https://www.vpnunlimitedapp.com/en) OpenVPN profiles for all existing servers on your Synology DiskStation.

It also provides commands to backup all your OpenVPN configurations from your Synology DiskStation or restore previous backups.

## Prerequisites

- You must install Python 3 package to your Synology DiskStation. You can do it from the package manager on the web interface.
  
  ![syno_install_python3](assets/syno_install_python3.png)

- You must enable SSH server on your Synology DiskStation. You can do this from the control panel in the web interface, activating *Advanced Mode* and going to *Terminal and SNMP*, ticking *Enable SSH service*. We'll suppose you keep the default port value 22.

  ![syno_enable_ssh](assets/syno_enable_ssh.png)

- You must have an SSH client on the device you are going to operate from. On Linux or Mac OS, your terminal will be enough. On Windows, you can use [Putty](https://mediatemple.net/community/products/dv/204404604/using-ssh-in-putty-).

- You must get an OpenVPN configuration file from KeepSolid, which will be used to setup the certificates and client key to authenticate to VPNUnlimited. To do this, [authenticate to KeepSolid](https://my.keepsolid.com/login/) and go to [VPN Unlimited user office](https://my.keepsolid.com/products/vpn/). Then, generate an OpenVPN configuration for a new device. Rename the downloaded file to `template.ovpn`.

  ![vpnunlimited_openvpn_conf_gen](assets/vpnunlimited_openvpn_conf_gen.png)

## Copy scripting and configuration files to your Synology DiskStation

*VPNUnlimited2Syno* must be run directly from your Synology DiskStation, so files must be copied there first. The simplest way to do it is once again to use the web interface.

**<u>Warning:</u>** The scripts must be run as root, so you need to use a Synology account which user is in *administrators* group. To check if a user is in the *administrators* group, go to *Control Panel/User*, double-click on the user's name and go to *User groups*. If *administrators* is ticked, everything is okay.

First, clone this repository or download it as a *.zip* and uncompress it. In the root folder `VPNUnlimited2Syno`, add the `template.ovpn` file your downloaded before.

Then, upload the whole directory `VPNUnlimited2Syno` to your user's *home* directory through *File Station* on the web interface.

## Run VPNUnlimited2Syno to backup, restore and setup VPN Unlimited OpenVPN

First, connect to your Synology DiskStation through SSH with your terminal or Putty.

On a Linux distribution, this would be something like:

```bash
ssh <username>@<diskstation-address> -p 22
```

Enter your user's password.

Go to the root directory you downloaded before.

```bash
cd VPNUnlimited2Syno
```

Elevate your rights to root. This is compulsory to get access to the OpenVPN configuration folder on your Synology DiskStation. To do this, enter:

```bash
sudo su
```

The root password is required. You chose it when you first installed your DiskStation. Enter it and you are ready to go.

## Backup your current configurations

If you already have OpenVPN configurations installed on your Synology, I advise you to back them up before doing anything else.

First, create a folder called `backups`:

```bash
mkdir backups
```

Then, run:

```bash
python3 VPNUnlimited2Syno backup backups
```

<u>Tip:</u> If you want to backup all your previous configurations and remove them all, run:

```bash
python3 VPNUnlimited2Syno backup backups --clean
```

This can be useful if you accidentally install all VPN Unlimited configurations several times.

You should now have a new folder in `VPNUnlimited2Syno/backups` named with a time stamp. Keep it preciously.

## Setup all VPN Unlimited OpenVPN configurations

Run:

```bash
python3 VPNUnlimited2Syno setup template.ovpn
```

where `template.ovpn` is the file retrieved in the [prerequisites section](#prerequisites).

That's it! You can check that all configurations are here in *Control Panel*, *Network*, *Network Interface*.

![syno_vpnunlimited_conf_result](assets/syno_vpnunlimited_conf_result.png)

To connect to any server, just right-click and *Connect*.

In bonus, all servers which accept P2P are appended with the suffix *_P2P*.

## Restore an OpenVPN backup

If you want to restore an old backup, run:

```bash
python3 vpnunlimited2syno restore backups/SYNO_OPENVPN_BACKUP_<timestamp>
```

where `<timestamp>` is your backup folder suffix. Be careful: restoring a backup erases all current OpenVPN configurations. Consider [doing a backup](#backup-your-current-configurations) first.
