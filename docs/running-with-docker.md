# Rough work

# Need to use docker compose
# You need - 
# API umbrella, postgres, opensearch
# 
# Talk about how to setup a bare bones version

```
docker-compose.yml

services:
  postgres:
    image: postgres:14
    environment:
      POSTGRES_USER: rootuser
      POSTGRES_PASSWORD: rootpassword
      POSTGRES_DB: postgres
    volumes:
      - postgres_data:/var/lib/postgresql/data

  api-umbrella:
    image: ghcr.io/nrel/api-umbrella:1.5.6
    depends_on:
      - postgres
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./config:/etc/api-umbrella:ro

volumes:
  postgres_data:
```

```
config/api-umbrella.yml

web:
  default_host: "0.0.0.0"
  admin:
    auth_strategies:
      enabled:
        - "local"

postgresql:
  host: postgres
  port: 5432
  database: api_umbrella
  username: web:
  default_host: "0.0.0.0"
  admin:
    auth_strategies:
      enabled:
        - "local"

postgresql:
  host: postgres
  port: 5432
  database: api_umbrella
  username: api_umbrella_app
  password: api_umbrella_app_password
  migrations:
    username: api_umbrella_owner
    password: api_umbrella_owner_password
```

1. Start only postgres `docker compose up postgres -d`
2. Start api-umbrella `docker compose up -d`
3. Attach to the api-umbrella container `docker exec -it root-api-umbrella-1 bash`
4. Now export root user creds `export DB_USERNAME=rootuser` and `export DB_PASSWORD=rootpassword`
5. run db setup `api-umbrella db-setup`
6. run db migrations `api-umbrella migrate`
7. start api umnbrella `api-umbrella start`
