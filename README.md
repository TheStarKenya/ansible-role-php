# Ansible Role: PHP

[![Build Status](https://img.shields.io/travis/rwanyoike/ansible-role-php.svg)](https://travis-ci.org/rwanyoike/ansible-role-php) [![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/rwanyoike/ansible-role-php/master/LICENSE)

Installs and configures PHP on RHEL/CentOS ~~or Debian/Ubuntu~~.

## Requirements

None

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
php_ini_use_template: true
# Decides whether PHP may expose the fact that it is installed on the server
php_ini_expose_php: "On"
# Maximum execution time of each script, in seconds
php_ini_max_execution_time: 30
# Maximum amount of time each script may spend parsing request data
php_ini_max_input_time: 60
# How many GET/POST/COOKIE input variables may be accepted
php_ini_max_input_vars: 1000
# Maximum amount of memory a script may consume
php_ini_memory_limit: 128M
# This directive informs PHP of which errors, warnings and notices you would
# like it to take action for
php_ini_error_reporting: E_ALL & ~E_DEPRECATED & ~E_STRICT
# This directive controls whether or not and where PHP will output errors,
# notices and warnings too
php_ini_display_errors: "Off"
# The display of errors which occur during PHP's startup sequence are handled
# separately from display_errors
php_ini_display_startup_errors: "Off"
# Maximum size of POST data that PHP will accept
php_ini_post_max_size: 8M
# Determines the size of the realpath cache to be used by PHP
php_ini_realpath_cache_size: 16K
# Maximum allowed size for uploaded files
php_ini_upload_max_filesize: 2M
# Defines the default timezone used by the date functions
php_ini_date_timezone: America/Chicago
php_ini_sendmail_path: /usr/sbin/sendmail -t -i
php_ini_other: []

php_opcache_ini_use_template: false
php_opcache_ini_zend_extension: opcache.so
# Determines if Zend OPCache is enabled
php_opcache_ini_enable: 1
# The OPcache shared memory storage size
php_opcache_ini_memory_consumption: 128
# The amount of memory for interned strings in Mbytes
php_opcache_ini_interned_strings_buffer: 8
# The maximum number of keys (scripts) in the OPcache hash table. Only numbers
# between 200 and 100000 are allowed
php_opcache_ini_max_accelerated_files: 4000
# The location of the OPcache blacklist file (wildcards allowed). Each OPcache
# blacklist file is a text file that holds the names of files that should not
# be accelerated
php_opcache_ini_blacklist_filename: /etc/php.d/opcache*.blacklist
php_opcache_ini_other: []
```

## Dependencies

None

## Example Playbook

```yaml
- hosts: servers

  vars_files:
    - vars/main.yml

  roles:
    - role: rwanyoike.php
```

Inside `vars/main.yml`:

```yaml
php_opcache_ini_other:
  # The maximum percentage of "wasted" memory until a restart is scheduled
  - max_wasted_percentage=5
  # When this directive is enabled, the OPcache appends the current working
  # directory to the script key, thus eliminating possible collisions between
  # files with the same name (basename). Disabling the directive improves
  # performance, but may break existing applications
  - use_cwd=1
  # When disabled, you must reset the OPcache manually or restart the
  # webserver for changes to the filesystem to take effect
  - validate_timestamps=1
  # How often (in seconds) to check file timestamps for changes to the shared
  # memory storage allocation. ("1" means validate once per second, but only
  # once per request. "0" means always validate)
  - revalidate_freq=2
  # Enables or disables file search in include_path optimization
  - revalidate_path=0
  # If disabled, all PHPDoc comments are dropped from the code to reduce the
  # size of the optimized code
  - save_comments=1
  # If disabled, PHPDoc comments are not loaded from SHM, so "Doc Comments"
  # may be always stored (save_comments=1), but not loaded by applications
  # that don't need them anyway
  - load_comments=1
  # If enabled, a fast shutdown sequence is used for the accelerated code
  - fast_shutdown=0
  # Allow file existence override (file_exists, etc.) performance feature
  - enable_file_override=0
  # A bitmask, where each bit enables or disables the appropriate OPcache
  # passes
  - optimization_level=0xffffffff
  - inherited_hack=1
  - dups_fix=0
  # Allows exclusion of large files from being cached. By default all files
  # are cached
  - max_file_size=0
  # Check the cache checksum each N requests. The default value of "0" means
  # that the checks are disabled
  - consistency_checks=0
  # How long to wait (in seconds) for a scheduled restart to begin if the cache
  # is not being accessed
  - force_restart_timeout=180
  # OPcache error_log file name. Empty string assumes "stderr"
  - error_log=
  # All OPcache errors go to the Web server log. By default, only fatal errors
  # (level 0) or errors (level 1) are logged. You can also enable warnings
  # (level 2), info messages (level 3) or debug messages (level 4)
  - log_verbosity_level=1
  # Preferred Shared Memory back-end. Leave empty and let the system decide
  - preferred_memory_model=
  # Protect the shared memory from unexpected writing during script execution.
  # Useful for internal debugging only
  - protect_memory=0

# ... etc ...
```

## License

MIT

## Author Information

- This role was created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
- This role was forked in 2015 by [Raymond Wanyoike](https://github.com/rwanyoike).
