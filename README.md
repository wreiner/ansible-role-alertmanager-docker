# Ansible Role: alertmanager-docker

Ansible role to create alertmanager docker container.

## Mandatory Role Variables

This role requires you to set the address on which alertmanager should listen.

```
alertmanager_docker__listen_address: "{{ ansible_host }}"
```

The container will be exposed to the _alertmanager_docker__listen_address_ on port 39093.

This role requires you to set the SMTP configuration for the default route:

```
alertmanager_docker__smtp_smarthost: "smtp.somewhere.me:587"
alertmanager_docker__smtp_from: "sender@somewhere.me"
alertmanager_docker__smtp_auth_username: "sender@somewhere.me"
alertmanager_docker__smtp_auth_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
alertmanager_docker__default_mail_receiver_address: "recipient@somewhere.me"
```

## Optional Role Variables

It is possible to add additional routes and receivers:

```
alertmanager_docker__additional_routes:
  routes:
  - matchers:
    - severity="critical"
    receiver: 'pushsafer-webhook'

alertmanager_docker__additional_receivers:
  - name: 'pushsafer-webhook'
    webhook_configs:
    - url: http://<host-or-ip>:9196/alert
      send_resolved: true
      http_config:
        basic_auth:
          username: 'user'
          password: 'password'

```

Specify the alertmanager version you want to deploy:

```
alertmanager_docker__alertmanager_version: "v2.30.3"
```
