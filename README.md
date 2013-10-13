sslh-logwatch
=============

You want to know from where all the nice guys are connecting to the services behind sslh. 

Before you download the sslh-logwatch perl script, you have to do the following:


<pre lang="bash"><code>

#It's handy to configure rsyslog to save the sslh log to a seperate file: 

sudo vi /etc/rsyslog/sslh.conf
if $programname == 'sslh' then /var/log/sslh.log
if $programname == 'sslh' then ~

#don't forget the logrotate:

sudo vi /etc/logrotate/sslh
/var/log/sslh.log
{
        rotate 4
        weekly
        missingok
        notifempty
        compress
        delaycompress
}

#tell logwatch where your sslh.log files are 
sudo vi /etc/logwatch/conf/logfiles/sslh.conf
LogFile = sslh.log
LogFile = sslh.log.0
Archive = sslh.log.*.gz

*ApplyStdDate = 

#logwatch service def
Title = "sslh connections"
LogFile = sslh

#and now place the perl sslh script to:
/etc/logwatch/scripts/services/sslh


#if everything is fine you can test it with:
logwatch --service sslh --output stdout --range today

#it should look like this: 

 --------------------- sslh connections Begin ------------------------ 

 Connections from:
  1.2.3.4 https: 2 times
  1.2.3.5 ssh: 1 time

 ---------------------- sslh connections End ------------------------- 
 
 ^..^
</code></pre>
