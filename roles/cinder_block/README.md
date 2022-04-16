# Cinder Block Nodes

This role installs and configures Cinder on block nodes. 

**It changes the LVM configuration! It assumes only one LVM physical volume is availabe and restricts the scan just to this LVM PS. If there are more than one LMV PS the role will fail and the system config might be broken!**

Example filter:
```
filter = [ "a/sda1/", "r/.*/"]
```

Please check [main.yml](tasks/main.yml).