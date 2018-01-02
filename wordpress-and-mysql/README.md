# Wordpress and MySQL

## Solution

Start by pulling the latest images.

```bash
$ docker pull wordpress:latest
$ docker pull mysql:latest
```

Start a MySQL container.

```bash
$ docker run --name mysqlwp -e MYSQL_ROOT_PASSWORD=wordpressdocker \
                            -e MYSQL_DATABASE=wordpress \
                            -e MYSQL_USER=wordpress \
                            -e MYSQL_PASSWORD=wordpresspwd \
                            -v /home/docker/mysql:/var/lib/mysql \
                            -d mysql
```

Note the `-v /home/docker/mysql:/var/lib/mysql` line that performs bind mounting a host directory to the mount point inside the container, which makes it trivial to backup data.

To get a dump of the entire MySQL database, use the `docker exec` command to run `mysqldump` inside the container:

```bash
$ docker exec mysqlwp mysqldump --all-databases \
                                --password=wordpressdocker > wordpress.backup
```

You can now run a WordPress container linking to the MySQL container.

```bash
$ docker run --name wordpress --link mysqlwp:mysql -p 80:80 -d wordpress
```

## Reference

*Docker Cookbook*, O'Reilly Media(2016)
