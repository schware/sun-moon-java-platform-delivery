# sun-moon-java-platform-delivery

Delivery assignment/tracking — part of the `sun-moon-java-platform`
family (alongside [Order](https://github.com/schware/sun-moon-java-platform-order)
and [KDS](https://github.com/schware/sun-moon-java-platform-kds), tied
together by the [umbrella repo](https://github.com/schware/sun-moon-java-platform)
via git submodules).

**Docker-deployed**, one container per service. Replaces
[sun-moon-java-platform-delivery-jetty](https://github.com/schware/sun-moon-java-platform-delivery-jetty)
(archived, WAR + shared external Jetty).

## Why Postgres + JSONB, not MongoDB

Originally planned as MongoDB; switched because MongoDB 5.0+ requires AVX
and this homelab CPU doesn't have it. See the umbrella repo's
`docs/adr/0001`.

## API

- `POST /deliveries` — assign a delivery (`{"orderId": "..."}`) in
  `ASSIGNED` status.

## Build & run

Requires JDK 21+, and PostgreSQL reachable at `localhost:5432` with a
`delivery_service` database and `sunmoon` role.

```
./gradlew test
./gradlew bootJar
java -jar build/libs/sun-moon-java-platform-delivery-0.1.0.jar
```

## Docker

```
docker build -t sun-moon-delivery .
docker run --network host sun-moon-delivery
```

See [Debian-Setting `docs/docker.md`](https://github.com/schware/Debian-Setting/blob/master/docs/docker.md)
for how this actually runs (docker-compose, alongside Order/KDS).

Listens on port **8082** (see `application.yml`).
