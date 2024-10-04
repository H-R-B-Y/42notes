#### Check UFW is on
`sudo systemctl status ufw`

#### Check SSH is on
`sudo systemctl status ssh`
`sudo systemctl status sshd`
See above.
#### Check User is sudo and user42 groups
`groups {username}`
Will list the groups said user is a member of.

#### create a user
`adduser {username}`
Will create a new user and prompt for some basic information about the user.
Uses the `skel` folder to initialize the users `home/{username}` folder.

## Password

Check that user is actually effected by password policy:
```
sudo chage --list {username}
```

```			From /etc/login.defs
PASS_MAX_DAYS 30 - Password must be reset after 30 days
PASS_MIN_DAYS 2  - Password cannot be changed for 2 days once changed
PASS_WARN_AGE 7  - Warn user after 7 days about password age
```

The password age policy wont take effect for users that already exist, so the following commands can be used to set the age controls for those users:
```
chage -M {days}  :: the max password age
chage -m {days}  :: the min password age 
chage -W {days}  :: Warning days
```

For the password character controls:
```
minlen=10				- Minimum length 10 chars
ucredit=-1				- Uppercase Credit -1 ENSURE 1 UPPERCASE LETTER
lcredit=-1 				- Lowercase Credit -1 ENSURE 1 LOWERCASE LETTER
dcredit=-1 				- Digit Credit -1 ENSURE 1 UPPERCASE LETTER
maxrepeat=3				- The maximum number of times a character can be repeated
reject_username			- Username and password cannot be the same
difok=7					- Atleast 7 characters different from last password
enforce_for_root		- Applies to root password
```

Then to enforce the new rules you will need to run:
```
passwd {username}
```
To change the password for a user.

#### Create a new group "Evaluating"
To create a new group
```
groupadd <groupname>
```

#### Add user to group
`usermod -aG {groupname} {username}`

#### Is hostname correct
Easy way to check hostname: 
```
hostnamectl status
```

#### Modify hostname and restart:
```
hostnamectl set-hostname {new hostname}
reboot now
```

#### How to view partitions:
`df`
or 
`lsblk`

#### Is sudo installed 
```
dpkg -l (lists installed packages)
Piped into:
grep "sudo"

$ dpkg -l | grep sudo 
```

#### Assign user to sudo group
`sudo usermod -aG sudo {username}`

#### explain value of sudo and what it does
Sudo (Super User do) allows users within a select group (or explicitly named in the config) to use the `sudo` command followed by any other command to execute the second command as if they had root privileges. 

#### Show and explain the rules implemented.
```sh
Defaults        secure_path
```

```sh
Defaults        authfail_message
Defaults        badpass_message
Defaults        passprompt
```

```sh
Defaults        logfile="/var/log....."
```

```sh
Defaults		requiretty
```

#### verify the sudo log is there and updates on sudo's use.\
```sh
cd /var/log/sudo
cat log > a
sudo {something}
cat log > b
diff a b
```

#### Is UFW on
`sudo systemctl status ufw`

#### Is it working?
start an ssh session, update the rules to exclude ssh port, restart ufw, stop ssh session, try to reconnect to ssh session.
```sh
EVALUATOR SIDE:
ssh {username}@{VM address} -p {ssh port}

VMSIDE:
ufw status numbered
ufw delete {number}
ufw deny {ssh port}
systemctl restart ufw

EVALUATOR SIDE:
logout
ssh {username}@{VM address} -p {ssh port}

```
#### Is 4242 (or ssh port) open
`sudo ufw something something`

#### add new rule
`sudo ufw allow {port number}`
Adds a new rule to the UFW table.

need to run:
`sudo systemctl restart ufw` 
for it to take effect.

#### Is SSH on?
`sudo systemctl status sshd`

#### is it working properly
#### what is ssh and why use it?

#### verify it is using 4242 (or alt port)
```sh
sudo systemctl status sshd
```
Will show you what ports the ssh service is listening on.
Alternatively:
```sh
cat /etc/ssh/sshd_config | grep "Port "
```


check cron configuration:
```
sudo nano /etc/crontab
```
or 
```
crontab -e
sudo crontab -e
```



#### Did you setup the partitions correctly?
```
lsblk
```
or
``` 
du
```

#### Is wordpress installed and working with the right services\
```sh
# for version info of current mariadb installed:
mariadb --version

# to see that wordpress is setup to use it:
mariadb
(mariadb) > SHOW DATABASES;

#you can now see the table `wpdb` (my wordpress database)
(mariadb) > EXIT;

# then you can do:
# to see the configured database name to use in the localhost database
cat /var/www/html/wp-config.php | grep "DB_NAME"  
```

```sh
# to see that lighttpd is working:
sudo systemctl status lighttpd
```

To see wordpress working:
From local machine:
```sh
firefox http://localhost:{forwarded IP address}
```
#### is there another service

#### can you explain the service.


```sh
systemctl status fail2ban
```


