# Traefik Ansible Setup

Complete Ansible playbook to deploy Traefik reverse proxy with Let's Encrypt SSL.

## Prerequisites

- Ansible installed on your local machine
- SSH access to your server
- Domain pointing to your server IP

## Quick Setup

1. **Update inventory/hosts.ini**
   ```ini
   [traefik_servers]
   notebook ansible_host=YOUR_SERVER_IP ansible_user=YOUR_USER ansible_become_password=YOUR_SUDO_PASS
   ```

2. **Update group_vars/all.yml**
   - Set `domain` to your domain (e.g., fresetech.com)
   - Set `email` for Let's Encrypt notifications
   - Generate dashboard password:
     ```bash
     echo $(htpasswd -nB admin) | sed -e s/\\$/\\$\\$\\$/g
     ```
   - Update `traefik_dashboard_password` with the generated hash

3. **Run the playbook**
   ```bash
   ansible-playbook -i inventory/hosts.ini playbook.yml
   ```

## What Gets Installed

- Docker and docker-compose
- Traefik reverse proxy
- SSL certificates (Let's Encrypt + self-signed fallback)
- Dashboard accessible at traefik.{your-domain}

## Folder Structure

```
traefik-ansible/
├── inventory/
│   └── hosts.ini                 # Host configuration
├── group_vars/
│   └── all.yml                   # Global variables
├── roles/
│   ├── docker/                   # Docker installation
│   ├── certificates/             # SSL certificate generation
│   └── traefik/                  # Traefik deployment
├── playbook.yml                  # Main playbook
└── README.md                      # This file
```

## Usage Examples

### Deploy
```bash
ansible-playbook -i inventory/hosts.ini playbook.yml
```

### Check connection
```bash
ansible all -i inventory/hosts.ini -m ping
```

### Run with verbose output
```bash
ansible-playbook -i inventory/hosts.ini playbook.yml -v
```

## After Deployment

1. Access dashboard at `https://traefik.{your-domain}`
2. Login with username: admin, password: (the one you set)
3. Add Docker services with labels to route through Traefik

## Troubleshooting

- Check Traefik logs: `docker logs traefik`
- Verify DNS: `nslookup traefik.{your-domain}`
- Check Let's Encrypt status: `cat /opt/traefik/data/acme.json`

## Support

For more info:
- Traefik docs: https://doc.traefik.io/
- Ansible docs: https://docs.ansible.com/

## Gitlab Runner
Let me create a guide for setting up a GitLab CI/CD runner on your machine:I've created a complete guide for setting up a GitLab CI/CD runner on your machine!


1. **Install GitLab Runner**
   ```bash
   # Linux
   curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash
   sudo apt-get install gitlab-runner
   ```

2. **Get your registration token**
   - Go to GitLab repo → Settings → CI/CD → Runners
   - Create new runner
