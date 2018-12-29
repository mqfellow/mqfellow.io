# install-mq-aws-cli
Basic MQ Installation using AWS CLI

```
# elb https://docs.aws.amazon.com/cli/latest/reference/elb/create-load-balancer.html

aws elb create-load-balancer --load-balancer-name mqfellowa-lbqae --listeners "Protocol=TCP,LoadBalancerPort=1414,InstanceProtocol=TCP,InstancePort=1414" --subnets subnet-60290b38 subnet-f0ef8595 --security-groups sg-5aecb621 sg-0e14bb35fbe76ed0c

# lc https://docs.aws.amazon.com/cli/latest/reference/autoscaling/create-launch-configuration.html

aws autoscaling create-launch-configuration --launch-configuration-name mqfellowa-lcqae --key-name blockchain --image-id ami-011b3ccf1bd6db744 --instance-type t2.large --security-groups sg-5aecb621 sg-0e14bb35fbe76ed0c --iam-instance-profile MQFELLOW-S3FullAccess --user-data file:///Users/hoffman/workspaces/mq-stuff/userdata.txt

# asg https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-register-lbs-with-asg.html#as-lbs-app-create-lc-cli

aws autoscaling create-auto-scaling-group --auto-scaling-group-name mqfellowa-asgqae   \
--launch-configuration-name mqfellowa-lcqae \
--availability-zones "us-east-1a" "us-east-1b" \
--load-balancer-names "mqfellowa-lbqae" \
--max-size 1 --min-size 1 --desired-capacity 1

# sudo su -

# userdata

#!/bin/bash -ex
exec > >(tee /var/log/user-data.log|logger -t user-data -s 2>/dev/console) 2>&1
uname -a
cd ~
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
yum install unzip -y
unzip awscli-bundle.zip
./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
aws s3 cp s3://mqfellow-us-east-1/mq-installer/IBM_MQ_9.1_LINUX_X86-64_TRIAL.tar.gz /root/IBM_MQ_9.1_LINUX_X86-64_TRIAL.tar.gz
tar -xzvf IBM_MQ_9.1_LINUX_X86-64_TRIAL.tar.gz
cd MQServer
echo 1 | sudo ./mqlicense.sh 
rpm -ivh MQSeriesRuntime*.rpm
rpm -ivh MQSeriesJRE*.rpm
rpm -ivh MQSeriesJava*.rpm
rpm -ivh MQSeriesServer*.rpm
rpm -ivh MQSeriesWeb*.rpm
rpm -ivh MQSeriesFTBase*.rpm
rpm -ivh MQSeriesFTAgent*.rpm
rpm -ivh MQSeriesFTService*.rpm
rpm -ivh MQSeriesFTLogger*.rpm
rpm -ivh MQSeriesFTTools*.rpm
rpm -ivh MQSeriesAMQP*.rpm
rpm -ivh MQSeriesAMS*.rpm
rpm -ivh MQSeriesXRService*.rpm
rpm -ivh MQSeriesExplorer*.rpm
rpm -ivh MQSeriesGSKit*.rpm
rpm -ivh MQSeriesClient*.rpm
rpm -ivh MQSeriesMan*.rpm
rpm -ivh MQSeriesMsg*.rpm
rpm -ivh MQSeriesSamples*.rpm
rpm -ivh MQSeriesSDK*.rpm
rpm -ivh MQSeriesSFBridge*.rpm
rpm -ivh MQSeriesBCBridge*.rpm
rpm -qa | grep MQ
/opt/mqm/bin/setmqinst -i -p /opt/mqm
su - mqm -c 'dspmqver'
```
