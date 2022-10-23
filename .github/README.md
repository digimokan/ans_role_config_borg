# ans_role_config_borg

Install the Borg backup program, and configure backups to a remote repo.

[![Release](https://img.shields.io/github/release/digimokan/ans_role_config_borg.svg?label=release)](https://github.com/digimokan/ans_role_config_borg/releases/latest "Latest Release Notes")
[![License](https://img.shields.io/badge/license-MIT-blue.svg?label=license)](LICENSE.md "Project License")

## Table Of Contents

* [Purpose](#purpose)
* [Repo And Key Basics](#repo-and-key-basics)
* [Remote Repo Requirements](#remote-repo-requirements)
* [Encryption Key Requirements](#encryption-key-requirements)
* [Supported Operating Systems](#supported-operating-systems)
* [Quick Start](#quick-start)
    * [Use From Playbook](#use-from-playbook)
* [Role Options](#role-options)
* [Contributing](#contributing)

## Purpose

* Install the [Borg](https://borgbackup.readthedocs.io/en/stable/) backup program.
* Configure backups to a Borg remote repo.
* Optionally configure automatic periodic backups to the Borg remote repo.
* Simplify Borg usage by providing a [utility script](../templates/do_borg.j2).

## Repo And Key Basics

* Borg associates one encryption key with one repo.
* A Borg remote repo provider is a remote server running Borg, which can host
  multiple remote repos for a user.
* _WARNING: ensure that all Borg operations are executed from the same user
  account._

## Remote Repo Requirements

* To use this role, an account must exist on a remote Borg repo provider.
* See [remote repos](../defaults/main/remote_repos.yml) for the list of
  supported providers.

## Encryption Key Requirements

### New Remote Repo

* Run this role for the first time, to create the keyfile and initialize the
  remote repo.
* __Use the [utility script](../templates/do_borg.j2) to back up the keyfile__
  to a USB stick or paper copy.

### Existing Remote Repo

* If this role is being run with the goal of using an existing remote Borg repo,
  then the matching repo key must be used.
* Set [`initialize_borg_remote_repo`](../defaults/main/remote_repos.yml) to
  `false` before this role is run for the first time.
* Use the [utility script](../templates/do_borg.j2) to import the keyfile from
  a USB stick or paper copy.

## Supported Operating Systems

* Ubuntu focal (20.04), jammy (22.04)
* Arch Linux
* FreeBSD

## Quick Start

### Use From Playbook

1. Create `requirements.yml` in ansible project root, and add this content:

   ```yaml
   # requirements.yml
   - src: https://github.com/digimokan/ans_role_config_borg
   ```

2. From the project root directory, install/download the role:

   ```shell
   $ ansible-galaxy install --role-file requirements.yml --roles-path ./roles --force-with-deps
   ```

   * _NOTE:_ `--force-with-deps` _ensures subsequent calls download updates_

3. Include the role like any local role, from the project playbook:

   ```yaml
   # playbook.yml
   - hosts: localhost
     connection: local
     tasks:
       - name: "Install the Borg backup program, and configure backups to a remote repo"
         ansible.builtin.include_role:
           name: ans_role_config_borg
         vars:
           borg_user_name: 'user2'
           borg_remote_repo_provider: 'borgbase'
           enable_automatic_backups: true
           automatic_backup_dirs:
             - '/home/user2/Documents/'

   ```

## Role Options

See the role `defaults` files for main role vars listings:

  * [defaults](../defaults/main/)

Define these _required_ vars for the role:

  * `borg_user_name`: name of primary Borg user to configure the backups for
  * `borg_remote_repo_provider`: the pre-existing remote Borg repo server

## Contributing

* Feel free to report a bug or propose a feature by opening a new
  [Issue](https://github.com/digimokan/ans_role_config_borg/issues).
* Follow the project's [Contributing](CONTRIBUTING.md) guidelines.
* Respect the project's [Code Of Conduct](CODE_OF_CONDUCT.md).

