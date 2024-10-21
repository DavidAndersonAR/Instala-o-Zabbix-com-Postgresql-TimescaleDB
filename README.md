# Instalação Zabbix com Postgres e TimescaleDB

Uma breve descrição sobre o que esse projeto faz e para quem ele é


## Instalação

Instale os certificados e os arquivos do Postgres

```bash
  sudo apt install curl ca-certificates

  sudo install -d /usr/share/postgresql-common/pgdg

  sudo curl -o /usr/share/postgresql-common/pgdg/apt.postgresql.org.asc --fail https://www.postgresql.org/media/keys/ACCC4CF8.asc

  sudo sh -c 'echo "deb [signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc] https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

  sudo apt update

  sudo apt -y install postgresql
```

Isntale as dependencias do Zabbix para o Banco de Dados

```bash
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest+ubuntu24.04_all.deb

dpkg -i zabbix-release_latest+ubuntu24.04_all.deb

apt update

apt install zabbix-sql-scripts zabbix-agent2

sudo -u postgres createuser --pwprompt zabbix

zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

```

Isntale as dependencias do Zabbix-Server

```bash
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest+ubuntu24.04_all.deb

dpkg -i zabbix-release_latest+ubuntu24.04_all.deb

apt update

apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-agent2

systemctl restart zabbix-server zabbix-agent apache2

systemctl enable zabbix-server zabbix-agent apache2
```

Isntalação do TimescaleDB

```bash
sudo apt-get install timescaledb-2-postgresql-15='2.15.3~ubuntu24.04' timescaledb-2-loader-postgresql-15='2.15.3~ubuntu24.04*'

sudo echo "shared_preload_libraries = 'timescaledb'" >> /etc/postgresql/15/main/postgresql.conf

sudo sed -i "s/max_connections = 20/max_connections = 50/" /etc/postgresql/15/main/postgresql.conf

sudo echo "timescaledb.license=timescale" >> /etc/postgresql/15/main/postgresql.conf

systemctl restart postgresql

sudo -u postgres timescaledb-tune --quiet --yes

sudo -u postgres psql

CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE;
```
