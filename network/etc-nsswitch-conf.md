# `/etc/nsswitch.conf`

The `/etc/nsswitch.conf` file configures how the system uses various databases and name resolution mechanisms. The first field of entries in this file identifies the name of the database. The second field defines a list of resolution mechanisms in the order in which the system attempts to resolve queries on the database.

The following example hosts definition from `/etc/nsswitch.conf` indicates that the system first attempts to resolve host names and IP addresses by querying files (that is, `/etc/hosts`) and, if that fails, next by querying a DNS server, and last of all, by querying NIS+ (NIS version 3) :

hosts:      files dns nisplus