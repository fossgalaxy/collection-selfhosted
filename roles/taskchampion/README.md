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
        #freshrss_oid: true
        #freshrss_oid_meta_url: ""
        #freshrss_oid_client: freshrss
        #freshrss_oid_secret: ""
```
