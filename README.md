# PostgreSQL Client (`postgresql-client`)

**Part of the BAS Ansible Role Collection (BARC)**

Installs PostgreSQL database client and configures password files for users
## Overview

* Installs PostgreSQL client package.
* Configures `.pgpass` files for *app* and *controller* users to prevent password prompting, an entry for the optional app database (provided by `postgresql-server`) is included by default through `postgresql_client_pgpass_defaults`. Other connections can be provided at run time using `postgresql_client_pgpass_user`.

## Author

[British Antarctic Survey](http://www.antarctica.ac.uk) - Web & Applications Team

Contact: [basweb@bas.ac.uk](mailto:basweb@bas.ac.uk).

## Availability

This role is designed for internal use but if useful can be shared publicly.

## License

[Open Government Licence V2](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/2/)

## Requirements

### BAS Ansible Role Collection (BARC)

* `core`

## Variables

* `postgresql_client_controller_user_username`
    * The username of the controller user, used for management tasks, if enabled
    * This variable **must** be a valid unix username
    * Default: "controller"
* `postgresql_client_controller_user_password`
    * Default password for controller user (i.e. root).
    * MUST NOT contain ":" or "\" characters to ensure compatibility with `.pgpass` files.
    * Default: "stirring-up^the=flames$381194££iz€JQ4"
* `postgresql_client_app_user_username`
    * The username of the app user, used for day to day tasks, if enabled
    * This variable **must** be a valid unix username
    * Default: "app"
* `postgresql_client_app_user_password`
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
				*  username: "controller"
				*  password: "{{ postgresql_client_controller_user_password }}"
		*  "user"
			*  lines: (array)
				*  hostname: "*"
				*  port: "*"
				*  database: "*"
				*  username: "app"
				*  password: "{{ postgresql_client_app_user_password }}"

* `postgresql_client_pgpass_user`
	* Optional additional connections to include in per user `pgpass` files.
	* Default: []  (empty array)

### `.pgpass` files

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

## Changelog

### 0.1.1 - October 2014

* Updating dependencies
* App and controller user usernames are now configurable using a variable

### 0.1.0 - October 2014

* Initial version
