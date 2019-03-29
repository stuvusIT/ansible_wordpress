# WordPress

This role deploys WordPress instances including plugins and their configuration.


## Requirements

This role requires the [php-fpm role](https://github.com/stuvusIT/php-fpm), the [nginx role](https://github.com/stuvusIT/nginx) and the [maria db role](https://github.com/stuvusIT/mariadb).


## Role Variables

The role has one top-level variable `wordpress_instances`, which is a list of dicts:

### General

Each entry in `wordpress_instances` describes one WordPress instance which will be available in `/var/www/wordpress-{{instance.name}}`.
A temporary directory for each instance will be created at `/var/www/wordpress-{{instance.name}}/wp-content/tmp` and should be configured accordingly in php.
A lot of configuration keys are available, only the most important ones are described here.
The variables described in the [defaults](defaults/main.yml) are used as default if a WordPress instance does not contain the respective key (e.g. if the instance does not define `admins`, `wordpress_default_admins` is used).

**Only the variables `name`, `siteurl`, `home`, `mysql_user`, `mysql_password`, `table_prefix`, `admins`, `plugins` and related options are evaluated and used on each role execution.**
**All other variables are only used when WordPress is installed.**


| Name                   | Required/Default        | Description                                                                                                                           |
|------------------------|:-----------------------:|---------------------------------------------------------------------------------------------------------------------------------------|
| `name`                 | :heavy_check_mark:      | A name to describe the instance. This is used e.g. to name the WordPress folder, the database and the database user of this instance. |
| `siteurl`              | :heavy_check_mark:      | The basename of the domain where this WordPress instance should be reachable, e.g. `https://example.com`.                             |
| `home`                 | :heavy_check_mark:      | The homepage of the WordPress instance                                                                                                |
| `blogname`             | :heavy_check_mark:      | The blog name (title) of this WordPress instance                                                                                      |
| `blogdescription`      | :heavy_check_mark:      | The description of the WordPress instance                                                                                             |
| `default_user`         | `admin`                 | The user that is created on setup                                                                                                     |
| `default_password`     | :heavy_check_mark:      | The password that is used for the default user                                                                                        |
| `default_email`        | `webmaster@example.com` | The email address for the default user                                                                                                |
| `template`             | `twentyseventeen`       | The name of the WordPress template being in use                                                                                       |
| `stylesheet`           | `twentyseventeen`       | What stylesheet should be used for the WordPress instance                                                                             |
| `admins`               | `{}`                    | List of admins to be created. For more information see [Admins](#admins)                                                              |
| `plugins`              | `[]`                    | List of plugins to install and options to set. For more information see [Plugins](#plugins)                                           |
| `plugin_merge_default` | `True`                  | Default decision whether the existing plugin options should be merged or overwritten by the ones defined in the role variables.       |

### Database connection

| Name             | Required/Default    | Description                                        |
|------------------|:-------------------:|----------------------------------------------------|
| `database_name`  | `{{instance.name}}` | The database to create and use for this instance   |
| `table_prefix`   | `wp_`               | The database to create and use for this instance   |
| `mysql_user`     | `{{instance.name}}` | The MySQL user to create and use for this instance |
| `mysql_password` | :heavy_check_mark:  | The password for the MySQL user                    |

### Mail

| Name                             | Required/Default   | Description                                                                                               |
|----------------------------------|:------------------:|-----------------------------------------------------------------------------------------------------------|
| `mailserver_url`   | `mail.example.com`  | URI of the mail server         |
| `mailserver_login` | `login@example.com` | Login name for the mail serer |
| `mailserver_pass`  | `password`          | Password for the mail server  |
| `mailserver_port`  | `110`               | Port for the mail server             |

### Importing
| Name                      | Required/Default | Description                                                                                                                                                                          |
|---------------------------|:----------------:|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `import_template`         | `False`          | If this is true ansible will try to extract a zip file from   `files/{{inventory_hostname}}/{{instance.name}}{{ wordpress_template }}.zip ` and install it in the WordPress instance |
| `import_wp_content`       | `False`          | If this is `True`, Ansible will try to copy the wp-content directory over to the remote host from `files/{{inventory_hostname}}/{{instance.name}}/wp-content/`                       |
| `import_db_file`          | `False`          | If this is `True`, Ansible will try to import a sql file from `files/{{inventory_hostname}}/{{instance.name}}/wordpress.sql`                                                         |
| `import_replace_siteurls` | `[]`             | List of domains to replace with `{{instance.siteurl}}` in the SQL file before importing it, e.g. `http://www.example.com`.                                                           |

### Plugins

`plugins` is a list of dicts with the following keys:

| Name      | Required/Default                      | Description                                                                                                                                                |
|-----------|---------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `name`    | :heavy_check_mark:                    | The name(slug) of the plugin to be installed                                                                                                               |
| `merge`   | `{{ instance.plugin_merge_default }}` | Setting whether the options should be merged or overwritten                                                                                                |
| `options` | :heavy_check_mark:                    | Dict containing the plugin options (the key is the same key as in the WordPress database)                                                                  |
| `update`  | `True`                                | If this is `False`, this plugin will be excluded from updating. By default, all plugins are updated, even if they're not configured in the `plugins` list. |

### Admins

`admins` is a dict of admins to be created, using the key as admin name.
Example:
```yaml
wordpress_default_admins:
  admin:
    email: admin@example.com
```
If no `password` is set, a random password will be set, which can be reset via the specified email address.
Alternatively, plugins that supply the password like [authorizer](https://github.com/uhm-coe/authorizer) may be used.

| Name       | Required/Default         | Description                           |
|------------|--------------------------|---------------------------------------|
| `email`    | :heavy_check_mark:       | The email address of the admin.       |
| `name`     | :heavy_check_mark:       | The display name of the admin.        |
| `password` | :heavy_multiplication_x: | The plain text password of the admin. |


## Example

```yml
wordpress_instances:
  - name: mainsite
    blogname: Wordpress test
    blogdescription: Wordpress test description
    home: https://example.com
    siteurl: https://example.com
    mysql_password: changeme
    default_user: alice
    default_password: pleasechangemealice
    default_email: alice@example.com
    admin_email: webmaster@example.com
```

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).


## Author Information

 * [Fritz Otlinghaus(Scriptkiddi)](https://github.com/Scriptkiddi) _fritz.otlinghaus@stuvus.uni-stuttgart.de_
 * [Michel Weitbrecht(SlothOfAnarchy)](https://github.com/SlothOfAnarchy) _michel.weitbrecht@stuvus.uni-stuttgart.de_
 * [Adriaan Nieß(AdriaanNiess)](https://github.com/AdriaanNiess) _adriaan.niess@stuvus.uni-stuttgart.de_
