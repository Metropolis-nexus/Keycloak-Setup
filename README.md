# System Setup

- Create `/etc/ssh/sshd_config.d/10-certs.conf`:

```
PermitRootLogin prohibit-password
```
- Restart sshd
- Add NGINX's ssh key to `/root/.ssh/authorized_keys`
- rsync TLS certificates to `/srv/authentik/authentik/certs/auth.metropolis.nexus`

# Create the Permanent Admin User

- Create the `kcadmin` user
  - Password -> Set `kcadmin`'s password
  - Role mapping -> Assign role
    - Realm roles -> `role_admin`
- Log out and log into `kcadmin`
- Delete the `admin` user

# Master Realm

## Realm Settings

### General
- User-managed access -> On

### Login
- Login with email -> Off

# Metropolis.nexus Realm
- Manage realm -> Create realm -> `metropolis.nexus`

## Realm Settings

### General
- User-managed access -> On

### Login
- Login with email -> On

### User profile
- firstName -> Required -> Off
- lastName -> Required -> Off

## Identity Providers

Add Keycloak Open ID connect

- Alias -> Authentik
- Discovery endpoint -> https://auth.metropolis.nexus/application/o/keycloak/.well-known/openid-configuration
- Client assertion signature algorithm -> ES256
- Hit "Save"
- PKCE -> On
- Backchannel logout -> On
- Scope -> openid profile email
- Prompt -> Consent
- Trust Email -> On
- Sync mode -> Force
- Case-sensitive username -> On
- Hit "Save" again
