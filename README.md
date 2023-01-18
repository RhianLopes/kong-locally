# kong-locally

```shell
docker run -d --name kong-database --network=kong-net -p 5555:5432 -e "POSTGRES_USER=kong" -e "POSTGRES_DB=kong" -e "POSTGRES_PASSWORD=helloworld" postgres:9.6
```

```shell
docker run --rm --link kong-database:kong-database --network=kong-net -e "KONG_DATABASE=postgres" -e "KONG_PG_HOST=kong-database" -e "KONG_PG_PASSWORD=helloworld" kong:latest kong migrations bootstrap
```

```shell
docker run -d --name kong --network=kong-net  -e "KONG_DATABASE=postgres" -e "KONG_PG_HOST=kong-database" -e "KONG_PG_PASSWORD=helloworld" -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" -e "KONG_PROXY_ERROR_LOG=/dev/stderr" -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" -p 8000:8000 -p 8443:8443 -p 8001:8001 -p 8444:8444 kong:latest
```

```shell
docker run --rm --network=kong-net pantsel/konga -c prepare -a postgres -u postgresql://kong:helloworld@kong-database:5432/konga_db
```

```shell
docker run -d -p 1337:1337 --network=kong-net -e "DB_ADAPTER=postgres" -e "DB_PASSWORD=helloworld" -e "DB_HOST=kong-database" -e "DB_USER=kong" -e "DB_DATABASE=konga_db" -e "KONGA_HOOK_TIMEOUT=120000" -e "NODE_ENV=production" --name konga pantsel/konga
```
