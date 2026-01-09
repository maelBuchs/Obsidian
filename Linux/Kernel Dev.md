
`sudo insmod module.ko` Load a module
`sudo rmmod module` Remove the module
`dmesg` Check the output

basic Makefile for .ko

```make
obj-m += hello.o

all:
        $(MAKE) -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
        $(MAKE) -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

basic includes
```
#include <linux/module.h> // Indispensable pour tout module noyau
#include <linux/kernel.h> // Pour les types et fonctions de base (ex: printk)
#include <linux/init.h>   // Pour les macros __init et __exit
```

insmod -> exec `module_init()`
rmmod -> exec `module_exit()`


metadata:
```
MODULE_LICENSE("GPL");
MODULE_DESCRIPTION("Simple Hello World Module");
MODULE_AUTHOR("Nom");
```