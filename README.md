# DNSUFW

DNS resolution for UFW

UFW Uncomplicated Firewall is based on IP and it's not based on DNS names. This script if runned periodically ( eg.: with CRON as ROOT ) provides this functionality.

## !! BE AWARE !!

If you run this software automatically and periodically as ROOT with CRON you could be locked out your own machine! The script is NOT 100% bullet proof. **You should not use this for ssh connections on remote servers.** To protect sshd I suggest fail2ban with a remote connection to the fail2ban comunity.

**You really need to know what this script does at every step.**

## Requirements

- clestar clear knowledge of what you are doing
- root privileges
- dig
- ufw

### How to satisfy some requirements

- sudo apt update
- sudo apt -y upgrade
- sudo apt install dig ufw

**Do not continue if you do not understand the following two lines. Go out, do something else.**

- consider 'ufw --dry-run'
- sudo ufw allow to any port 22 proto tcp **<-- BE AWARE possible vulnerability**
- sudo ufw enable **<-- BE AWARE otherwise you are going to regret this. A lot.**

_**Fast copy&paste to console ( except the dangerous part )**_

`sudo apt update;sudo apt -y upgrade;sudo apt install dig ufw;`

## Installation

- update your server backup
- mkdir /tmp/dnsufw/
- git clone https://github.com/IacopoOrtis/DNSUFW.git /tmp/dnsufw/
- mkdir ~/dnsUfw/
- cp /tmp/dnsUfw/dnsUfw.\* ~/dnsUfw/
- rm -r /tmp/dnsufw
- chmod -R 500 ~/dnsufw
- chown -R root:root ~/dnsufw

- as root:
  - sudo -s
  - _you should know what you are doing_
  - crontab -e
  - _/5 _ \* \* \* sh /path/to/dnsUfw.sh
  - save & exit
  - edit "dnsUfw.hosts" file
  - test it

## Format of "dnsUfw.hosts"

- proto:port:dnsName
- eg.: 22:tcp:yourdns.no-ip.org
- eg.: 80:tcp:yourdns.no-ip.org
- eg.: 80:tcp:yourdns2.no-ip.org

| parameter | explanation | example | reference |
| --- | --- | --- | --- |
| protocol | accepted protocol from ufw | tcp, udp | Read UFW documentation. |
| port number | Port number to open to the specific dns name | 22,80,81,8080,443 |
| dns name | the dynamic dns name to solve | name.dyndns.org |

## Files default locations & explanation

| File | Default path | Explanation |
| --- | --- | --- |
| dnsUfw.sh | ~/dnsUfw/dnsUfw.sh | main script |
| dnsUfw.conf | ~/dnsUfw/dnsUfw.conf | configuration file |
| dnsUfw.hosts | ~/dnsUfw/dnsUfw.hosts | ufw rules ( you need to edit this ) |
| dnsUfw.ips | /var/tmp/dnsUfw.ips | sript will use this to store dns resolution ips |
| dnsUfw.log | /var/log/dnsUfw.log | log file |

## Tested on

- Debian
- Ubuntu
- Raspbian

## Know issues

### 1) Temporary ( 5 min ) ufw IP deny:

#### 1) Conditions

- Multiple dns names with same port and protocol
- At least two of the above mentioned set with the same IP

#### 1) Event

When a dns name having the same IP, port and proto of others changes IP, depending on the configuration in 'dnsUfw.hosts' file the old ip get denied UNTIL next script run.

#### 1) Temporary fix

Run script twice every time.

#### 1) To know

It's not a big deal for me to lose connectivity for 5 min so it is not in my immediate todo list to fix this issue.
