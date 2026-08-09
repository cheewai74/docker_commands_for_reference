```
docker ps
docker run --name postgres -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=password -e POSTGRES_DB=postgres -p 5432:5432 -d postgres
docker ps -a

```

Postgres:</BR>
psql -h postgres -U postgres -W</BR>


VSCODE / DBeaver:
| Field                   | Enter       |
| ----------------------- | ----------- |
| **Server name**         | `localhost` |
| **Authentication type** | `Password`  |
| **User name**           | `postgress` |
| **Password**            | `password`  |
| **Database name**       | `postgres`  |

