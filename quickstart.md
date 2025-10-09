# Quickstart - Install DataPower on Red Hat Linux

## Pre-requisites

1. Ensure it is supported Red Hat Linux version, e.g. `Red Hat Enterprise Linux 9.6 (Plow)`.

1. Download DataPower RPM files, e.g. idg_lx10640.cd.dev.tar to the Red Hat Linux.

1. Extract the file
    ```
    tar xvf idg_lx10640.cd.dev.tar
    ```

    Two files are extracted
    ```
    idg_lx10640.cd.common.x86_64.rpm
    idg_lx10640.cd.dev.image.x86_64.rpm
    ```

## Installation

1. Install the RPMs. For non-root users, the non-root needs to run under `root` with sudo.
    ```
    yum install idg_lx10640.cd.nonprod.image.x86_64.rpm idg_lx10640.cd.common.x86_64.rpm
    ```
   
   The output:
    ```
    Updating Subscription Management repositories.
    Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)                                                               76 MB/s |  81 MB     00:01    
    Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)                                                            71 MB/s |  70 MB     00:00    
    Last metadata expiration check: 0:00:01 ago on Wed 08 Oct 2025 07:37:58 PM PDT.
    Dependencies resolved.
    ===================================================================================================================================================
    Package                                       Architecture      Version                         Repository                                   Size
    ===================================================================================================================================================
    Installing:
    ibm-datapower-agoradco-nonpd-image            x86_64            10.6.4.0.375248-1               @commandline                                3.8 G
    ibm-datapower-common                          x86_64            10.6.4.0.375248-1               @commandline                                995 k
    Installing dependencies:
    boost-iostreams                               x86_64            1.75.0-10.el9                   rhel-9-for-x86_64-appstream-rpms             39 k
    boost-program-options                         x86_64            1.75.0-10.el9                   rhel-9-for-x86_64-appstream-rpms            106 k
    ipvsadm                                       x86_64            1.31-6.el9                      rhel-9-for-x86_64-appstream-rpms             54 k

    Transaction Summary
    ===================================================================================================================================================
    Install  5 Packages

    Total size: 3.8 G
    Total download size: 199 k
    Installed size: 3.8 G
    Is this ok [y/N]: y
    Downloading Packages:
    (1/3): boost-program-options-1.75.0-10.el9.x86_64.rpm                                                              549 kB/s | 106 kB     00:00    
    (2/3): boost-iostreams-1.75.0-10.el9.x86_64.rpm                                                                    137 kB/s |  39 kB     00:00    
    (3/3): ipvsadm-1.31-6.el9.x86_64.rpm                                                                               186 kB/s |  54 kB     00:00    
    ---------------------------------------------------------------------------------------------------------------------------------------------------
    Total                                                                                                              680 kB/s | 199 kB     00:00     
    Running transaction check
    Transaction check succeeded.
    Running transaction test
    Transaction test succeeded.
    Running transaction
    Preparing        :                                                                                                                           1/1 
    Installing       : ibm-datapower-agoradco-nonpd-image-10.6.4.0.375248-1.x86_64                                                               1/5 
    Installing       : boost-program-options-1.75.0-10.el9.x86_64                                                                                2/5 
    Installing       : boost-iostreams-1.75.0-10.el9.x86_64                                                                                      3/5 
    Installing       : ipvsadm-1.31-6.el9.x86_64                                                                                                 4/5 
    Running scriptlet: ipvsadm-1.31-6.el9.x86_64                                                                                                 4/5 
    Running scriptlet: ibm-datapower-common-10.6.4.0.375248-1.x86_64                                                                             5/5 
    Installing       : ibm-datapower-common-10.6.4.0.375248-1.x86_64                                                                             5/5 
    Running scriptlet: ibm-datapower-common-10.6.4.0.375248-1.x86_64                                                                             5/5 
    Created symlink /etc/systemd/system/multi-user.target.wants/datapower.service → /usr/lib/systemd/system/datapower.service.

    Verifying        : ipvsadm-1.31-6.el9.x86_64                                                                                                 1/5 
    Verifying        : boost-iostreams-1.75.0-10.el9.x86_64                                                                                      2/5 
    Verifying        : boost-program-options-1.75.0-10.el9.x86_64                                                                                3/5 
    Verifying        : ibm-datapower-agoradco-nonpd-image-10.6.4.0.375248-1.x86_64                                                               4/5 
    Verifying        : ibm-datapower-common-10.6.4.0.375248-1.x86_64                                                                             5/5 
    Installed products updated.

    Installed:
    boost-iostreams-1.75.0-10.el9.x86_64                                            boost-program-options-1.75.0-10.el9.x86_64                       
    ibm-datapower-agoradco-nonpd-image-10.6.4.0.375248-1.x86_64                     ibm-datapower-common-10.6.4.0.375248-1.x86_64                    
    ipvsadm-1.31-6.el9.x86_64                                                      

    Complete!
    ```

    The installed folder is `/opt/ibm/datapower`

