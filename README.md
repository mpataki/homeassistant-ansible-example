# Example Ansible Based Home Assistant Setup

This example project demonstrates how to use and combine the various ansible roles provided [here]() to produce a home assistant based home monitoring/automation hub with a number of features including:
- Dynamic DNS via DuckDNS
- HTTPS support via letsencrypt and nginx
- Mosquitto MQTT broker (with SSL)
- InfluxDB for time series data
- Grafana for monitoring dashboards
- InfluxDB Backups so you'll never lose your data
- AWS credential management
- Home Assistant config management

This example has two playbooks:
- [`smart_home.yaml`](smart_home.yaml) which provides the functionality mentioned above
- [`upgrade_ha.yaml`](upgrade_ha.yaml) which will fully upgrade your host's packages, upgrade home assistant, and bring the service back up.
  - note that this does upgrade all your packages, which is good for security and other reasons, but also means this is a relatively risky operation depending on what you have installed. It's better to do this regularly to reduce the scope of what each individual upgrade will change, just in case something breaks.

## Initial Setup

This example can be used as a starting place for a home automation repository.

This won't cover steps like flashing Hasbian, etc. as these are well covered elsewhere.

1. Install the various roles from ansible-galaxy:

```
$>  ansible-galaxy install -r requirements.yml

```

3. Update the `inventory.yaml` file with your home assistant host's info (IP, etc).

2. Add you config option to `smart_home.yaml`. Each of these options are described in their respective repositories and in ansible galaxy.

3. Run the playbook:

```
$>  ansible-playbook -i inventory.yaml smart_home.yaml
```

## Upgrade Home Assistant

```
$>  ansible-playbook -i inventory.yaml upgrade_ha.yaml
```

## What this playbook doesn't do:

It's worth reading the README in each role to get a better idea of each, but in general this playbook is for managing your pi (or whatever host you're running HA on). It doesn't configure your router's port forwarding for example, or go through the steps on the DuckDNS UI that are required via the web interface.

## So why use this?

Declarative configuration management is an industry standard best practice -- and for good reason! Using tools like ansible makes these configurations more portable, subject to version control, and much less prone to human error (like oops I fell on my keyboard and deleted my HA configuration.yaml.. now what?).

All of these roles are idempotent, meaning you can run them again and again and always arrive at the same known state. This is great for experimentation. You can easily try something wild and fun, and roll it back if it breaks. Further, if everything is managed through ansible, you could fry your pi's SD, pop in a new one, and be back up and running in minutes. Who doesn't like that?

