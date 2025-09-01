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
        caddy_version: 2.9
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

## Build & Publish

To build and publish the role, visit the GitHub Actions page of the repository and trigger the workflow "Release Package" manually.
