# README.md

ldap docker stack to perfom various test with an Frafos ABC SBC stack.

## Start the container

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

## Access phpldapadmin

You can get the phpLDAPadmin IP by running :
```
$ docker inspect -f "{{.NetworkSettings.Networks.dockerldapphp_default.IPAddress}}" dockerldap_phpldapadmin_1
```

You can then access the phpLDAPadmin at https://$IP_LDAP and log with those credentials :


| **Login DN**               | **Password** |
| :-:                        | :-:          |
| cn=admin,dc=example,dc=org | admin        |

## Attach to an ABC SBC docker stack

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

# Contributuing

First of all, **thank you** for contributing :hearts:

If you find any typo/misconfiguration/... please send me a PR or open an issue. You can also ping me on twitter.

Also, while creating your Pull Request on GitHub, please write a description which gives the context and/or explains why you are creating it.
