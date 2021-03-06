<img src="https://user-images.githubusercontent.com/20154956/67162203-297e5300-f362-11e9-899b-4644d3084a02.png" width="200" height="160">

**fancy** let's you fanout `rsyslog` to [Loki](https://github.com/grafana/loki) and is meant to be executed by `rsyslog` under
[omprog](http://www.rsyslog.com/doc/master/configuration/modules/omprog.html)


## Setup

1. Download **fancy** from [releases](https://github.com/negbie/fancy/releases) or compile it from sources
2. Make **fancy** executable. chmod +x fancy
3. Move **fancy** to /opt. mv fancy /opt/
4. Edit and paste following under /etc/rsyslog.conf. vim /etc/rsyslog.conf

```bash
    module(load="omprog")

    $template fancy,"%timegenerated:::date-rfc3339% %syslogseverity% %hostname% %programname%%msg%\n"

    action(type="omprog" name="fancy" template="fancy" output="/var/log/fancy.log" binary="/opt/fancy --loki-url http://lokihost:3100")
```
5. Make sure you have set the right Loki URL
6. Restart `rsyslog`. systemctl restart rsyslog
7. Check logs under /var/log/syslog and /var/log/fancy.log
8. Check [example](https://github.com/negbie/fancy/tree/master/example) and build a fancy dashboard! Uh fancy :)

![fancy_dashboard](https://user-images.githubusercontent.com/20154956/67338148-cab70600-f528-11e9-97c3-5782e4714193.png)