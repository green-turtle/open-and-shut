# Open and Shut

Type in Morse code by repeatedly slamming your laptop shut

[![output](https://user-images.githubusercontent.com/8890878/78181506-882c1a80-7432-11ea-89c0-bee00e9d183f.gif)](https://youtu.be/UAQ60P61vYw)

## Features
* Use a battle-tested encoding trusted by pilots, submariners, and amateur radio nerds
* Type at 100+ words per ~~minute~~ hour
* Innovative [SIGTERM-based concurrency control](https://github.com/veggiedefender/open-and-shut/blob/master/morse_code_close.sh#L8). Semaphores *who??*
* Closing your laptop now types the letter E instead of locking

## Setup
### Dependencies
* [evemu](https://freedesktop.org/wiki/Evemu/)
* [acpid](https://wiki.archlinux.org/index.php/Acpid)

### Choosing your keyboard
Use `evemu-describe` to find the correct path for your keyboard device and replace `KEYBOARD_DEV` in `morse_code_open.sh` with it. 

### Installation
Clone this repository and copy files into `/etc/acpi`
```
git clone git@github.com:veggiedefender/open-and-shut.git
sudo cp morse_code_close.sh morse_code_open.sh morse_code_acpi.sh /etc/acpi/
```

### Configure acpid
Acpid needs to know how and when to run our scripts. Create a file `/etc/acpi/events/lm_lid` with the following contents:
```
event=button/lid.*
action=/etc/acpi/morse_code_acpi.sh
```

### Disable screen lock
**Temporarily:** Run `systemd-inhibit --what=handle-lid-switch cat` and press Ctrl+C when you're done.

**Permanently:** Add `HandleLidSwitch=ignore` to `/etc/systemd/logind.conf` and either run `sudo systemctl restart systemd-logind.service` (this will log you out), or reboot your computer.
