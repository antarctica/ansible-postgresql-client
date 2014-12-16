# PostgreSQL Client (`postgresql-client`)

**Part of the BAS Ansible Role Collection (BARC)**

Installs PostgreSQL database client and configures password files for users
## Overview

* Installs PostgreSQL client package.
* Configures `.pgpass` files for *app* and *controller* users to prevent password prompting, an entry for the optional app database (provided by `postgresql-server`) is included by default through `postgresql_client_pgpass_defaults`. Other connections can be provided at run time using `postgresql_client_pgpass_user`.

## Availability

This role is designed for internal use but if useful can be shared publicly.

## Usage

### Requirements

#### BAS Ansible Role Collection (BARC)

* `core`

### Variables

* `postgresql_client_controller_user_username`
    * The username of the controller user, used for management tasks, if enabled
    * This variable **must** be a valid unix username
    * Default: "controller"
* `postgresql_client_controller_postgresql_user_username`
    * The username of the controller PostgreSQL user, used for managing the database server, if enabled
    * This variable **must** be a valid PostgreSQL user
    * Default: "controller"
* `postgresql_client_controller_postgresql_user_password`
    * Default password for controller user (i.e. root).
    * MUST NOT contain ":" or "\" characters to ensure compatibility with `.pgpass` files.
    * Default: "stirring-up^the=flames$381194££iz€JQ4"
* `postgresql_client_app_user_username`
    * The username of the app user, used for day to day tasks, if enabled
    * This variable **must** be a valid unix username
    * Default: "app"
* `postgresql_client_app_postgresql_user_username`
    * The username of the app PostgreSQL user, used for day to day database tasks, if enabled
    * This variable **must** be a valid PostgreSQL user
    * Default: "app"
* `postgresql_client_app_postgresql_user_password`
    * Default password for app user.
    * MUST NOT contain ":" or "\" characters to ensure compatibility with `.pgpass` files.
    * Default: "chase-PaX-87524"
* `postgresql_client_pgpass_defaults`
	* Default connections to include in per user `pgpass` files.
	* Default: (array)
		*  "controller"
			*  lines: (array)
				*  hostname: "*"
				*  port: "*"
				*  database: "*"
				*  username: "{{ postgresql_client_controller_postgresql_user_username }}"
				*  password: "{{ postgresql_client_controller_postgresql_user_password }}"
		*  "user"
			*  lines: (array)
				*  hostname: "*"
				*  port: "*"
				*  database: "*"
				*  username: "{{ postgresql_client_app_postgresql_user_username }}"
				*  password: "{{ postgresql_client_app_postgresql_user_password }}"

* `postgresql_client_pgpass_user`
	* Optional additional connections to include in per user `pgpass` files.
	* Default: []  (empty array)

#### `.pgpass` files

`postgresql_client_pgpass_defaults` and `postgresql_client_pgpass_user` are used to create a `.pgpass` file (defined [here](http://www.postgresql.org/docs/current/static/libpq-pgpass.html)), their format is as follows:

    <os_user>
      lines:
        - hostname: <hostname>
          port: <port>
          database: <database>
          username: <username>
          password: <password>

Entries defined in this file are tried in order, therefore `postgresql_client_pgpass_user` entries are outputted before `postgresql_client_pgpass_defaults`.  

If using wildcard options ensure specific entries are suitably elevated.

* `<os_user>`
	* Determine which user account the `.pgpass` file should be outputted to
	* MUST be the name of a user account present on the host, usually either "app" or "controller"
	* E.g. "controller"
* `lines`
	* A line to be added to the `.pgpass` file consisting of the following key/values:
		* `hostname: <hostname>`
			* Either a specific hostname or wildcard (`*`)
			* A value of "localhost" matches TCP and socket connections
			* A Wildcard value MUST be quoted to be considered valid YAML
			* E.g. "hostname: localhost"
		* `port: <port>`
			* Either a specific port or wildcard (`*`)
			* A Wildcard value MUST be quoted to be considered valid YAML
			* E.g. "port: "  "*"  "
		* `database: <database>`
			* Either a specific database or wildcard (`*`)
			* A Wildcard value MUST be quoted to be considered valid YAML  
		* `username: <username>`
			* Either a specific username or wildcard (`*`)
			* A Wildcard value MUST be quoted to be considered valid YAML
		* `password: <password>`
			* The password to use for this combination of `hostname`, `port`, `database`, `username` and `password`  


## Developing

### Committing changes

The [Git flow](https://github.com/fzaninotto/Faker#formatters) workflow is used to manage development of this package.

Discrete changes should be made within *feature* branches, created from and merged back into *develop* (where small one-line changes may be made directly).

When ready to release a set of features/changes create a *release* branch from *develop*, update documentation as required and merge into *master* with a tagged, [semantic version](http://semver.org/) (e.g. `v1.2.3`).

After releases the *master* branch should be merged with *develop* to restart the process. High impact bugs can be addressed in *hotfix* branches, created from and merged into *master* directly (and then into *develop*).

### Issue tracking

Issues, bugs, improvements, questions, suggestions and other tasks related to this package are managed through the BAS Web & Applications Team Jira project ([BASWEB](https://jira.ceh.ac.uk/browse/BASWEB)).


