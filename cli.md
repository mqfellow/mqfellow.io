# cli

Latest CLI commands that can be used.

## Create a CSR request

This command is used to generate CSR for commercially signed certificate.

```
docker run \
-e "MF_CERTREQ_PASSWD=12345" \
-e "MF_CERTREQ_LABEL=ibmwebspheremqqmr01" \
-e "MF_DN=CN=mqfellowwest.host,O=MQFellow,OU=IT,L=Springfield,ST=Illinois,C=US" \
-e "MF_CERTREQ_SIZE=2048" \
-e "MF_CERTREQ_PREFIX=jan1" \
-e "MF_CERTREQ_S3_BUCKET=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-east-1" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-it mqfellow/cli:1.0.5 certreq

```

## Receive the commercially signed certificate

Once the certificate is received, import the certificate using the sample command below.

```
cat ../mqfellowwest_host.crt | docker run \
-e "MF_CERTREQ_PREFIX=jan1" \
-e "MF_CERTREQ_LABEL=ibmwebspheremqqmr01" \
-e "MF_CERTREQ_S3_BUCKET=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-east-1" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 certrec
```

## Create VPC with public-subnet

Easilly create a new VPC with public subnet. It uses the commericially signed certificate.

```
docker run \
-e "MF_ID=dqmgr-mq-jan1-west" \
-e "MF_KEYPAIR_NAME=mqfellow-us-west-2" \
-e "MF_AMI_ID=ami-01447eb4f920602e3" \
-e "MF_INSTANCE_TYPE=t2.large" \
-e "MF_PUBLIC_IP1=52.42.174.228" \
-e "MF_IAM_ROLE=MQFELLOW-S3FullAccess" \
-e "MF_AVAILABILITY_ZONE=us-west-2a" \
-e "MF_S3_BUCKET_NAME=mqfellow-us-east-1" \
-e "MF_USER_DATA_LOCATION=file:///mf/cli/userdata/dqmgr-qmr01-commercially-signed-cert.txt" \
-e "MF_MQ_LISTENER_PORT1=9000" \
-e "MF_CERT_PREFIX=jan1" \
-e "MF_CERT_LABEL=ibmwebspheremqqmr01" \
-e "MF_MQ_REMOTE_IP1=52.86.146.171" \
-e "MF_MQ_REMOTE_PORT1=1414" \
-e "AWS_DEFAULT_REGION=us-west-2" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 create-vpc-public-subnet

```
## Describe VPC values

Show the values that has been saved in s3 for the selected MF_ID

```

docker run \
-e "MF_ID=dqmgr-mq-jan1-west" \
-e "MF_S3_BUCKET_NAME=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-west-2" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 describe-vpc-public-subnet

```

## Delete the VPC

It will delete the following created aws resources: security group, subnet group, routing table, internet gateway and vpc.

```
docker run \
-e "MF_ID=dqmgr-mq-jan1-west" \
-e "MF_S3_BUCKET_NAME=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-west-2" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 delete-vpc-public-subnet
```

## Use case

* [Automatic creaton of Distributed Queue Manager Setup](https://mqfellow.io/distributed/distributed-auto-signed-ssl)




