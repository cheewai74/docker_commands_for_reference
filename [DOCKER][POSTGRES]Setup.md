```
docker network create postgres-network

docker run -it -d --name postgres --network postgres-network -e POSTGRES_PASSWORD=password postgres

Enter password: password

psql -h postgres -U postgres -W

```
