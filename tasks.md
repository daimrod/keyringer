[[!meta title="Keyringer Issue Tracker"]]

## Using

Current issue tracker: [Taskwarrior](https://taskwarrior.org/) with data stored at `tasks/` folder.

    sudo apt install taskwarrior
    task rc.data.location=tasks list

## Migration from Trac

### Server side

    sudo apt install trac-xmlrpc
    trac-admin . config set components tracrpc.* enabled
    trac-admin . permission add authenticated XML_RPC 

## Client side

* Edit `.task/{taskrc,bugwarriorrc}` accordingly.
* Import tickets:

    BUGWARRIORRC=.task/bugwarriorrc bugwarrior-pull

### References

* https://bugwarrior.readthedocs.io/en/latest/common_configuration.html#envvar-BUGWARRIORRC
* https://bugwarrior.readthedocs.io/en/latest/services/trac.html
* https://bugwarrior.readthedocs.io/en/latest/configuration.html#example-configuration 
* https://bugwarrior.readthedocs.io/en/latest/using.html
* https://trac.edgewall.org/wiki/TracPlugins
* https://trac-hacks.org/wiki/XmlRpcPlugin
