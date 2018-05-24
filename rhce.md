# RHCE299 笔记精简版

# SELinux

public_content_t nfs
httpd_sys_content_t httpd



## null client mail

1. postconf ‐e inet_interfaces=loopback‐only 
1. postconf ‐e mydestination=
1. postconf ‐e local_transport=error:err 
1. postconf ‐e mynetworks="127.0.0.0/8 [::1]/128"
1. postconf ‐e relayhost=[mail.group8.example.com] 
1. postconf ‐e myorigin=server.group8.example.com 
1. systemctl enable postfix
1. systemctl restart postfix


## MariaDB

1. use <database>
1. source /root/Contacts.mdb
1. grant select on Contacts.* to Mary@localhost identified by 'redhat'

## iSCSI

1. yum install -y targetcli
1. targetcli:
    1. /backstores/block create disk1 /dev/vdb1
    1. /iscsi create iqn.2014-06.com.example:serverX
    1. /iscsi/iqn.2014-06.com.example:serverX/tpg1/acls create iqn.2014-06.com.example:desktopX
    1. /iscsi/iqn.2014-06.com.example:serverX/tpg1/luns create /backstores/block/disk1
    1. /iscsi/iqn.2014-06.com.example:serverX/tpg1/portals create 172.25.X.11 3260
    1. /iscsi/iqn.2014-06.com.example:serverX/tpg1/set attribute authentication=0
    1. /iscsi/iqn.2014-06.com.example:serverX/tpg1/set attribute generate_node_acls=0
    1. ls
    1. saveconfig
    1. exit
1. systemctl restart target.service
1. firewall-cmd --permanent --add-port=3260/tcp
1. firewall-cmd --reload

## iSCSI client

1. yum install -y iscsi-initiator*
1. vi /etc/iscsi/initatorname.iscsi -> InitiatorName=<IQN>
1. systemctl enable iscsi iscsid; systemctl start iscsi iscsid
1. iscsiadm -m discovery -t st -p 172.25.X.11 发现服务
1. iscsiadm ‐m node ‐l 登录 iscsiadm -m node -T iqn.2014-06.com.example:serverX -p 172.25.X.11 -l  登录 
1. ...

## samba

package:

* server: samba samba-client
* client: samba-client cifs-utils

service:

* server: smb nmb
* client: 

others:

man 8 cifs

smb.conf

 path = /devops
        hosts allow = 172.25.0.
        writable = no
        browseable = yes
        write list = ldapuser2


//172.24.X.11/devops /mnt/dev cifs defaults,multiuser,username=silene,password=redhat,sec=ntlmssp 0 0

## nfs

* system1:/public  /mnt/nfsmount  nfs  defaults,sec=sys,sync 0 0
* system1:/protected /mnt/nfssecure nfs defaults,sec=krb5p,sync,v4.2 0 0
