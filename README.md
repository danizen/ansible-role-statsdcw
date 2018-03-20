# ansible-role-statsdcw

Installs statsd and the CloudWatch backend together

## Console output only

CloudWatch custom metrics are pretty expensive.  It is suggested by my leadership to put logs into CloudWatch Logs through some log-file instead.
The solution for this is a simpler install:

    yum -y install statsd

Change /etc/statsd/config.js to use the console backend:

    backends: [ "./backends/console" ],

Also change the flush interval to lower resolution (15 minutes, or 1000 * 60 * 15)

    flushInterval: 900000

Then, change the systemd files to forward the log to syslog or a special place (maybe don't use syslog, but node/npm):

    StandardOutput=/var/log/statsd.log

and provide a logrotate.d file to throw away the log after some time.

## syslog output only

You can send it to syslog with 

    StandardOutput=syslog
    SyslogIdentifier=statsd


