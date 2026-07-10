# Keycloak
Deploy Keycloak in a Podman container.

## Example Usage

```
# application servers
- name: Bring database server up
  hosts: appservers
  roles:
    - role: fossgalaxy.infra.container_host
    - role: fossgalaxy.infra.traefik
    - role: fossgalaxy.selfhosted.keycloak
      vars:
        keycloak_domain: "auth.example.com"
```
