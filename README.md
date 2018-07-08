# installingSupervisorOnEc2
Install supervisor on Ec2 Instance

1. Change to superuser 
```
> sudo su 
```
2. Install supervisor from pip or easy_install tool
```
> pip install supervisor  
--or yo can try--  
> easy_install supervisor 
```
Verify where is your supervisor installation folder (normally on /usr/local/bin/ or /usr/bin/)
> which supervisord
> whereis supervisorctl


3. Create the file supervisord.conf in folder /etc/ (a sample of this file exist on this repo)
```
> sudo touch /etc/supervisord.conf  
```
you could create this sample file using 
```
/${supervisorInstallationFolder}/echo_supervisord_conf > supervisord.conf
```
Edit the file and edit the last lines in this way (uncommented and edit)
```
[include]
files = /etc/supervisor/conf.d/*.conf
```
4. In this Folder you could create one or more file*.conf for each service that supervisor will managers
```
> touch /etc/supervisor/conf.d/mywatchedService.conf
> touch /etc/supervisor/conf.d/mywatchedService2.conf
```
This is a sample content for this kind of configuration files (f.ex: mywatchedService.conf): (a file sampled exists on this repo)
```
[program:MyWatchedService]
command=/usr/local/jdk1.8.0_91/bin/java -Dspring.profiles.active=develop -jar ./myWatchedService.jar
directory=/usr/local/MyWatchedService
autostart=true
autorestart=true
startretries=3
stderr_logfile=/var/log/MyWatchedService/MyWatchedService.err.log
stdout_logfile=/var/log/MyWatchedService/MyWatchedService.out.log
user=root
#environment=SECRET_PASSPHRASE='this is secret',SECRET_TWO='another secret'
```

5. Configure supervisor as Service on the instance 
```
>touch  /etc/rc.d/init.d/supervisord  (a sample of this file exist on this repo)
Using the repo sample file ensure that line 19..21 points to the correct supervisor installation directory 
>chmod 755 /etc/rc.d/init.d/supervisord
>sudo chkconfig --add supervisord
>sudo chkconfig supervisord on
```

6. Start supervisor as a service
```
>service supervisord start
```

7. To manage supervisor managed services, open supervisorctl console
```
> /usr/local/bin/supervisorctl 
Now you enter on supervisorctl cli  and you can start/stop and restart the services
```

Keep in mind that /etc/supervisord.conf contains the supervisor program configuration
you can uncomment this lines to get the supervisor web console:
```
[inet_http_server]         ; inet (TCP) server disabled by default
port=0.0.0.0:9001        ; (ip_address:port specifier, *:port for all iface)
username=myuser              ; (default is no username (open server))
password=mypass               ; (default is no password (open server))
```
Navigate to http://{myhost}:9001  and you will ask for username and password a Basic Auth