## Post-installation

1. Configure the file `/opt/ibm/datapower/datapower.conf`. Run with `sudo` if you are non-root.

    ```
    DataPowerConfigDir=/datapower/config
    DataPowerLocalDir=/datapower/local
    DataPowerAcceptLicense=true
    ```

1. Create folders for config and local. Run with `sudo` if you are non-root. 
    ```
    mkdir -p /datapower/config /datapower/local
    ```

1. Start datapower using systemctl (approx 5 mins). Run with `sudo` if you are non-root. 
    ```
    systemctl start datapower
    ```

1. Check the status of datapower
    ```
    systemctl status datapower
    ```

    The output:
    ```
        ● datapower.service - DataPower Service
        Loaded: loaded (/usr/lib/systemd/system/datapower.service; enabled; preset: disabled)
        Active: active (running) since Thu 2025-10-09 16:49:35 PDT; 15s ago
        Process: 3041588 ExecStartPre=/bin/bash /opt/ibm/datapower/dpPreStart.sh /opt/ibm/datapower/datapower.img (code=exited, status=0/SUCCESS)
    Main PID: 3041703 (datapower-contr)
        Tasks: 1 (limit: 48877)
        Memory: 194.5M
            CPU: 7.569s
        CGroup: /system.slice/datapower.service
                └─3041703 /opt/ibm/datapower/datapower-control exec-datapower /opt/ibm/datapower/datapower.img

    Oct 09 16:49:18 testdp-svr1.fyre.ibm.com systemd[1]: Starting DataPower Service...
    Oct 09 16:49:18 testdp-svr1.fyre.ibm.com bash[3041599]: setenforce: SELinux is disabled
    Oct 09 16:49:35 testdp-svr1.fyre.ibm.com bash[3041588]: Thu Oct 09 2025 16:49:35 INF dpControl [pre-start][3041588] Processing /tmp/ibm-dp-contain>
    Oct 09 16:49:35 testdp-svr1.fyre.ibm.com bash[3041588]: Thu Oct 09 2025 16:49:35 INF dpControl [pre-start][3041588]    Standby Control Not Enabled
    Oct 09 16:49:35 testdp-svr1.fyre.ibm.com bash[3041588]: Thu Oct 09 2025 16:49:35 INF dpControl [pre-start][3041588]    License automatically accep>
    Oct 09 16:49:35 testdp-svr1.fyre.ibm.com bash[3041588]: Thu Oct 09 2025 16:49:35 INF dpControl [pre-start][3041588]    RaidBlockDeviceName is not >
    Oct 09 16:49:35 testdp-svr1.fyre.ibm.com systemd[1]: Started DataPower Service.
    Oct 09 16:49:36 testdp-svr1.fyre.ibm.com datapower-control[3041703]: Thu Oct 09 2025 16:49:36 INF dpControl [session][3041703] Starting DataPower >
    Oct 09 16:49:36 testdp-svr1.fyre.ibm.com datapower-control[3041703]: Thu Oct 09 2025 16:49:36 INF dpControl [session][3041703]    License automati>
    lines 1-20/20 (END)
    ```

## Configuration

1. After 5 mins, access DataPower Gateway using Telnet
    ```
    telnet 127.0.0.1 2200
    ```

    Login using `admin`. Default password is `admin`.

    You may have to install telnet
    ```
    yum install -y telnet
    ```

1. Setup Web Management within DataPower CLI terminal
    ```
    configure terminal; web-mgmt; admin-state enabled; local-address 0.0.0.0; exit; write mem;
    ```

    Type `y` to confirm write.