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
