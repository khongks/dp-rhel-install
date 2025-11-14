# Quickstart - Install DataPower on Red Hat Linux (10.6.0.0 and below)

I am using v10.5.0.2 for this installation.

## Pre-requisites

1. Ensure it is supported Red Hat Linux version, e.g. `Red Hat Enterprise Linux 8.10 (Ootpa)`.

1. Download DataPower RPM files, e.g. idg_lx10502.tar to the Red Hat Linux.

1. Extract the file
    ```
    tar xvf idg_lx10502.tar
    ```

    Two files are extracted
    ```
    idg_lx10502.lts.common.x86_64.rpm
    idg_lx10502.lts.nonprod.image.x86_64.rpm
    ```

The DataPower Gateway depends on the schroot package. You can obtain the schroot package. You can install the schroot package with the Extra Packages for Enterprise Linux (EPEL).

1. Install Extra Packages for Enterprise Linux (EPEL)
    ```
    ARCH=$(/bin/arch)
    subscription-manager repos --enable "codeready-builder-for-rhel-8-${ARCH}-rpms"
    dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
    dnf repolist
    ```

1. Install required packages (e.g. schroot)
    ```
    dnf install schroot libsepol json-c -y
    ```

## Installation

1. Install the RPMs. For non-root users, the non-root needs to run under `root` with sudo.
    ```
    dnf install idg_lx10502.lts.common.x86_64.rpm idg_lx10502.lts.nonprod.image.x86_64.rpm -y
    ```
   
   The output:
    ```
    Updating Subscription Management repositories.
    Red Hat Enterprise Linux 8 for x86_64 - BaseOS (RPMs)                                                                                                                                128 kB/s | 4.1 kB     00:00    
    Red Hat Enterprise Linux 8 for x86_64 - AppStream (RPMs)                                                                                                                             146 kB/s | 4.5 kB     00:00    
    Red Hat CodeReady Linux Builder for RHEL 8 x86_64 (RPMs)                                                                                                                             135 kB/s | 4.5 kB     00:00    
    Dependencies resolved.
    =====================================================================================================================================================================================================================
    Package                                                        Architecture                       Version                                        Repository                                                    Size
    =====================================================================================================================================================================================================================
    Installing:
    ibm-datapower-agoradco-nonpd-image                             x86_64                             10.5.0.2.345566-1                              @commandline                                                 3.1 G
    ibm-datapower-common                                           x86_64                             10.5.0.2.345566-1                              @commandline                                                 660 k
    Installing dependencies:
    ipvsadm                                                        x86_64                             1.31-1.el8                                     rhel-8-for-x86_64-appstream-rpms                              59 k

    Transaction Summary
    =====================================================================================================================================================================================================================
    Install  3 Packages

    Total size: 3.1 G
    Total download size: 59 k
    Installed size: 3.1 G
    Is this ok [y/N]: y
    Downloading Packages:
    ipvsadm-1.31-1.el8.x86_64.rpm                                                                                                                                                        837 kB/s |  59 kB     00:00    
    ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    Total                                                                                                                                                                                817 kB/s |  59 kB     00:00     
    Running transaction check
    Transaction check succeeded.
    Running transaction test
    Transaction test succeeded.
    Running transaction
    Preparing        :                                                                                                                                                                                             1/1 
    Installing       : ibm-datapower-agoradco-nonpd-image-10.5.0.2.345566-1.x86_64                                                                                                                                 1/3 
    Installing       : ipvsadm-1.31-1.el8.x86_64                                                                                                                                                                   2/3 
    Running scriptlet: ipvsadm-1.31-1.el8.x86_64                                                                                                                                                                   2/3 
    Running scriptlet: ibm-datapower-common-10.5.0.2.345566-1.x86_64                                                                                                                                               3/3 
    Installing       : ibm-datapower-common-10.5.0.2.345566-1.x86_64                                                                                                                                               3/3 
    Running scriptlet: ibm-datapower-common-10.5.0.2.345566-1.x86_64                                                                                                                                               3/3 
    Created symlink /etc/systemd/system/multi-user.target.wants/datapower.service → /usr/lib/systemd/system/datapower.service.

    Verifying        : ipvsadm-1.31-1.el8.x86_64                                                                                                                                                                   1/3 
    Verifying        : ibm-datapower-common-10.5.0.2.345566-1.x86_64                                                                                                                                               2/3 
    Verifying        : ibm-datapower-agoradco-nonpd-image-10.5.0.2.345566-1.x86_64                                                                                                                                 3/3 
    Installed products updated.

    Installed:
    ibm-datapower-agoradco-nonpd-image-10.5.0.2.345566-1.x86_64                           ibm-datapower-common-10.5.0.2.345566-1.x86_64                           ipvsadm-1.31-1.el8.x86_64                          

    Complete!
    ```

    The installed folder is `/opt/ibm/datapower`

