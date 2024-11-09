# Installing PostgreSQL
```sh
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql.service
sudo systemctl status postgresql 
```

# PostgreSQL
```sh
sudo -u postgres psql
CREATE USER masteruser WITH SUPERUSER CREATEROLE CREATEDB REPLICATION BYPASSRLS PASSWORD '123qweA!';
CREATE DATABASE tennis_club OWNER masteruser;
GRANT ALL PRIVILEGES ON DATABASE tennis_club TO masteruser;
\du
\l
\q
DROP DATABASE tennis_club;
DROP ROLE masteruser;

netstat -pln | grep 5432
psql -U masteruser -h localhost -d tennis_club

python3 manage.py dumpdata > datadump.json
python3 manage.py loaddata --exclude auth.permission --exclude contenttypes datadump.json
python3 manage.py makemigrations
python3 manage.py migrate --run-syncdb
python3 manage.py collectstatic

python3 manage.py shell
from django.contrib.contenttypes.models import ContentType
ContentType.objects.all().delete()
ContentType.objects.count()
```

# DATABASES Configration
```sh
DATABASES = {
  'default': {
  'ENGINE': 'django.db.backends.postgresql_psycopg2',
  'NAME': 'tennis_club',
  'USER' : 'masteruser',
  'PASSWORD' : '123qweA!',
  'HOST' : 'localhost',
  'PORT' : '5432',
  }
}
```