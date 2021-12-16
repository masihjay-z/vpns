
# Requirements

 1. docker
 2.   docker-compose

## Protocols

### PPTP:
put your user list to ``.env`` . 
``PPTP_USER_LIST`` format:
```
client    server      secret      acceptable_local_IP_addresses
```
 example:
 ``PPTP_USER_LIST="user1 * pass2 *\nuser2 * pass2 *\nuser3 * pass3 *"``
 command:
 ``docker-compose up -d pptp``

### L2TP:
put your user list and psk to ``.env`` . 
 example:
 ``L2TP_IPSEC_PSK=example``
``L2TP_USER_LIST=[{"login":"user1","password":"pass1"}]``

 command:
 ``docker-compose up -d l2tp``
 
### Cisco Open Connect:
set configs to ``.env`` . 
 example:
 ```
OPENCONNECT_PORT=4443
OPENCONNECT_CA_CN=example.com
OPENCONNECT_CA_ORG=example.com
OPENCONNECT_CA_DAYS=365
OPENCONNECT_SRV_CN=example.com
OPENCONNECT_SRV_ORG=example
OPENCONNECT_SRV_DAYS=365
OPENCONNECT_NO_TEST_USER=0
 ```

 command:
 ``docker-compose up -d openconnect``
 
**NOTE:**
 - The ocserv requires the ocpasswd file to start, if `NO_TEST_USER=1` is provided, there will be no ocpasswd created, which will stop the container immediately after start it.
### User operations

All the users opertaions happened while the container is running. If you used a different container name other than  `openconnect`, then you have to change the container name accordingly.

#### Add user

If say, you want to create a user named  `tommy`, type the following command

```
docker exec -ti openconnect ocpasswd -c /etc/ocserv/ocpasswd -g "Route,All" tommy
Enter password:
Re-enter password:
```

When prompt for password, type the password twice, then you will have the user with the password you want.

> `-g "Route,ALL"`  means add user  `tommy`  to group  `Route`  and group  `All`

#### Delete user

Delete user is similar to add user, just add another argument  `-d`  to the command line

``docker exec -ti ocserv ocpasswd -c /etc/ocserv/ocpasswd -d test``

The above command will delete the default user  `test`, if you start the instance without using environment variable  `NO_TEST_USER`.


### MTPROTO:
set port and number of secrets in ``.env`` . 
 example:
 ```
 MTPROTO_PORT=4444
MTPROTO_SECRET_COUNT=1
```

 command:
 ``docker-compose up -d mtproto``
 
**NOTE:**
 - The container's log output (`docker logs mtproto`) will contain the
   links to paste into the Telegram app
  - container generate the secrets automatically using the ``MTPROTO_SECRET_COUNT `` variable to limit the number of generated secrets.
