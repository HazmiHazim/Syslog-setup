# Setup up Syslog-ng in linux

1. Install syslog-ng
```
sudo apt-get install syslog-ng
```
2. Configuring Syslog-ng (Client Side)
    - Copy the original configuration file for back up
    ```
    sudo cp /etc/syslog-ng/syslog-ng.conf /etc/syslog-ng/syslog-ng.conf.backup
    ```

    - Paste these at the bottom of the file
    ```
    source s_network {
        syslog(ip(0.0.0.0) port(514) transport("udp"));
        syslog(ip(0.0.0.0) port(514) transport("tcp"));
    };

    destination d_logs {
        file("/var/log/centralized/$HOST/$YEAR-$MONTH-$DAY.log" create_dirs(yes));
    };

    log { source(s_network); destination(d_logs); };
    log { source(s_network); destination(d_syslog); };
    log { source(s_network); destination(d_messages); };

    ```

    - Restart Syslog-ng to apply changes
    ```
    sudo systemctl restart syslog-ng
    ```

    - Check if the syslog-ng is running (Optional)
    ```
    sudo systemctl status syslog-ng
    ```

3. Configuring Syslog-ng (Client SIde)aaaa
    - Paste these at the bottom of the page
    ```
    source s_network {
        syslog(ip(0.0.0.0) port(514) transport(udp));
        syslog(ip(0.0.0.0) port(514) transport(tcp));
    };

    destination logserver {
        network("192.168.93.129" port(514) transport(tcp));
    };

    destination d_logs {
        file("/var/log/centralized/$HOST/$YEAR-$MONTH-$DAY.log" create_dirs(yes>};

    log { source(s_network); destination(d_logs); };
    log { source(s_network); destination(logserver); };
    ```