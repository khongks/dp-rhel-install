# Document the filesystems and processes in DataPower for Linux

## FileSystem

```
# lsblk
NAME                                    MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
loop0                                     7:0    0   16G  0 loop  
├─loop0dp1                              253:2    0    1M  0 part  
└─loop0dp2                              253:3    0   16G  0 part  
  └─var_opt_ibm_datapower_datapower_img 253:4    0   16G  0 crypt 
loop1                                     7:1    0 48.8M  0 loop  /run/schroot/mount/var_opt_ibm_datapower_datapower_img-b75bd17e-84fb-4ccc-8975-613b43401755
vda                                     252:0    0  250G  0 disk  
├─vda1                                  252:1    0    1G  0 part  /boot
└─vda2                                  252:2    0  249G  0 part  
  ├─rhel-root                           253:0    0  233G  0 lvm   /
  └─rhel-swap                           253:1    0   16G  0 lvm   [SWAP]
vdb                                     252:16   0  100G  0 disk  
└─vdb1                                  252:17   0  100G  0 part 
```

1. `loop0 (16G)`: This is the main loop device attached to a large disk image file (datapower.img). loop0 is the virtual disk that holds the encrypted, partitioned data for your running DataPower instance.

    - 2 partitions (`loop0dp1` & `loop0dp2`). Since the loop device is treated like a disk, it can have virtual partitions.

        - `loop0dp1 (1M)` is likely a small partition for boot information or metadata.

        - `loop0dp2 (16G)` is the main data partition inside the DataPower image

    - `var_opt_ibm_datapower_datapower_img` is a crypt (encrypted) volume. The data on `loop0dp2` is encrypted, and this layer is the decrypted, usable block device that DataPower actually uses for its storage.

1. `loop1 (48.8M)` is much smaller and is currently mounted on `/run/schroot/mount/...`. It holds a specific file system or temporary configuration data. This file is a temporary configuration or a very small filesystem image required during the DataPower's startup or execution.

The key function of these loop devices is to facilitate a sandboxed, virtualized, and encrypted environment for the DataPower application:

1. Isolation: The DataPower application data is stored in a single file on the main filesystem (e.g., on /dev/vda), but it is accessed through its own separate block devices (loop0, loop0dp1, etc.), isolating it from the host OS.

1. Encryption: The crypt layer ensures that the DataPower appliance's internal data is protected even if the host machine's disk is compromised.