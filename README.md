django-ansible-vagrant-nfs-template
===================================

Vagrant/Ansible development-orientated template suitable to ship with a Django project included.

This is forked from <https://github.com/jcalazan/ansible-django-stack>

## Changes to this fork

- Use this as a project template that you check into git *with* your project included, not to deploy an external git repo (replace /example_project)
- Configures /vagrant NFS mount to project root, so you can access directly from a IDE/git client from your host (no need to copy files in/out of VM)
- Cleans up directory/file structure (ansible files now live under ansible/)
- Intentionally only provisions development environment (removes everything else); production deployments can be based off this (see ansible/env_vars/*.yml), but should be an explicit step with specific configuration oversight
- VM IP address centralised in env_vars/development.yml (instead of hard coded)
- Vagrantfile/Virtualbox config improvements (2GB, 2CPU, X11 forwarding)
- 'localrepo' role to ensure Ubuntu is using a local mirror
- Uses latest Ubuntu LTS (14.04)
- Dynamically generates self-signed SSL certificate (instead of including private key)
- Dynamically generates Django secret (instead of including)
- Doesn't deploy virtualenv/django project as root (instead as custom user)
- Uses avahi to make VM available at hostname.local
- Misc other improvements/fixes

## Overview / Technology Stack

Ansible Playbook designed for environments running a Django app.  It installs & configures the following technology stack;

- Ubuntu 14.04 LTS
- Python 2.7
- Django 1.8
- Nginx
- Gunicorn
- PostgreSQL
- Supervisor
- virtualenv
- Memcached
- Celery
- RabbitMQ

Default settings (including app to deploy, hostname) are stored in ```roles/role_name/vars/main.yml```.
Development settings (including git branch, VM IP address) are stored in ```roles/role_name/vars/development.yml```

## Getting Started

Ensure you have the tools in Requirements below installed.

Your django project is designed to replace example_project/*

1. ```git clone``` this repo
2. (skip if doing default example_project deploy) Either use example_project as a basis for a new Django project, or replace it with your Django project (for example, 'django-votingtool' - see expected project structure below).
3. (skip if doing default example_project deploy) edit ansible/env_vars/base.yml and development.yml to reflect your Django project details & hostname/IP
4. (skip if doing default example_project deploy) If you've deployed a different Django project, migrate over appropriate settings from project/project/celery.py, project/project/settings.py
5. ```vagrant up```
6. After deploy, access your project by going to the appropriate URL. i.e.: https://exampleproject.local or https://192.168.42.100

Note project structure assumptions below.  If your project requires additional system packages installed, you can add them in `roles/web/tasks/install_additional_packages.yml`.

## Requirements

- [Ansible](http://docs.ansible.com/intro_installation.html)
- [Vagrant](http://www.vagrantup.com/downloads.html)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

The main settings to change here is in the **env_vars/base** file, where you can configure the location of your Git project, the project name, and application name which will be used throughout the Ansible configuration.

Note that the default values in the playbooks assume that your project structure looks something like this:

```
myproject
├── manage.py
├── myproject
│   ├── apps
│   │   └── __init__.py
│   ├── __init__.py
│   ├── celery.py 
│   ├── settings
│   │   ├── base.py
│   │   ├── __init__.py
│   │   ├── local.py
│   │   └── production.py
│   ├── templates
│   │   ├── 403.html
│   │   ├── 404.html
│   │   ├── 500.html
│   │   └── base.html
│   ├── urls.py
│   └── wsgi.py
├── README.md
└── requirements.txt
```

The main things to note are the locations of the `manage.py` and `wsgi.py` files.  If your project's structure is a little different, you may need to change the values in these 2 files:

- `roles/web/tasks/setup_django_app.yml`
- `roles/web/templates/gunicorn_start.j2`

## Contact

- Ryan Verner <ryan.verner@gmail.com>
- Twitter/Github: @xfxf
