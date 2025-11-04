# OpenLDAP Docker Image for testing

This Docker image provides an OpenLDAP Server for testing LDAP applications, i.e. unit tests. The server is initialized with the example domain `example.com` with data from mammals and birds.

This work uses code from https://github.com/rroemhild/docker-test-openldap and replaces its data (i.e. the actual LDAP directory contents) with animals instead of Futurama characters.

The Flask extension [flask-ldapconn][flaskldapconn] use this image for unit tests.

[slapd]: https://github.com/nickstenning/docker-slapd
[openldap]: https://github.com/osixia/docker-openldap
[flaskldapconn]: https://github.com/rroemhild/flask-ldapconn
[futuramawikia]: http://futurama.wikia.com
[docker-test-openldap]: https://github.com/rroemhild/docker-test-openldap


## Features

* Initialized with data from animals
* Support for LDAP over TLS (STARTTLS) using a self-signed cert, or valid certificates (LetsEncrypt, etc)
* memberOf overlay support
* MS-AD style groups support
* Supports Forced STARTTLS 
* Supports custom domain and custom directory structure


## Usage

```
podman pull ghcr.io/phess/docker-test-openldap:master
podman run --rm -p 10389:10389 -p 10636:10636 --net host ghcr.io/phess/docker-test-openldap:master
```

## Testing

```
# List all animals
ldapsearch -H ldap://localhost:10389 -x -b "ou=zoo,dc=example,dc=com" -D "cn=admin,dc=example,dc=com" -w GoodNewsEveryone "(objectClass=inetOrgPerson)"

# Request StartTLS
ldapsearch -H ldap://localhost:10389 -Z -x -b "ou=zoo,dc=example,dc=com" -D "cn=admin,dc=example,dc=com" -w GoodNewsEveryone "(objectClass=inetOrgPerson)"

# Enforce StartTLS
ldapsearch -H ldap://localhost:10389 -ZZ -x -b "ou=zoo,dc=example,dc=com" -D "cn=admin,dc=example,dc=com" -w GoodNewsEveryone "(objectClass=inetOrgPerson)"

# Enforce StartTLS with self-signed cert
LDAPTLS_REQCERT=never ldapsearch -H ldap://localhost:10389 -ZZ -x -b "ou=zoo,dc=example,dc=com" -D "cn=admin,dc=example,dc=com" -w GoodNewsEveryone "(objectClass=inetOrgPerson)"

# Enforce TLS with self-signed cert
LDAPTLS_REQCERT=never ldapsearch -H ldaps://localhost:10636 -ZZ -x -b "ou=zoo,dc=example,dc=com" -D "cn=admin,dc=example,dc=com" -w GoodNewsEveryone "(objectClass=inetOrgPerson)"
```

## Exposed ports

* 10389 (ldap)
* 10636 (ldaps)

## Exposed volumes

* /etc/ldap/slapd.d
* /etc/ldap/ssl
* /var/lib/ldap
* /run/slapd


## LDAP structure

### dc=example,dc=com

| Admin            | Secret           |
| ---------------- | ---------------- |
| cn=admin,dc=example,dc=com | GoodNewsEveryone |

### ou=zoo,dc=example,dc=com

### cn=bat,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | bat |
| sn          | creature |
| description | Non human batman |
| givenName   | Bruce |
| mail        | bat@zoo.example.com |
| ou          | animals |
| uid         | bat |
| userPassword| bat |

### cn=cat,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | cat |
| sn          | Feline |
| description | If we fitz we sitz |
| givenName   | Meow |
| mail        | cat@zoo.example.com |
| ou          | animal |
| uid         | cat |
| userPassword| cat |

### cn=chicken,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | chicken |
| sn          | McDonald |
| description | Proud mom of - where are all my babies |
| givenName   | Amelia Egghart |
| mail        | chicken@zoo.example.com |
| ou          | animal |
| uid         | chicken |
| userPassword| chicken |

### cn=cow,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | cow |
| sn          | McDonald |
| description | Feeds much of the world |
| givenName   | Cow-vin Klein |
| mail        | cow@zoo.example.com |
| ou          | animal |
| uid         | cow |
| userPassword| cow |

