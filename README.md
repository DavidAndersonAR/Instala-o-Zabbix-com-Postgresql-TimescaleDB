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
