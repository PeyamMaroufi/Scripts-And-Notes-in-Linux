# Linux
These are my notes on Linux kernel related stuff.

## Kernel modules
Modprobe loads module from kernel directory in `/lib/modules/$(uname -r)`. Usually files have the extension `.ko`. Find them with:

```bash
$ find /lib/modules/$(uname -r) -type f -name "*.ko"
```
You can load the modules by referring to its alias in `/lib/modules/$(uname -r)/modules.alias` or/and `modules.alias.bin`. 

To load a module `modprobe` need their dependencies listed in `/lib/modules/$(uname -r)/modules.dep` and corresponding binary version `modules.dep.bin`. If a module is present on the system but is not on the list you can run `depmod` that generate such dependencies and include your module to `modules.dep` and `modules.dep.bin`. 

If the module is loaded it will be listed in `/proc/module` or can be listed using `lsmod`
