## Welcome to MQFellow Pages

### QM ini

* ADOPTMCA ADOPTNEWMCA on QM.ini
* http://www-01.ibm.com/support/docview.wss?uid=swg21080140

```
echo 'SSL:' >> $DATA_PATH/qm.ini
echo '  CDPCheckExtensions=NO' >> $DATA_PATH/qm.ini
echo '  OCSPCheckExtensions=NO' >> $DATA_PATH/qm.ini
echo '  OCSPAuthentication=OPTIONAL' >> $DATA_PATH/qm.ini

cat $DATA_PATH/qm.ini
```

```
Copy of qm.ini in AWS

Log:
   LogPrimaryFiles=6
   LogSecondaryFiles=4
   LogFilePages=4096
   LogType=CIRCULAR
   LogBufferPages=32
   LogPath=/var/mqm/log/QMGRNAME/
   LogWriteIntegrity=TripleWrite
Service:
   Name=AuthorizationService
   EntryPoints=14
ServiceComponent:
   Service=AuthorizationService
   Name=MQSeries.UNIX.auth.service
   Module=amqzfu
   ComponentDataSize=0
TCP:
   SndBuffSize=0
   RcvBuffSize=0
   RcvSndBuffSize=0
   RcvRcvBuffSize=0
   ClntSndBuffSize=0
   ClntRcvBuffSize=0
   SvrSndBuffSize=0
   SvrRcvBuffSize=0
   KeepAlive=YES <-----
SSL:
  CDPCheckExtensions=NO
  OCSPCheckExtensions=NO
  OCSPAuthentication=OPTIONAL
ApiExitLocal:
   Name=MQAuditor
   Sequence=1
   Function=EntryPoint
   Module=mqa
   Data=QMGRNAME-mqa.ini
CHANNELS: <-----
   AdoptNewMCA=all
   AdoptNewMCATimeout=5
   AdoptNewMCACheck=all
   MaxChannels=700
   MaxActiveChannels=600
```

### Maintenance 

