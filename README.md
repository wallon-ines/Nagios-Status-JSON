# Centreon Status JSON
Centreon Status JSON

This PHP API script reads Centreon status.dat file and return the JSON result. This API is desinged for Nagios Client unofficial Nagios status monitoring app.

# Step 1
Create directory (/usr/share/centreon/www/json) and Upload **nath_status.php** and config.dist.php to your Centreon web root folder.
###Centreon CES Server default Web Root folder Web Root Folder - Centos
**/usr/share/centreon/www/json**

# Step 2
Rename config.dist.php to config.php and edit **config.php** *You can use your favourite text editor*

vi /usr/share/centreon/www/json/config.php  

Change status.dat file's path according to your Centreon Server configuration.

**$statusFile = '/var/log/centreon-engine/status.dat';**

Configure database if use Centreon Server and set true in $isCentreon variable


# Step 3
**Download and Configure iPhone or Android Server Alarms Nagios Client**

[Nagios Client](https://play.google.com/store/apps/details?id=com.serveralarms.nagios&hl=en)

Go to settings

![Settings](https://github.com/wallon-ines/Nagios-Status-JSON/blob/master/SettingPage-A-I.png)

Update URL
###Nagios Core
**(http or https)://centreonserver_address/centreon/json**



![URL Update](https://github.com/wallon-ines/Nagios-Status-JSON/blob/master/URLUpdatePage-A-I.png)

#Step 5
###Add IOS Push Notification and Android FCM Notification

- **1** Download Script from following PHP Script File
  - https://github.com/wallon-ines/Nagios-Status-JSON/blob/master/ServerAlarmNotify.php
  
- **2** Upload File to Nagios's Plugin Folder**
```javascript
    /usr/share/centreon/cron/
```
  
- **3** Make **ServerAlarmNotify.php** file executable using following command.
```javascript
    chmod +x ServerAlarmNotify.php
```
- **4** Add two command in Configuration  >  Commands  >  Notifications
  
- **4** Edit **commands.cfg** and add following two commands. You will find your under settings. Menu -> Setting.
![Settings](https://github.com/wallon-ines/Nagios-Status-JSON/blob/master/services-command.png)
```javascript
# 'sm-host-push-notify' command definition
define command{
    command_name 	sm-host-push-notify
    command_line 	/usr/share/centreon/cron/ServerAlarmNotify.php $HOSTNAME$ YOURGROUPKEY HOST $HOSTSTATE$
}


# 'sm-service-push-notify' command definition
define command{
  	command_name 	sm-service-push-notify
	command_line  	/usr/share/centreon/cron/ServerAlarmNotify.php $HOSTNAME$ YOURGROUPKEY SERVICE $SERVICESTATE$
}
```
#
- **5** Edit Users and add **sm-service-push-notify** as service notification command and **sm-host-push-notify** as host notification command.
![Settings](https://github.com/wallon-ines/Nagios-Status-JSON/blob/master/user.png)
   -
```javascript
define contact{
        name                            generic-contact
        service_notification_period     24x7
        host_notification_period        24x7
        service_notification_options    c,r
        host_notification_options       d,r
        service_notification_commands   notify-service-by-email,sm-service-push-notify
        host_notification_commands      notify-host-by-email,sm-host-push-notify
        register                        0       					
        }
```
#
- **6** Nagios Client Generates **GROUP API KEY** using Centreon URL
  - All devices using same URL will get Notification simultaneously.  
  - Every Android/IOS user has option to Turn off Notification for his device only.
  
- **7** If your GROUP API KEY is not showing
  - Update URL 
  - Turn OFF and ON Notification.*
  
###TODO

- Add ldap auth
