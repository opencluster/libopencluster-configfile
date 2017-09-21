# libopencluster-configfile
Simple library used to parse config files.

This config file parser is very simple.  It does not currently handle config groupings.
It does ignore lines that begin with `#`
It handles key/value pairs that are separated by an '='.

OpenCluster in particular uses config files for initial connection and authentication parameters.  Almost all of the more complicated settings are generally kept inside the clustered services themselves.  This means a very simple, small, local configuration is desired, and does not require more advanced and complicated config scenarios. Therefore, opencluster-configfile does not require groupings within config files, lists, etc.   In those cases, OpenCluster would typically use a simple config file to start things, and point to more complicated JSON files.

An example config file.

```
# Example config file
discovery_host=discovery.local
discovery_port=28655
```



