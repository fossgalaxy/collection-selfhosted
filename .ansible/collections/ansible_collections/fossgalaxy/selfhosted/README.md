# fossgalaxy.selfhosted

Composable Ansible roles for running self-hosted services using Podman and systemd quadlets.

This collection provides modular roles for deploying and managing self-hosted applications in homelab or small infrastructure environments.

The goal is simple:

> Provide clean, reusable building blocks — not a framework that takes over your playbook.

---

## Features

- **Composable roles**
  Designed to be included in your own playbooks without enforcing a specific structure.

- **Feature flags**
  Optional components (database provisioning, OIDC, reverse proxy integration) can be enabled or disabled per role.

- **Pluggable extension points**
  Roles expose `*_extra_*` hooks so you can extend behaviour without forking.

- **SELinux-aware**
  Designed to work cleanly on SELinux-enabled systems (e.g. AlmaLinux).

- **Podman + Quadlet based**
  Containers are managed as systemd services using quadlets.

---

## Design Philosophy

This collection is inspired by structured homelab projects like `matrix-ansible-docker-deploy`, but takes a different approach:

- No central orchestration role
- No enforced architecture
- No assumption about your inventory layout
- No "take over your entire playbook" design

You control your infrastructure.
These roles simply manage individual services cleanly and predictably.

---

## Supported Platforms

These roles are developed primarily on:

- **AlmaLinux** (RHEL-compatible)
- systemd
- Podman (quadlet-based services)
- SELinux (enforcing)

They are also intended to support:

- **Debian-based distributions** (Debian, Ubuntu)

Where distribution-specific handling is required (e.g. SELinux behaviour or package names), it is isolated to infrastructure roles.

Application roles are written to remain distribution-agnostic.

---

## Example Usage

Deploy FreshRSS on a host that already has Podman available:

```yaml
- hosts: podman_hosts
  become: true

  roles:
    - role: fossgalaxy.selfhosted.freshrss
      vars:
        freshrss_domain: rss.example.com
        freshrss_admin_pass: "{{ vault_freshrss_admin_pass }}"
        freshrss_db_host: db-primary.internal
```

Each role is self-contained and can be composed alongside others in the same playbook.

---

## Extension Hooks

Most roles provide extension points to avoid hardcoding behaviour:

- `*_extra_env`
- `*_extra_labels`
- `*_extra_volumes`
- `*_extra_secrets`
- `*_extra_quadlet_options`

These allow you to customize deployments without modifying the role itself.

---

## Feature Flags

Optional components can be toggled per role:

- Database provisioning
- OpenID Connect support
- Reverse proxy integration
- Other service-specific features

You can opt out if you manage those components separately.

---

## Infrastructure Roles

Where appropriate, shared infrastructure components are provided as separate roles (e.g. PostgreSQL provisioning).

Application roles consume these via `include_role` but do not require a central orchestration layer.

---

## Status

⚠️ Active development.

Roles are being migrated and refined from internal homelab deployments. Interfaces may evolve until a stable release is tagged.

### AI Disclosure
These roles were originally hand-written, but have been code 'reviewed' and suggestions made by an LLM for improvements. This readme was LLM generated and then human reviewed.

---

## Contributing

Contributions are welcome.

When adding new roles:

- Follow the existing variable naming pattern (`appname_*`)
- Provide `meta/argument_specs.yml`
- Use feature flags for optional components
- Expose extension hooks instead of hardcoding behaviour
- Keep application roles distribution-neutral

If you're unsure, open an issue or draft PR for discussion.

---

## License

GPLv3-or-later
