Ansible Caddy Role
==================

Install Caddy role.

## Role Variables

- `caddy_version`: The version of Caddy that should be installed. (Optional)
- `caddy_packages`: The additional packages that should be included in the Caddy installation. (The packages can be found here: https://caddyserver.com/download)
- `caddy_sites_reverse_proxy`: A list of sites, that should be configured as reverse proxy.
- `caddy_sites_redirect`: A list of sites, that should be configured as redirects.
- `caddy_sites_custom`: A list of sites, that should be configured completely custom.

## Example Playbook

```yaml
- hosts: all
  tasks:
    - ansible.builtin.include_role:
        name: ansible-caddy-installation
      vars:
        caddy_version: 2.10
        caddy_packages:
          - github.com/caddy-dns/digitalocean
        caddy_sites_reverse_proxy:
          - domain_name: "test1.example.com"
            destination_url: https://example.com
          - domain_name: "test2.example.com"
            destination_url: https://example.com
            compression: true
            tls_insecure_skip_verify: true
            headers_up:
              - operation: +
                field: Host
                value: example.com
            tls_dns:
              provider: digitalocean
              config: xxx
        caddy_sites_redirect:
          - domain_name: "test1.example.com"
            destination_url: https://example.com
            tls_dns:
              provider: digitalocean
              config: xxx
          - domain_name: "test2.example.com"
            destination_url: https://example.com
            tls_certificate:
              private_key: private-key-content
              certificate: certificate-content
          - domain_name: "test2.example.com"
            destination_url: https://example.com
            permanent: true
```


## Versioning

In order to have a versioning in place and working, create lightweight tags that point to the appropriate minor release versions.

Creating a new minor release:

```bash
git tag v1
git push --tags
```

Replacing an already existing minor release:

```bash
git tag -d v1
git push origin :refs/tags/v1
git tag v1
git push --tags
```
