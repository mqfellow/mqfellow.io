# QM01 

Stack that can be ran on AWS us-east-1

## Certificate setup

Signed certificate can be obtained from CA provider. In this example, Comodo PositiveSSL can be bought from namecheap.com 

### Request a certificate - CSR generation

```
docker run \
-e "MF_CERTREQ_PASSWD=12345" \
-e "MF_CERTREQ_LABEL=ibmwebspheremqqm01" \
-e "MF_DN=CN=mqfelloweast.host,O=MQFellow,OU=IT,L=Springfield,ST=Illinois,C=US" \
-e "MF_CERTREQ_SIZE=2048" \
-e "MF_CERTREQ_PREFIX=jan1" \
-e "MF_CERTREQ_S3_BUCKET=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-east-1" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-it mqfellow/cli:1.0.5 certreq
```

### Receive the certificate

Once the csr is generated. It should be submitted to namecheap to receive the actual certificate to use in the server.

```
cat ../mqfelloweast_host.crt | docker run \
-e "MF_CERTREQ_PREFIX=jan1" \
-e "MF_CERTREQ_LABEL=ibmwebspheremqqm01" \
-e "MF_CERTREQ_S3_BUCKET=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-east-1" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 certrec
```

### Create the stack with vpc-public-subnet scenario

Replace the sample values accordingly.

docker run \
-e "MF_ID=dqmgr-mq-jan1-east" \
-e "MF_KEYPAIR_NAME=blockchain" \
-e "MF_AMI_ID=ami-050e97d01cc49671a" \
-e "MF_INSTANCE_TYPE=t2.large" \
-e "MF_PUBLIC_IP1=52.86.146.171" \
-e "MF_IAM_ROLE=MQFELLOW-S3FullAccess" \
-e "MF_AVAILABILITY_ZONE=us-east-1a" \
-e "MF_S3_BUCKET_NAME=mqfellow-us-east-1" \
-e "MF_USER_DATA_LOCATION=file:///mf/cli/userdata/dqmgr-qm01-commercially-signed-cert.txt" \
-e "MF_MQ_LISTENER_PORT1=1414" \
-e "MF_CERT_PREFIX=jan1" \
-e "MF_CERT_LABEL=ibmwebspheremqqm01" \
-e "MF_MQ_REMOTE_IP1=52.42.174.228" \
-e "MF_MQ_REMOTE_PORT1=9000" \
-e "AWS_DEFAULT_REGION=us-east-1" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 create-vpc-public-subnet

### Double check if the stack is created

```
docker run \
-e "MF_ID=dqmgr-mq-jan1-east" \
-e "MF_S3_BUCKET_NAME=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-east-1" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 describe-vpc-public-subnet
```

### Delete the stack created

```
docker run \
-e "MF_ID=dqmgr-mq-jan1-east" \
-e "MF_S3_BUCKET_NAME=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-east-1" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 delete-vpc-public-subnet
```

# QMR01

Stack that can be ran on AWS us-east-1

```

docker run \
-e "MF_ID=dqmgr-mq-jan1-west" \
-e "MF_S3_BUCKET_NAME=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-west-2" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 delete-vpc-public-subnet

docker run \
-e "MF_ID=dqmgr-mq-jan1-west" \
-e "MF_S3_BUCKET_NAME=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-west-2" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 describe-vpc-public-subnet

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

cat ../mqfellowwest_host.crt | docker run \
-e "MF_CERTREQ_PREFIX=jan1" \
-e "MF_CERTREQ_LABEL=ibmwebspheremqqmr01" \
-e "MF_CERTREQ_S3_BUCKET=mqfellow-us-east-1" \
-e "AWS_DEFAULT_REGION=us-east-1" \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.5 certrec

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
