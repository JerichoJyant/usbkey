# USBKey

Unix/Linux only! (May work on macOS, try at your own risk)

## Usage:
The term 'usbkey' refers to both your encrypted usb drive and the script in this repo used to manage it.

When you plug in your usbkey, run `usbkey unlock` and enter your drive's password (You will choose it when you set up your drive). If you need to use your ssh key in a terminal session (I use it for git), run `usbkey ssh` and input your SSH key passphrase. Your passphrase will be saved in ssh-agent for the entirety of that terminal session. `usbkey go` runs both `usbkey unlock` and `usbkey ssh`, in that order.

When you are done with your usbkey, run `usbkey lock`. This will encrypt and unmount your usbkey. You may also run `usbkey shutdown`, which will lock and deactivate your usbkey. You wil need to remove it and reinsert it to use it again. **Warning:** Do not remove your usbkey until you have run `usbkey lock` or `usbkey shutdown`. Data loss and unexpected behaviour may occur.

From the script's help output:

```
usage: unlock -- decrypt and mount usbkey
usage: ssh -- load SSH key from usbkey
usage: go -- shortcut to run both 'unlock' and 'ssh'

usage: lock -- encrypt and unmount usbkey
usage: shutdown -- encrypt, unmount, and disable usbkey until removed and replaced
```

## Setting up your drive:
Follow these [drive setup instructions](https://unix.stackexchange.com/a/329639/397714) from stackexchange's [asciiphil](https://unix.stackexchange.com/users/39176/asciiphil). 

**CAUTION:** Disk partitioning is dangerous and should only be done by someone experienced with unix/linux. You will lose all data on the USB drive. Make backups, and be aware that you may lose other data on other disks, such as mission-critial, business related, and sentimental data if you don't know what you're doing. Don't blame me! 

Make sure you find the correct name for your device -- when you run `sudo parted /dev/sda` replace `/dev/sda` with the name of your device (if it's not actually `/dev/sda`). I almost nuked my hard drive by not using the correct name.

Make sure to decide on a name for your usbkey drive, otherwise it will be called `example` if you simply copy paste the commands. I chose `keysandbackups`.

I had to slightly modify the instructions for Ubuntu 18.04 and use `/dev/disk/` instead of `/dev/disks`.

## Configuring the script:
The script was designed and tested on Ubuntu 18.04. You need udisksctl, ssh-add, and ssh-agent installed.

Run `cp ~/.bashrc ~/.bashrc-backup` to make a backup of your bash profile.

Add `eval $(ssh-agent)` and `export PATH=$PATH:~/scripts` to your ~/.bashrc. Replace `~/scripts` with the location you placed the usbkey script. `cd` to it and make it executable with "chmod +x usbkey".

Edit the usbkey script, changing the `encryptedDriveName="keysandbackups"` variable to the name you chose in the device setup.

If you are using your SSH key for GitHub, edit `/home/[Your Username]/.ssh/config` and add the following to it:

```
Host github.com
	HostName github.com
	IdentityFile /media/[Your Username]/keysandbackups/id_rsa
	User git
```

Refer to your git host provider's documentation if you are not using GitHub. Once again, replace `keysandbackups` with your perferred drive name.

## Disclamier
This script is not completely finished and may not work well for you. See the LICENSE file for the full disclamier.

Once again, be becareful as you may lose data. Also, if you place senstive or valuable data on the drive, you will lose access to it if you forget your passphrase. Make a backup of everything you can not afford to lose and place it in a secure location other than your usbkey.

## Contributing
I welcome contributions to this project. Please use GitHub's tools to report bugs, request features, and submit pull requests. Please let me know if you find this project useful! I'm new to open source contributing and welcome all constructive and critical feedback.
