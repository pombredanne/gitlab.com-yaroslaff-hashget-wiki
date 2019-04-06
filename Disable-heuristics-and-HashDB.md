By default all found heuristical plugins and all found local HashDB are enabled, also enabled remote HashDB (HashServer) https://hashdb.okerr.com/hashdb/

# Which plugins and hashdb are enabled
Run any `--pack` or `--index` command with `-v` flag:
```shell
$ hashget --index /dev/null -v       
Load hashdb kernel.org
Load hashdb _cached
Initialize remote HashDB https://hashdb.okerr.com/hashdb/
Development hashget hashdb repository
https://gitlab.com/yaroslaff/hashget
import heuristic hint from hashget.heuristics.debian
import heuristic hint from hashget.heuristics.hint
Indexing done in 0.00s. 0 local + 0 pulled + 0 new = 0 total packages
```

# Enable/Disable HashServer
With --hashserver option you can override default. --hashserver without value disables remote hashserver.

This enables default hashserver and hashserver http://example.com:
```shell
$ hashget --index /dev/null -v --hashserver https://hashdb.okerr.com/hashdb/ \
 http://example.com/  # default + example
$ hashget --index /dev/null -v --hashserver # disable all hashservers                                                     
```

# Enable/Disable HashDB
Option `--hashdb <name> <name>` enables all hashdb. Magic name 'all' enables all hashb (default).
```shell
$ hashget -v # load all hashdb
$ hashdb -v --hashdb all # load all hashdb
$ hashget -v --hashdb _cached # load only hashdb _cached
```

# Enable/Disable heuristics
```shell
$ hashget --index /dev/null -v  # enable all 
$ hashget --index /dev/null -v --heur all # enable all too
$ hashget --index /dev/null -v --heur # disable all heuristics
$ hashget --index /dev/null -v --heur debian # enable only 'debian'
$ hashget --index /dev/null -v --heur debian # enable 'debian' and 'hint'
```
