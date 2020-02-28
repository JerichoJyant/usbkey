# USBKey

A bash script that makes working with an encrypted removable storage device such as a USB flash drive easy. The intention is for the device to be used to store private keys, two factor authentication backup codes, password caches, API keys, contracts, and any other senstive data that you need to use on a regular basis but prefer to keep on your physical keychain.

The script allows you to insert your encrypted flash drive, decrypt it, and load your SSH key in one command (`usbkey go`). All you have to do is type your drive's passphrase and your SSH key's passphrase.

I made USBKey because I was tired of entering my SSH key passphrase on every git command that interacted with my upstream origin. I didn't want to permanently unlock the key either. And, I wanted to carry my private key around with me, as my keys are always in my pocket or near me, and my laptop is not.

Unix/Linux only! (May work on macOS, try at your own risk)

## Usage:
The term 'usbkey' refers to the bash script in this repository. 'USBKey' refers to your encrypted USB flash drive.

When you plug in your USBKey, run `usbkey unlock` and enter your drive's password (You will choose it when you set up your drive). If you need to use your ssh key in a terminal session (I use it for git), run `usbkey ssh` and input your SSH key passphrase. Your passphrase will be saved in ssh-agent for the entirety of that terminal session. `usbkey go` runs both `usbkey unlock` and `usbkey ssh`, in that order.

When you are done with your USBKey, run `usbkey lock`. This will lock and unmount your USBKey. You may also run `usbkey shutdown`, which will lock, unmount, and deactivate your usbkey. You wil need to remove it and reinsert it to use it again. **Warning:** Do not remove your USBKey until you have run `usbkey lock` or `usbkey shutdown`. Data loss and unexpected behaviour may occur.

From the script's help output:

```
usage: unlock -- decrypt and mount your usbkey
usage: ssh -- load SSH key from your usbkey
usage: go -- shortcut to run both 'unlock' and 'ssh'

usage: lock -- encrypt and unmount your usbkey
usage: shutdown -- encrypt, unmount, and disable your usbkey until removed and replaced
```

## Setting up your drive:
**Read this section and the drive setup instructions carefully before taking any action.**

To erase your USB drive and turn it into an encrypted USBKey, follow these [drive setup instructions](https://unix.stackexchange.com/a/329639/397714) from stackexchange's [asciiphil](https://unix.stackexchange.com/users/39176/asciiphil). 

**CAUTION:** Disk partitioning is dangerous and should only be done by someone experienced with Unix/Linux. You will lose all data on the USB drive. Make backups, and be aware that you may lose other data on other disks, such as mission-critical, business related, and sentimental data if you don't know what you're doing. Don't blame me! 

Make sure you find the correct name for your device -- when you run `sudo parted /dev/sda` replace `/dev/sda` with the name of your device (if it's not actually `/dev/sda`). I almost nuked my hard drive by not using the correct name.

Make sure to decide on a name for your USBKey partiton, otherwise it will be called `example` if you simply copy and paste the commands. I chose `keysandbackups`.

I had to slightly modify the instructions for Ubuntu 18.04 and use `/dev/disk/` instead of `/dev/disks`.

## Configuring the script:
The script was designed and tested on Ubuntu 18.04. You need udisksctl, ssh-add, and ssh-agent installed.

Run `cp ~/.bashrc ~/.bashrc-backup` to make a backup of your bash profile.

Add `eval $(ssh-agent)` and `export PATH=$PATH:~/scripts` to your `~/.bashrc`. Replace `~/scripts` with the location you placed the usbkey script. `cd` to it and make it executable with `chmod +x usbkey`.

Edit the usbkey script, changing the `encryptedDriveName="keysandbackups"` variable to the name you chose in the device setup.

## GitHub / SSH key setup
To place an SSH key on your usbkey, with your usbkey unlocked (`usbkey unlock`), follow [GitHub's SSH creation key instructions](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent). Use the path `/media/[Your username]/keysandbackups/id_rsa` instead of pressing enter for the default location. Change `keysandbackups` to the name of your drive, if you used a different name.

If you are using your SSH key for GitHub, edit `/home/[Your Username]/.ssh/config` and add the following to it:

```
Host github.com
	HostName github.com
	IdentityFile /media/[Your Username]/keysandbackups/id_rsa
	User git
```

Refer to your git host's documentation if you are not using GitHub. Once again, replace `keysandbackups` with your perferred drive name.

## Disclamier
This script is not completely finished and may not work well for you. See the LICENSE file for the full disclamier.

Once again, be careful as you may lose data. Also, if you place senstive or valuable data on the drive, you will lose access to it if you forget your passphrase. Make a backup of everything you can not afford to lose and place it in a secure location other than your usbkey.

I recommend reading the script before you use it so you have an idea of what the script does under the hood.

## Contributing
I welcome contributions to this project. Please use GitHub's tools to report bugs, request features, and submit pull requests. Please let me know if you find this project useful! I'm new to open source contributing and welcome all constructive and critical feedback.
