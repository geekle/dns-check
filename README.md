dns-check
=========

Hacky DNS query against a list of DNS servers.

*You need the net-dns gem installed for this to work.*

Examples
--------
1. To check the MX records for hotmail.com
```./bin/dns-check -f providers.yml -t MX -q hotmail.com```

DNS Providers File
------------------
Layout is as follows:

```<Provider's Name>:
  primary: <Provider's Primary DNS>
  secondary: <Provider's Secondary DNS>```
