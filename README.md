OpenWrt Configuration
=====================

This role handles basic system configuration on [OpenWrt](https://www.openwrt.org/) targets.

Requirements
------------

This role requires [gekmihesg's Ansible library for OpenWrt](https://github.com/gekmihesg/ansible-openwrt) on the Ansible controller.

Role Variables
--------------

* `openwrt_config_system`  
    A dictionary of OpenWrt system configuration settings.
    Keys are configuration options, values are their values.
    Refer to [the documentation](https://openwrt.org/docs/guide-user/base-system/system_configuration) for valid values and what they do.
    Options not included in `openwrt_config_system` will not be changed.
    Optional.
* `openwrt_config_ntp`  
    A dictionary of OpenWrt NTP configuration settings.
    Keys are configuration options, values are their values.
    Refer to [the documentation](https://openwrt.org/docs/guide-user/advanced/ntp_configuration) for valid values and what they do.
    Options not included in `openwrt_config_ntp` will not be changed.
    Optional.
* `openwrt_config_uhttpd`  
    A dictionary of uhttpd configuration settings.
    Keys are uhttpd configuration section names.
    The values are dictionaries with configuration options as keys and their values as values.
    Refer to [the documentation](https://openwrt.org/docs/guide-user/services/webserver/uhttpd) for valid values and what they do.
    If not explicitly set, the `listen_http` and `listen_https` options of each section are set to the `lan` interface IPv4 address and `redirect_https` is enabled.
    Other options not included in `openwrt_config_uhttpd` will not be changed.
    To change only the `listen_http` and `listen_https` address and `redirect_https` setting of a section to the role default values, add the section as an empty dictionary (see example below).
    Optional.
    https://openwrt.org/docs/guide-user/services/webserver/uhttpd
* `openwrt_config_uhttpd_cert`  
    A dictionary of settings for self-signed X.509 certificates for uhttpd.
    Keys are configuration options, values are their values.
    Refer to [the documentation](https://openwrt.org/docs/guide-user/services/webserver/uhttpd) for valid values and what they do.
    Options not included in `openwrt_config_uhttpd_cert` will not be changed.
    Optional.
* `openwrt_config_install_packages`, `openwrt_config_remove_packages`  
    A list of package names to be installed or removed, respectively.
    Optional.
* `openwrt_config_enable_services`, `openwrt_config_disable_services`  
    A list of service names to enable and start or disable and stop, respectively.
    Optional.
* `openwrt_dst_reload_time`  
    In timezones with daylight saving time (DST), the DST change may not properly propagate to the kernel, resulting in issues with time-based firewall rules.
    This option allows reloading the in-kernel timezone after changing to or from DST.
    Set it to a time specification as understood by cron to control when timezone gets reloaded.
    The default setting works in most (but not all) timezones, but reloads the kernel timezone more often than needed.
    Set this option to `false` to disable the cron job.
    When `timezone` and `zonename` are not set or set to `UTC` in `openwrt_config_system`, the cron job is also disabled by default.
    For more information, see:
    https://forum.openwrt.org/t/parental-control-times-not-dst-aware/92120

Dependencies
------------

This role does not depend on any specific roles.

Example Configuration
---------------------

The following is a short example for some of the configuration options this role provides:

```yaml
openwrt_config_system:
  hostname: OpenWrt
  description: 'My favourite network device'
  ttylogin: true
openwrt_config_ntp:
  server:
    - 0.openwrt.pool.ntp.org
    - 1.openwrt.pool.ntp.org
    - 2.openwrt.pool.ntp.org
    - 3.openwrt.pool.ntp.org
openwrt_config_uhttpd:
  main: {}
  other:
    listen_http: []
    listen_https:
      - 192.168.0.1:8443
    home: /www/other
```

License
-------

MIT
