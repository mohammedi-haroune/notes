# DomJudge

```bash
# Stopping and removing mariadb container ...
docker stop dj-mariadb && docker rm dj-mariadb

# Stopping and removing domserver container ...
docker stop domserver && docker rm domserver

# Stoping and removing judgehost-0 container ...
docker stop judgehost-0 && docker rm judgehost-0

# Starting mariabdb container ...
docker run -d --name dj-mariadb -v data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=hED522QC@pwDR$n+ -e MYSQL_USER=domjudge -e MYSQL_PASSWORD=_D-F%X2KfKNSqCQB -e MYSQL_DATABASE=domjudge -p 13306:3306 mariadb --max-connections=1000
sleep 10

# Starting domserver container ....
docker run -d -v /sys/fs/cgroup:/sys/fs/cgroup:ro --link dj-mariadb:mariadb -e MYSQL_HOST=mariadb -e MYSQL_USER=domjudge -e MYSQL_DATABASE=domjudge -e MYSQL_PASSWORD=_D-F%X2KfKNSqCQB -e MYSQL_ROOT_PASSWORD=hED522QC@pwDR$n+ -p 80:80 --name domserver domjudge/domserver:latest
sleep 10

# Starting judgehost-0 container ...
docker run -d --privileged -v /sys/fs/cgroup:/sys/fs/cgroup:ro --link domserver:domserver -e DOMSERVER_BASEURL=http://domserver/ -e JUDGEDAEMON_PASSWORD=Bng2TV2Nv%mPdfW! --name judgehost-0 --link domserver:domserver --hostname judgedaemon-0 -e DAEMON_ID=0 domjudge/judgehost:latest
```

### Notes:

* `link` is used to link containes to each others, for example to be able to access the domserver from the judgehost.
* Even if the domejudge is down the web interface can't detect that and the status **Active: yes** is still there with a green flag \(**Status:OK**\), after some time it is replaced with the yellow flag \(**Status:Warning**\) then the red one \(**Status:Critical\)** but the status **Active:yes** is still there. if the judghost is booted up again the green flag \(**Status:OK**\) will showed up agin
* If we run another judgehost \(with another `DAEMON_ID`\), it will show up in the judgehosts web interface
* `JUDGEDAEMON_PASSWORD`\(defaults to `judgehost`\) environment variable should be the password for the `JUDGEDAEMON_USERNAME` \(defaults to `password`\) user. This should be ready before starting the domjudge
* If the db container took down the web interface will go out of service
* When the db container is rebooted up :Boom: the web interface is reactive again and the data is also persitested.



