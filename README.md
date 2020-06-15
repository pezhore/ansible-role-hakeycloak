HA Keycloak
=========

This role should be applied after `geerlingguy.java` and `andrewrothstein.keycloak` to alter the default install of Keycloak to:

1. Create a `keycloak` user and group
2. Create an admin user/password based on the `keycloak_admin_user` and `keycloak_admin_password` role variables
3. Create the necessary folder structure for postgres
4. Download the jdbc driver
5. Make the necessary xml changes to enable postgres as the backend database
6. Create a `launch.sh` script, launcher config file, and systemd service
7. Start/enable the keycloak service

Requirements
------------

* Currently, this only has been tested on Keycloak version 10.0.2 - your mileage will vary with other versions (probably poorly as I'm doing a wholesale replacement of the standalone xml files.
* See the sample playbook for specific settings that have been tested for the java/keycloak
* **This role should have `become` set to `yes`.**


Role Variables
--------------

Multiple variables are leveraged for this role


* `keycloak_version`: Used to derive the `keycloak_root` and subsequent `keycloak_standalone_config` / `postgres_module_path` (see `defaults/main.yml`)
* `keycloak_admin` and `keycloak_admin_password`: Used for first admin account creation 
* `postgres_jar`: Used to specify which version of PostgreSQL Keycloak will use (used to derive the `postgres_module_path`).
* `postgres_server`: Indicates the address for the postgres instance
* `postgres_db`: Indicates the database to use for postgres (note this should have UTF-8 encoding)
* `postgres_username` / `postgres_password`: Connection account/details


Dependencies
------------

* **`geerlingguy.java`**

```
java_packages:
  - openjdk-8-jdk
```

* **`andrewrothstein.keycloak`**

```yaml
keycloak_ver: '10.0.2'
keycloak_checksums:
   '10.0.2': sha1:2b90bdefd3c837b2f3cc3d44263b6503cd6fbf62
```

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: range.hakeycloak, become: yes }

License
-------

BSD

