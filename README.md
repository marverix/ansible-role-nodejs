# Ansible Role: Node.js

[![Build Status](https://travis-ci.com/marverix/ansible-role-nodejs.svg?branch=master)](https://travis-ci.com/marverix/ansible-role-nodejs)
![Ansible Quality Score](https://img.shields.io/ansible/quality/47511)
![Ansible Role](https://img.shields.io/ansible/role/47511)
[![License: ISC](https://img.shields.io/badge/License-ISC-blue.svg)](LICENSE)

Ansible role that installs on linux Node.js, npm and optionally does basic npm configuration.

## Features

- ✔️ Installing Node.js
  - You can define which version should be installed
  - Double-check that the newest version of `npm` is installed
- ✔️ `npm` configuration
  - Set global registry for an user
  - Set prefix for an user
  - Set default scope for an user
  - Configure scopes for an user
- ✔️ Installing global `npm` packages that you wish
- ✔️ Making sure that `nodejs` alias is available
- ✔️ Tested with Molecule Verify

## Supported Platforms

- ✔️ Ubuntu 16.04 (Xenial)
- ✔️ Ubuntu 18.04 (Bionic)
- ✔️ Ubuntu 20.04 (Focal)
- ✔️ CentOS 7
- ✔️ CentOS 8

## Requirements

None

## Role Variables

Variable | Description | Default Value
--- | --- | ---
`nodejs_version` | Version of Node.js to be installed | `14`
`nodejs_npm_install_globally` | List of `npm` packages that should be installed globally | `[]`
`nodejs_npm_config` | List on `npm` configurations - See in section _How to configure npm_ | `[]`

### How to configure npm

`nodejs_npm_config` must be an array of objects. Here is how each object should look like:

Property | Description | Required
--- | --- | ---
`user` | User (one npm config per user) | Yes
`prefix` | npm prefix | No
`registry` | npm registry URL | No
`scopes` | List of scopes. Each scope must have `name` (without `@`) and `registry`. Look at playbook examples below. | No
`default_scope` | Default scope | No

## Dependencies

None

## Example Playbook

1. The simplest one

    ```yml
    ---
    - hosts: all
      roles:
        - marverix.nodejs

    ```

1. Install globally `mocha` and `eslint`

    ```yml
    ---
    - hosts: all
      roles:
        - role: marverix.nodejs
          vars:
            nodejs_npm_install_globally:
              - mocha
              - eslint
    ```

1. Set `npm` registry for user `root`, set prefix, configure scopes and set default scope:

    ```yml
    ---
    - hosts: all
      roles:
        - role: marverix.nodejs
          vars:
            nodejs_npm_config:
              - user: root
                prefix: /home/root/.node
                registry: https://nexus.example.org/repository/npm/
                scopes:
                  - name: example-int
                    registry: https://nexus.example.org/repository/npm-int/
                  - name: example2-int
                    registry: https://nexus.example2.org/repository/npm-int/
                default_scope: example-int
    ```

    BTW: Here is good blog post how to setup Nexus as your `npm` registry:
    https://blog.sonatype.com/using-nexus-3-as-your-repository-part-2-npm-packages

## License

ISC
