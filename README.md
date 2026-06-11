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
    - Realm roles -> `admin`
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
- Login with email -> Off

### User profile
- email -> Required -> On
  - Who can edit -> Uncheck user
- firstName -> Required -> Off
- lastName -> Required -> Off

## Authentication

### Flows
- Create Authentik flow (Note that we are not using the Identity Provider Redirector here because the implementation is broken and will make it impossible to log out)
![Authentik flow](Authentik-Flow.png)
- Bind Authentik flow as the browser flow
- Create the Block Registration flow (otherwise registration can still be triggered under certain condition)
![Block Registration flow](Block-Registration-Flow.png)
- Bind Block Registration as the registration flow
- Copy the Block Registration flow and make the Block Reset Credentials flow
- Bind Block Reset Credentials as the reset credentials flow
- Repeat the same for the Block Direct Grant flow

### Required actions
- Update User Locale -> On

Keep the rest off

## Identity Providers
Add Keycloak Open ID connect

- Alias -> Authentik
- Discovery endpoint -> https://auth.metropolis.nexus/application/o/keycloak/.well-known/openid-configuration
- Remove Logout URL
- Client assertion signature algorithm -> ES256
- Hit "Add"
- PKCE -> On
- PKCE Method -> S256
- Backchannel logout -> Off
- Scope -> openid profile email
- Prompt -> Consent
- Access Token is JWT -> On
- Trust Email -> On
- Show in Account console -> Never
- Sync mode -> Force
- Case-sensitive username -> On
- Hit "Save" again
