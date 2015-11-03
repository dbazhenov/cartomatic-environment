## About

Cartomatic environment for developers.

## Installation

Rename config from 'settings.yml.example' to 'settings.yml' and edit parameters:

    ---

    settings:
      source_path: "provisioner"
      playbook: "lamp.yml"
      config_path: "config/advanced.json"
      tags:
        - deploy

    nodes:
      trusty:
        ip_addr: 10.0.0.10
        image: ubuntu/trusty64

_Specify the directory with Cartomatic, playbook and config path. Tags are optional._

Run vagrant:

    vagrant up trusty

Please wait. It may takes from 5 to 15 minutes. Done.

## License

GNU Public Licence 3 (GPL v3)
