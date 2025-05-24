I always felt an uneasy while using arrow keys in my system. So, I mapped 'Caps + i/j/k/l' to work as arrow keys. This help me not life my hands off the keyboard to press the arrow keys. 

My OS is linux. So, this will be useful only for linux users.
Steps:
1. Install kmonad (tool which facilitates this)
2. Update the config file
3. Make this as a user service to enable this during startup

Install kmonad
The installation step seems complex. Luckily a pre-built binary is avilable in the repo.
Link - https://github.com/kmonad/kmonad/releases

Config file which has the keyboard changes that's required. Go thru the kmoad repo to get the syntax of the config files.
Here the config_files/ directory has a couple of config files targetting different keyboards. Each config file targets a specific keyboard. Since I use multiple keyboards, I've created multiple config files.
The keyboard id can be taken from /dev/input/by-path/ and here u fill find the keyboard ids which u can use in the config.
The config file in this repo gives the arrow keys functionality to 'caps + i/j/k/l'.

After updating the config file, check its working by using '/path/to/kmonad/ config.kbd'. This should give ur desired keyboard functionalities. If not, something's wrong with the script.

If the config is working propoerly, make this as a service and enable it to get triggerd at startup.
Now create a user service for kmonad
1. Paste ur config file in ~/.config dirctory
2. Create a config directory for systemd (This is responsible for managing services) inside ~/.config directory
3. Create a service file

The systemd_service_files/ in this repo has the sample service files. The ExecStart is important. Point that to correct executable and the config file. Once done,
systemctl --user start kmonad.service -> this will start the service. u can verify if ur changes are working.
TO make this permanent use
systemctl --user enable kmonad.service -> this will enable this service during device startup.
kmonad needs super user permissions to update the keyboard device. So, in the ExecStart in 'kmonad.service', sudo is used in the prefix. This sudo will ask for password. So the service will fail due to this reason. To counter this, make this service not ask for password.<user> ALL=(ALL) NOPASSWD: <path to kmonad exec> <path to kmonad config file>
This will prevent kmonad from asking passwords during sudo execution.


