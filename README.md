# puncentral-teamspeak

Ansible playbook to deploy a TeamSpeak 3 server on Ubuntu 24.04.

## Requirements

```bash
ansible-galaxy collection install community.general
```

## Inventory

Create an `inventory.ini` (gitignored):

```ini
[teamspeak]
1.2.3.4 ansible_user=root
```

## Usage

```bash
ansible-playbook -i inventory.ini teamspeak.yml
```

## Monitoring & security (optional)

Installs GoAccess (log reports) and Fail2Ban (intrusion prevention) alongside TeamSpeak.

```bash
ansible-playbook -i inventory.ini monitor.yml
```

Access the GoAccess report via SSH tunnel:

```bash
ssh -L 8080:127.0.0.1:8080 root@1.2.3.4
# then open http://localhost:8080
```

Check Fail2Ban jail status:

```bash
fail2ban-client status
fail2ban-client status nginx-botsearch
fail2ban-client status nginx-http-auth
fail2ban-client status sshd
fail2ban-client status teamspeak
```

Run only a specific part:

```bash
ansible-playbook -i inventory.ini monitor.yml --tags monitoring-fail2ban
ansible-playbook -i inventory.ini monitor.yml --tags monitoring-verify
```

## Geoblock (optional)

Restricts inbound traffic to Norwegian IPs only (IPv4) using `ipset` + ipdeny.com zone data.

**Warning:** Run this only from a Norwegian IP or you will lock yourself out.

```bash
ansible-playbook -i inventory.ini geoblock.yml
```

IP ranges are refreshed weekly via cron (Sunday 03:00). To trigger manually on the server:

```bash
/usr/local/bin/update-geoip.sh
```

## Ports opened

| Port  | Protocol | Purpose       |
|-------|----------|---------------|
| 9987  | UDP      | Voice         |
| 10011 | TCP      | ServerQuery   |
| 30033 | TCP      | File transfer |

## Variables

Defined in `teamspeak.yml` under `vars`:

| Variable     | Default                                      |
|--------------|----------------------------------------------|
| `ts_version` | `3.13.7`                                     |
| `ts_user`    | `teamspeak`                                  |
| `ts_dir`     | `/opt/teamspeak`                             |
| `ts_url`     | Download URL derived from version and dir    |
