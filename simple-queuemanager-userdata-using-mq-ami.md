```
#!/bin/bash -ex
exec > >(tee /var/log/user-data.log|logger -t user-data -s 2>/dev/console) 2>&1
uname -a
cd ~
su - mqm -c 'dspmqver'
su - mqm -c 'crtmqm -oa user -u SYSTEM.DEAD.LETTER.QUEUE QM01'
su - mqm -c 'strmqm QM01'
cat <<EOF >/tmp/CreateQs.mqsc
DIS QMGR
DIS Q(SYSTEM*)
DEF QL(QL.A) REPLACE DESCR('QL.A MQFellow')
DIS QL(QL.A)
ALTER QL(QL.A) MAXDEPTH(1000)
DIS QL(QL.A) 
DEF QL(QL.B) REPLACE DESCR('QL.B MQFellow')
DEF QL(QL.B) REPLACE MAXDEPTH(2000)
DIS QL(QL.B)
EOF
su - mqm -c 'runmqsc QM01 < /tmp/CreateQs.mqsc > /tmp/CreateQs-report.log'
cat /tmp/CreateQs-report.log
su - mqm -c 'dspmq'


```
