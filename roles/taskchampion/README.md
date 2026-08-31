# TaskChampion
Deploy [TaskChampion Sync-Server](https://gothenburgbitfactory.org/taskchampion-sync-server/introduction.html) in a container.

## Example usage
You can embed the required vars when the role is invoked, or integrate them into your inventory variables:


```
    - role: fossgalaxy.selfhosted.taskchampion
      vars:
        taskchampion_domain: "tasks.example.com"
        taskchampion_db_host: "psql.example.com"
        taskchampion_db_pw: "DB_PASSWORD_HERE (Or vault variable)"
```

## Database Options
Like most roles in this collection, you can manage the PostgreSQL database via this role.

| setting                 |          default | description                                                      |
| ----------------------- | ---------------- | ---------------------------------------------------------------- |
| `taskchampion_db`       |             true | Should the database settings be passed to the container?         |
| `taskchampion_db_host`  |                  | The hostname of the database server                              |
| `taskchampion_db_name`  | taskchampion     | The name of the database                                         |
| `taskchampion_db_user`  | taskchampion_app | The user to connect to the database as                           |
| `taskchampion_db_pw`    |     _undefined_  | The password for the account specified as `taskchampion_db_user` |

By default, the role will attempt to login to the database server and create the database and accounts.
If you want to disable this, you can do so using `taskchampion_db_managed`.

| setting                   |            default | description                                                  |
| ------------------------- | ------------------ | ------------------------------------------------------------ |
| `taskchampion_db_managed` |               true | Should we create the database and users if they don't exist? |
| `taskchampion_db_owner`   | taskchampion_admin | The database role which should own the database              |

Note that behind the scenes, taskchampion is really using a single URI. We will build the URI from the values
provided, but ask for them in this way for consistency with our other applications (and to allow for database
management).

If you are using TLS for database connections, be aware that the database connections require a valid certificate.

## Client management

Taskchampion doesn't use a 'user account' system per-se, instead it uses a list of known-good clients. You
can provide a list of authorised clients by setting `taskchampion_clients`, by default any client will be
accepted.

To only accept the predefined clients:

```yaml
taskchampion_clients:
  - your-client-id
  - a-second-client-id
```

## TLS integration

If using an internal CA, you may need to override the container's trust store for it's TLS certs. The most
common situation is using FreeIPA certs.

Setting `inject cert` to true, will inject the FreeIPA CA cert from the host into the container using
volume mounts.

```
taskchampion_inject_cert: true
```

If your internal CA is not located at `/etc/ipa/ca.crt` you can alter `taskchampion_host_cert_location` to
point to it on the host, and it should work.

## Extras

* This role will automatically add the labels required for traefik. You will need traefik deployed to use this
* This role supports adding extra values to the container definition using `freshrss_extra` variables
