# Wireguard

Install Wireguard and Wireguard-UI in Kubernetes.

[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-wireguard-blue.svg)](https://galaxy.ansible.com/ui/standalone/roles/rmasters270/wireguard)

## Requirements

- Run playbook from Ansible controller.
- Kube config added to user profile running the playbook.
- Traefik installed in Kubernetes for the ingress route.
- UDP port 51820 exposed in Traefik.
- DNS entry for `wireguard_ui_hostname` and Traefik ingress route.
- Forward UDP port 51820 from external router to Traefik's UPD IP address.

### Localhost

The role is intended to run from the Ansible controller.  If the playbook is executed on a different host it will fail because the templates must be copied to the target host.

### Kube Config

The host and user running the playbook must have kube config configured.

## Role Variables

| Variable                      | Required | Default                        | Choices                                                                                  | Comments                                                               |
| ----------------------------- | -------- | ------------------------------ | ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| timezone                      | yes      | Etc/UTC                        | [TZ Identifier](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)            | Timezone on Wireguard                                                  |
| wireguard_namespace           | yes      | wireguard                      |                                                                                          | Kubernetes namespace                                                   |
| wireguard_image_version       | yes      | latest                         | [linuxserver.io/wireguard releases](https://hub.docker.com/r/linuxserver/wireguard/tags) | Use the latest version or pin an image version to the deployment.      |
| wireguard_ui_image_version    | yes      | latest                         | [wireguard-ui releases](https://hub.docker.com/r/ngoduykhanh/wireguard-ui/tags)          | Use the latest version or pin an image version to the deployment.      |
| wireguard_ui_hostname         | yes      | wireguard.{{ ansible_domain }} |                                                                                          | Hostname assigned to Wireguard UI.                                     |
| wireguard_ui_endpoint_address | no       | external ip address            |                                                                                          | External IP or DNS entry used to resolve the public IP address.        |
| wireguard_ui_dns              | yes      | 1.1.1.1                        |                                                                                          | DNS IP address for wireguard clients.                                  |
| wireguard_internal_subnet     | yes      | 10.252.1.0/24                  |                                                                                          | CIDR for wireguard client IP address range.                            |
| wireguard_ui_username         | yes      | admin                          |                                                                                          | Default user for Wireguard UI login.                                   |
| wireguard_ui_password         | yes      | admin                          |                                                                                          | Default user password for Wireguard UI login.                          |
| wireguard_storageclass        | no       |                                |                                                                                          | Leave blank to use default storage class                               |
| wireguard_install             | no       | present                        | present, absent                                                                          | Install or uninstall the wireguard container.                          |
| wireguard_ui_post_all         | no       |                                |                                                                                          | Bash commands to run during post up and post down.                     |
| wireguard_ui_post_up          | yes      | default iptables commands      |                                                                                          | Bash commands to run during post up.  Typically `iptables` commands.   |
| wireguard_ui_post_down        | yes      | default iptables commands      |                                                                                          | Bash commands to run during post down.  Typically `iptables` commands. |

## Dependencies

Setup `kube config` for the user account and host.

## Example Playbook

```yaml
- hosts: localhost

  roles:
    - rmasters270.traefik
    - rmasters270.wireguard
```

## License

MIT

## Author Information

Ryan Masters
