# Install docker
```
sudo apt install docker.io
```

# Install mssql17
```
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit

sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
sudo ACCEPT_EULA=Y apt-get install mssql-tools
sudo apt-get install unixodbc-dev

docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Dung741862' -p 1433:1433 -d --name mssql2017 mcr.microsoft.com/mssql/server:2017-latest
docker exec -it mssql2017 /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P Dung741862
```

# Install mysql
```
sudo docker run --name mysql-latest  -p 3306:3306 -e MYSQL_ROOT_HOST='%' -e MYSQL_ROOT_PASSWORD='1' -d --restart unless-stopped --name mysqldb mysql/mysql-server:latest
```