### cn=dog,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | dog |
| sn          | Chewbarka |
| description | Hoomans' best friend |
| givenName   | Chewbarka |
| mail        | dog@zoo.example.com |
| ou          | animal |
| uid         | dog |
| userPassword| dog |

### cn=dolphin,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | dolphin |
| sn          | Bubbles |
| description | Can hear your thoughts in ultrasound |
| givenName   | Finn Diesel |
| mail        | dolphin@zoo.example.com |
| ou          | animal |
| uid         | dolphin |
| userPassword| dolphin |

### cn=duck,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | duck |
| sn          | Quackson |
| description | Walks like one, quacks like one |
| givenName   | Duck Norris |
| mail        | duck@zoo.example.com |
| ou          | animal |
| uid         | duck |
| userPassword| duck |

### cn=eagle,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | eagle |
| sn          | Feathers |
| description | Looks serious but tells great jokes |
| givenName   | Clarence |
| mail        | eagle@zoo.example.com |
| ou          | animal |
| uid         | eagle |
| userPassword| eagle |

### cn=falcon,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | falcon |
| sn          | Maverick |
| description | Is a falcon |
| givenName   | Swoop Dogg |
| mail        | falcon@zoo.example.com |
| ou          | animal |
| uid         | falcon |
| userPassword| falcon |

### cn=flamingo,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | flamingo |
| sn          | Floyd |
| description | Majestic pink birb |
| givenName   | Flinnigan |
| mail        | flamingo@zoo.example.com |
| ou          | animal |
| uid         | flamingo |
| userPassword| flamingo |

### cn=horse,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | horse |
| sn          | Trotter |
| description | Hairy Trotter |
| givenName   | Hairy |
| mail        | horse@zoo.example.com |
| ou          | animal |
| uid         | horse |
| userPassword| horse |

### cn=hummingbird,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | hummingbird |
| sn          | Hummingway |
| description | Buzzes like an insect |
| givenName   | Ernest |
| mail        | hummingbird@zoo.example.com |
| ou          | animal |
| uid         | hummingbird |
| userPassword| hummingbird |

### cn=ostrich,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | ostrich |
| sn          | Legolas |
| description | Run for your life |
| givenName   | Leggy |
| mail        | ostrich@zoo.example.com |
| ou          | animal |
| uid         | ostrich |
| userPassword| ostrich |

### cn=owl,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | owl |
| sn          | Pacino |
| description | Not really wise, just weird |
| givenName   | Owl Pacino |
| mail        | owl@zoo.example.com |
| ou          | animal |
| uid         | owl |
| userPassword| owl |

### cn=penguin,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | penguin |
| sn          | Waddlesworth |
| description | Champagne, sir? |
| givenName   | Tux |
| mail        | penguin@zoo.example.com |
| ou          | animal |
| uid         | penguin |
| userPassword| penguin |

### cn=pidgeon,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | pidgeon |
| sn          | Poitier |
| description | Flying pestilence |
| givenName   | Pigeon Poitier |
| mail        | pidgeon@zoo.example.com |
| ou          | animal |
| uid         | pidgeon |
| userPassword| pidgeon |

### cn=seagull,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | Stephen Seagull |
| sn          | Seagull |
| description | Mean flying sea critter |
| givenName   | Stephen |
| mail        | seagull@zoo.example.com |
| ou          | animal |
| uid         | seagull |
| userPassword| seagull |

### cn=seal,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | Sealvester |
| sn          | Stallone |
| description | Winner at all sports |
| givenName   | Sealvester |
| mail        | seal@zoo.example.com |
| ou          | animal |
| uid         | seal |
| userPassword| seal |

### cn=squirrel,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | Squirrely |
| sn          | Temple |
| description | Majestic rat |
| givenName   | Squirrely |
| mail        | squirrel@zoo.example.com |
| ou          | animal |
| uid         | squirrel |
| userPassword| squirrel |

### cn=whale,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectClass | inetOrgPerson |
| cn          | Moby |
| sn          | Dick |
| description | Hellooooo |
| givenName   | Moby |
| mail        | whale@zoo.example.com |
| ou          | animal |
| uid         | whale |
| userPassword| whale |