[Maintenance](https://mqfellow.io/maintenance) - cleanup docker images, containers and github credentials

### MQFellow project shows different IBM MQ installation scenario from basic to advance.

* [install-mq-aws-cli](https://mqfellow.io/install-mq-aws-cli) - Manual installation of IBM MQ using S3 as the binary repository. It uses AWS CLI.

* [auto-mq-install-vpc-public-subnet](https://mqfellow.io/auto-mq-install-vpc-public-subnet) - Installing IBM MQ v9 using shell script. It requires the following as the input. [mq-install.sh](https://github.com/mqfellow/auto-mq-install-vpc-public-subnet/blob/master/mq-install.sh) [mq-delete.sh](https://github.com/mqfellow/auto-mq-install-vpc-public-subnet/blob/master/mq-delete.sh) 

* [simple-queuemanager](https://github.com/mqfellow/mqfellow-docs/blob/master/simple-queuemanager-userdata.txt) - Simple QueueManager. Use the userdata from this link on the mq-install.sh script

```markdown
$ sh mq-install.sh

ssh -i "mqfellow-us-west-2.pem" ec2-user@IP
sudo su -
su mqm
-bash-4.2$ ./amqsput QL.A QM01
Sample AMQSPUT0 start
target queue is QL.A
aaaaa
bbbbb
Sample AMQSPUT0 end
-bash-4.2$ ./amqsget QL.A QM01
Sample AMQSGET0 start
message <aaaaa>
message <bbbbb>
^C
-bash-4.2$ ./amqsbcg QL.A QM01
AMQSBCG0 - starts here
**********************
 MQOPEN - 'QL.A'
 No more messages 
 MQCLOSE
 MQDISC
-bash-4.2$ 

$ sh mq-delete.sh

```

* [simple-queuemanager-userdata-using-mq-ami](https://mqfellow.io/simple-queuemanager-userdata-using-mq-ami) - It uses AMI that has IBM MQ binary already installed. The total startup time is 2 minutes.

* [distributed-qmanager](https://mqfellow.io/distributed-qmanager) - Distributed Queue Manager scenario that uses Remote Queue, Sender, Receiver and XMITQ

* [multi-instance-qmanager](https://mqfellow.io/multi-instance-qmanager) - Manual installation

* [mqfellow/cli](https://mqfellow.io/cli) - MQFellow CLI using Docker 

* [Cluster](https://mqfellow.io/cluster) - MQ Cluster Setup - Full/Partial Repository

### Loopback Check Logic

```
Loopback Check

DEFINE QLOCAL('{MQ_QUEUE_MANAGER_NAME}.LOOPBACK') DISTL(NO) MAXDEPTH(5000) REPLACE
DEFINE QREMOTE('{MQ_QUEUE_MANAGER_NAME}.LOOPBACK.RMT') RQMNAME('REMOTEQM') RNAME('{MQ_QUEUE_MANAGER_NAME}.LOOPBACK.RMT') XMITQ('REMOTEQM') REPLACE

AMQ8629: Display service information details.
   SERVICE(MQM.LOOPBACKSVC)                 CONTROL(QMGR)
   SERVTYPE(SERVER)                     
   STARTCMD(/var/mqm/scripts/mq-chls.sh)
   STARTARG(&)                             STOPCMD(/usr/bin/pkill)
   STOPARG(-f -9 /var/mqm/scripts/mq-chls.sh)
   STDOUT(/var/mqm/scripts/mqboot.+QMNAME+)
   STDERR(/var/mqm/scripts/mqboot.+QMNAME+)
   DESCR(Keep VENDOR channels up and running)
   ALTDATE(2010-03-02)                     ALTTIME(09.03.49)

---------

#!/usr/bin/sh
# -----------------------------------------------------------------------------
# Keep VENDOR channels up and running
#
# -----------------------------------------------------------------------------
_PROG=${0##*/}

# Must run as mqm!
[[ $(whoami) != "mqm" ]] && printf "$_PROG: FATAL: Script must be executed as the mqm user.\n" && exit 255
printf "$_PROG: Begin processing at $(date)\n"

while true
do
  _DATE=$(date "+%Y%m%d")
  _LOG=/var/mqm/audit/$_DATE.${_PROG/.sh/}.log
  # Get the QMgr name for this host
  QMNAME=$(dspmq | tr ')' '\n' | grep 'QMNAME(' | cut -d '(' -f 2)
  PREFIX_ENV=P # VENDOR SDLC P=Prod, V=QA
  [[ ${QMNAME:0:1} != "P" ]] && PREFIX_ENV=V
  date  >>$_LOG 2>&1
  printf "Hello world from $QMNAME to ${PREFIX_ENV}H0Q at $(date)" | /opt/mqm/samp/bin/amqsput $QMNAME.LOOPBACK.${PREFIX_ENV}H0Q.RMT $QMNAME >>$_LOG 2>&1
  printf "Hello world from $QMNAME to ${PREFIX_ENV}H1Q at $(date)" | /opt/mqm/samp/bin/amqsput $QMNAME.LOOPBACK.${PREFIX_ENV}H1Q.RMT $QMNAME >>$_LOG 2>&1
  if [ "$PREFIX_ENV" = "P" ]; then
    printf "Hello world from $QMNAME to ${PREFIX_ENV}H2Q at $(date)" | /opt/mqm/samp/bin/amqsput $QMNAME.LOOPBACK.${PREFIX_ENV}H2Q.RMT $QMNAME >>$_LOG 2>&1
    printf "Hello world from $QMNAME to ${PREFIX_ENV}H3Q at $(date)" | /opt/mqm/samp/bin/amqsput $QMNAME.LOOPBACK.${PREFIX_ENV}H3Q.RMT $QMNAME >>$_LOG 2>&1
  fi
  /opt/mqm/samp/bin/amqsget $QMNAME.LOOPBACK $QMNAME  >>$_LOG 2>&1
  printf  "\n\n" >>$_LOG 2>&1
  sleep 500
done


```