## Post-installation

1. Configure the file `/opt/ibm/datapower/datapower.conf`. Run with `sudo` if you are non-root. I will "accept license" in the WebGui.

    ```
    DataPowerConfigDir=/datapower/config
    DataPowerLocalDir=/datapower/local
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
    Loaded: loaded (/usr/lib/systemd/system/datapower.service; enabled; vendor preset: disabled)
    Active: active (running) since Fri 2025-11-14 04:26:14 EST; 11s ago
    Process: 98687 ExecStopPost=/opt/ibm/datapower/datapower-control post-stop /opt/ibm/datapower/datapower.img (code=exited, status=0/SUCCESS)
    Process: 98684 ExecStop=/opt/ibm/datapower/datapower-control pre-stop /opt/ibm/datapower/datapower.img (code=exited, status=0/SUCCESS)
    Process: 99220 ExecStartPre=/bin/bash /opt/ibm/datapower/dpPreStart.sh /opt/ibm/datapower/datapower.img (code=exited, status=0/SUCCESS)
    Main PID: 99328 (datapower-contr)
        Tasks: 1 (limit: 98196)
    Memory: 51.7M
    CGroup: /system.slice/datapower.service
            └─99328 /opt/ibm/datapower/datapower-control exec-datapower /opt/ibm/datapower/datapower.img

    Nov 14 04:26:03 datapw-svr1.dev.fyre.ibm.com systemd[1]: Starting DataPower Service...
    Nov 14 04:26:03 datapw-svr1.dev.fyre.ibm.com bash[99231]: setenforce: SELinux is disabled
    Nov 14 04:26:14 datapw-svr1.dev.fyre.ibm.com bash[99220]: Fri Nov 14 2025 04:26:14 INF dpControl [pre-start][99220] Processing /tmp/ibm-dp-container-1-99220-1763112373/primary/etc/datapower.conf
    Nov 14 04:26:14 datapw-svr1.dev.fyre.ibm.com bash[99220]: Fri Nov 14 2025 04:26:14 INF dpControl [pre-start][99220]    Standby Control Not Enabled
    Nov 14 04:26:14 datapw-svr1.dev.fyre.ibm.com bash[99220]: Fri Nov 14 2025 04:26:14 INF dpControl [pre-start][99220]    License not automatically accepted
    Nov 14 04:26:14 datapw-svr1.dev.fyre.ibm.com bash[99220]: Fri Nov 14 2025 04:26:14 INF dpControl [pre-start][99220]    RaidBlockDeviceName is not specified
    Nov 14 04:26:14 datapw-svr1.dev.fyre.ibm.com systemd[1]: Started DataPower Service.
    Nov 14 04:26:15 datapw-svr1.dev.fyre.ibm.com datapower-control[99328]: Fri Nov 14 2025 04:26:15 INF dpControl [session][99328] Starting DataPower service ...
    Nov 14 04:26:15 datapw-svr1.dev.fyre.ibm.com datapower-control[99328]: Fri Nov 14 2025 04:26:15 INF dpControl [session][99328]    License not automatically accepted
    Nov 14 04:26:15 datapw-svr1.dev.fyre.ibm.com datapower-control[99328]: Fri Nov 14 2025 04:26:15 INF dpControl [mount-watch][99328] Found schoot/mount at /var/run/schroot
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

1. Access the DataPower Web UI using this URL
`https://<serverhostname>:9090` and accept license (need to wait a while after accept).