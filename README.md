# README.md

## Up the stack

```bash
$ docker-compose up --build
```

## Feed the ldap

```bash
$ docker-compose exec ldap ldapadd -x -D "cn=admin,dc=example,dc=org" -w admin -H ldap://127.0.0.1:389 -f /ldap_entries/add_content.ldif
```

| **user** | **pass** | **bind dn**                            |
| :-:      | :-:      | :-:                                    |
| admin    | admin    | `cn=admin,dc=example,dc=org`           |
| john     | johnldap | `uid=john,ou=People,dc=example,dc=org` |

## Query the ldap (from host)

```bash
$ ldapsearch -x -D "cn=admin,dc=example,dc=org" -w admin -H ldap://127.0.0.1:389 -b "dc=example,dc=org" "*"
```

## Fetch ldap ip

```bash
11:07:55 ❯ docker inspect -f "{{.NetworkSettings.Networks.ldap_default.IPAddress}}" test-ldap
```

## help us

Attach an ldap container to a running Frafos ABC SBC stack

```bash
$ # run the ldap container 
$ docker run -it \
    --rm \
    --name test-ldap \
    --hostname ldap \
    -p 389:389 \
    -p 636:636 \
    --network sbc_default \
    osixia/openldap:1.2.4

$ # feed the docker
$ ldapadd -x -D "cn=admin,dc=example,dc=org" -w admin -H ldap:// -f ./ldap_entries/add_content.ldif
```
