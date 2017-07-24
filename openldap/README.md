
# openldap

### Environment variables

```sh
export INFRASTRUCTURE=<e.g. internal>
export OPENLDAP_ORGANISATION=<e.g. Example Inc.>
export OPENLDAP_ADMIN_PASSWORD=<admin_pass>
```

### LDAP commands

```sh
ldapadd -x -D "cn=admin,dc=internal" -w admin_pass -h ldap.internal <*.ldif>

# "here" documents
# add ou=users
ldapadd -x -D "cn=admin,dc=internal" -w admin_pass -h ldap.internal <<EOF
dn: ou=users,dc=internal
objectclass: organizationalUnit
objectclass: top
ou: users
EOF

# add group cn=developer under ou=users
ldapadd -x -D "cn=admin,dc=internal" -w admin_pass -h ldap.internal <<EOF
dn: cn=developer,ou=users,dc=internal
cn: developer
gidnumber: 500
objectclass: posixGroup
objectclass: top
EOF

# add user (password: user_pass) uid=user under group cn=developer
ldapadd -x -D "cn=admin,dc=internal" -w admin_pass -h ldap.internal <<EOF
dn: cn=user,cn=developer,ou=users,dc=internal
cn: user
gidnumber: 500
givenname: user
homedirectory: /home/users/user
objectclass: inetOrgPerson
objectclass: posixAccount
objectclass: top
sn: user
uid: user
uidnumber: 1000
userpassword: user_pass
EOF

ldapsearch -x -D "cn=admin,dc=internal" -w admin_pass -h ldap.internal -b "dc=internal"

ldapdelete -x -D "cn=admin,dc=internal" -w admin_pass "cn=user,cn=developer,ou=users,dc=internal"
```

see: http://www.thegeekstuff.com/2015/02/openldap-add-users-groups/
