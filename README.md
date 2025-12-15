# System Setup

- Create `/etc/ssh/sshd_config.d/10-certs.conf`:

```
PermitRootLogin prohibit-password
```
- Restart sshd
- Add NGINX's ssh key to `/root/.ssh/authorized_keys`
- rsync TLS certificates to `/srv/authentik/authentik/certs/auth.metropolis.nexus`

## Create the Permanent Admin User

- Create the `kcadmin` user
  - Role mapping -> Assign role -> Real roles -> `role_admin`
- Log out and log into `kcadmin`
- Delete the `admin` user

