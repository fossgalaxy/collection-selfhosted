# FreshRSS
Deploy [FreshRSS](https://freshrss.org/index.html) in a container.

## Example usage
You can embed the required vars when the role is invoked, or integrate them into your inventory variables:


```
    - role: fossgalaxy.selfhosted.freshrss
      vars:
        freshrss_db_host: "psql.example.com"
        freshrss_db_pw: "DB_PASSWORD_HERE (Or vault variable)"
        freshrss_admin_pass: "ADMIN_PASSWORD_HERE (or vault variable)"
        # openID integration
        #freshrss_oid_meta_url: ""
        #freshrss_oid_secret: ""
```

## Database Options
Like most roles in this collection, you can manage the PostgreSQL database via this role.

| setting             |      default | description                                                  |
| ------------------- | ------------ | ------------------------------------------------------------ |
| `freshrss_db`       |         true | Should the database settings be passed to the container?     |
| `freshrss_db_host`  |              | The hostname of the database server                          |
| `freshrss_db_name`  | freshrss     | The name of the database                                     |
| `freshrss_db_user`  | freshrss_app | The user to connect to the database as                       |
| `freshrss_db_pw`    | _undefined_  | The password for the account specified as `freshrss_db_user` |

By default, the role will attempt to login to the database server and create the database and accounts.
If you want to disable this, you can do so using `freshrss_db_managed`.

| setting               |        default | description                                                  |
| --------------------- | -------------- | ------------------------------------------------------------ |
| `freshrss_db_managed` |           true | Should we create the database and users if they don't exist? |
| `freshrss_db_owner`   | freshrss_admin | The database role which should own the database              |

## Default admin account

You will need to provide a password for the default 'admin' account which is created.

| setting                   |      default | description                                                     |
| ------------------------- | ------------ | --------------------------------------------------------------- |
| `freshrss_admin_user`     | admin        | The name of the database                                        |
| `freshrss_admin_pass`     | _undefined_  | The password for the account specified as `freshrss_admin_user` |
| `freshrss_admin_api_pass` | _undefined_  | The API password, by default will follow `freshrss_admin_pass`  |

You probably want to override the API password to be something different, by default it will take the value
of whatever the admin password is set to.

## OpenID integration
This service supports single sign on using OpenID connect. The role can configure this for you if desired.

| setting                  |       default | description                                                  |
| ------------------------ | ------------- | ------------------------------------------------------------ |
| `freshrss_oid_meta_url`  |               | The 'auto-discover' url for the OpenID connect provider      |
| `freshrss_oid`           | _depends (1)_ | Should we configure the OpenID integration?                  |
| `freshrss_oid_client`    | freshrss      | The OpenID connect client name                               |
| `freshrss_oid_secret`    | _undefined_   | The OpenID secret from the provider                          |

(1) If the `meta_url` is provided, we assume that you intend to configure OpenID and will set the feature
flag `freshrss_oid` to true.

Keycloak, our provider of choice allows user-provided client IDs. We default the OpenID client name to the
service name.

The 'meta url' is the `/.well-known/openid-configuration` url for your provider. If you are using keycloak
it will be something like:

https://auth.example.com/realm/realmname/.well-known/openid-configuration

If needed for your provider, the FreshRSS redirect URI will be: https://feeds.example.com/i/oidc/, assuming
you deployed FreshRSS on `feeds.example.com`

## Extras

* This role will automatically add the labels required for traefik. You will need traefik deployed to use this
* This role supports adding extra values to the container definition using `freshrss_extra` variables
