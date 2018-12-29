## Welcome to MQFellow Pages

MQFellow project shows different IBM MQ installation scenario from basic to advance.

1. [install-mq-aws-cli](https://mqfellow.io/install-mq-aws-cli) - Manual installation of IBM MQ using S3 as the binary repository. It uses AWS CLI.

2. [auto-mq-install-vpc-public-subnet](https://mqfellow.io/auto-mq-install-vpc-public-subnet) - Installing IBM MQ v9 using shell script. It requires the following as the input. [mq-install.sh](https://github.com/mqfellow/auto-mq-install-vpc-public-subnet/blob/master/mq-install.sh) [mq-delete.sh](https://github.com/mqfellow/auto-mq-install-vpc-public-subnet/blob/master/mq-delete.sh) 

3. [simple-queuemanager](https://github.com/mqfellow/mqfellow-docs/blob/master/simple-queuemanager-userdata.txt) - Simple QueueManager. Use the userdata from this link on the mq-install.sh script

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

4. [simple-queuemanager-userdata-using-mq-ami](https://mqfellow.io/simple-queuemanager-userdata-using-mq-ami) - It uses AMI that has IBM MQ binary already installed. The total startup time is 2 minutes.

5. [distributed-qmanager](https://mqfellow.io/distributed-qmanager) - Distributed Queue Manager scenario that uses Remote Queue, Sender, Receiver and XMITQ

6. [multi-instance-qmanager](https://mqfellow.io/multi-instance-qmanager) - Manual installation

7. Cluster Queues

8. [mqfellow/cli](https://mqfellow.io/cli) - MQFellow CLI using Docker 



