# README.md

## Up the stack

```bash
$ docker-compose up --build
```

## Feed the ldap

```bash
$ docker-compose exec ldap ldapadd -x -D "cn=admin,dc=example,dc=org" -w admin -H ldap://127.0.0.1:389 -f /ldap_entries/add_content.ldif
```

## Query the ldap (from host)

```bash
$ ldapsearch -x -D "cn=admin,dc=example,dc=org" -w admin -H ldap://127.0.0.1:389 -b "dc=example,dc=org" "*"
```

## Fetch ldap ip

```bash
11:07:55 ❯ docker inspect -f "{{.NetworkSettings.Networks.ldap_default.IPAddress}}" test-ldapa
```