### cn=birds,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectclass| Group |
| cn         | birds |
| member     | cn=chicken,ou=zoo,dc=example,dc=com |
| member     | cn=duck,ou=zoo,dc=example,dc=com |
| member     | cn=eagle,ou=zoo,dc=example,dc=com |
| member     | cn=falcon,ou=zoo,dc=example,dc=com |
| member     | cn=flamingo,ou=zoo,dc=example,dc=com |
| member     | cn=hummingbird,ou=zoo,dc=example,dc=com |
| member     | cn=ostrich,ou=zoo,dc=example,dc=com |
| member     | cn=owl,ou=zoo,dc=example,dc=com |
| member     | cn=penguin,ou=zoo,dc=example,dc=com |
| member     | cn=pidgeon,ou=zoo,dc=example,dc=com |
| member     | cn=seagull,ou=zoo,dc=example,dc=com |

### cn=can_dive,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectclass| Group |
| cn         | can_dive |
| member     | cn=dolphin,ou=zoo,dc=example,dc=com |
| member     | cn=duck,ou=zoo,dc=example,dc=com |
| member     | cn=penguin,ou=zoo,dc=example,dc=com |
| member     | cn=seagull,ou=zoo,dc=example,dc=com |
| member     | cn=seal,ou=zoo,dc=example,dc=com |
| member     | cn=whale,ou=zoo,dc=example,dc=com |

### cn=can_fly,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectclass| Group |
| cn         | can_fly |
| member     | cn=bat,ou=zoo,dc=example,dc=com |
| member     | cn=duck,ou=zoo,dc=example,dc=com |
| member     | cn=eagle,ou=zoo,dc=example,dc=com |
| member     | cn=falcon,ou=zoo,dc=example,dc=com |
| member     | cn=hummingbird,ou=zoo,dc=example,dc=com |
| member     | cn=owl,ou=zoo,dc=example,dc=com |
| member     | cn=pidgeon,ou=zoo,dc=example,dc=com |
| member     | cn=seagull,ou=zoo,dc=example,dc=com |

### cn=can_run,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectclass| Group |
| cn         | can_run |
| member     | cn=cat,ou=zoo,dc=example,dc=com |
| member     | cn=chicken,ou=zoo,dc=example,dc=com |
| member     | cn=cow,ou=zoo,dc=example,dc=com |
| member     | cn=dog,ou=zoo,dc=example,dc=com |
| member     | cn=horse,ou=zoo,dc=example,dc=com |
| member     | cn=ostrich,ou=zoo,dc=example,dc=com |
| member     | cn=squirrel,ou=zoo,dc=example,dc=com |

### cn=mammals,ou=zoo,dc=example,dc=com

| Attribute | Value  |
|-----------|--------|
| objectclass| Group |
| cn         | mammals |
| member     | cn=bat,ou=zoo,dc=example,dc=com |
| member     | cn=cat,ou=zoo,dc=example,dc=com |
| member     | cn=cow,ou=zoo,dc=example,dc=com |
| member     | cn=dog,ou=zoo,dc=example,dc=com |
| member     | cn=dolphin,ou=zoo,dc=example,dc=com |
| member     | cn=horse,ou=zoo,dc=example,dc=com |
| member     | cn=seal,ou=zoo,dc=example,dc=com |
| member     | cn=squirrel,ou=zoo,dc=example,dc=com |
| member     | cn=whale,ou=zoo,dc=example,dc=com |


## JAAS configuration

In case you want to use this OpenLDAP server for testing with a Java-based
application using JAAS and the `LdapLoginModule`, here's a working configuration
file you can use to connect.

```
other {
  com.sun.security.auth.module.LdapLoginModule REQUIRED
    userProvider="ldap://localhost:10389/ou=zoo,dc=example,dc=com"
    userFilter="(&(uid={USERNAME})(objectClass=inetOrgPerson))"
    useSSL=false
    java.naming.security.principal="cn=admin,dc=example,dc=com"
    java.naming.security.credentials="GoodNewsEveryone"
    debug=true
    ;
};
```

This config uses the admin credentials to connect to the OpenLDAP server and to
submit the search query for the user that enters their credentials. As username
the `uid` attribute of each entry is used.
