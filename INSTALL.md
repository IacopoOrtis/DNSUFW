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

_**Fast copy&paste to console for the above part**_

`mkdir /tmp/dnsufw/;git clone https://github.com/IacopoOrtis/DNSUFW.git /tmp/dnsufw/;mkdir ~/dnsUfw/;cp /tmp/dnsUfw/dnsUfw.\* ~/dnsUfw/;rm -r /tmp/dnsufw;chmod -R 500 ~/dnsufw;chown -R root:root ~/dnsufw;`

- as root:
  - sudo -s
  - _you should know what you are doing_
  - crontab -e
  - `*/5 * \* \* \* sh /path/to/dnsUfw.sh`
  - save & exit
  - edit "dnsUfw.hosts" file
  - test it
