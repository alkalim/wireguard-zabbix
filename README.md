# wireguard-zabbix
`wireguard-linux` is a Zabbix template to monitor Wireguard peers on Linux. It discovers all your Wireguard peers and monitors incoming/outgoing traffic usage.

This template assumes you have installed Wireguard with `wireguard-install.sh` script (see [this repo](https://github.com/Nyr/wireguard-install)). It should work with vanilla Wireguard, but I didn't test it.
## Installation
The instructions below are valid for Debian/Ubuntu but the principle should be applicable for other Linuxes as well.
1. Add user zabbix to sudoers by adding the file `zabbix` to `/etc/sudoers.d` with the following line:
   ```
   zabbix ALL=(root) NOPASSWD: /usr/bin/wg
   ```
2. Add access rights to user `zabbix` for `/etc/wireguard/wg0.conf`:
   ```
   setfacl -m 'u:zabbix:--x' /etc/wireguard
   setfacl -m 'u:zabbix:r--' /etc/wireguard/wg0.conf
   ```
3. Add UserParameter to zabbix by adding the file `user_parameters.conf` to `/etc/zabbix/zabbix_agentd.d`:
   ```
   UserParameter=wireguard.peers,/usr/share/zabbix/wg-json-zbx
   ```
4. Copy script `wg-json-zbx` (based on [wg-json](https://github.com/WireGuard/wireguard-tools/tree/master/contrib/json)) from this repo to `/usr/share/zabbix`, make sure it's readable by the user `zabbix`
5. Restart the agent:
   ```
   service zabbix-agent restart
   ```
6. Import the template

