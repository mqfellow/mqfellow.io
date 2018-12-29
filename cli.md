# cli
Docker MQ CLI
Reset the login/pass - git config --global --unset credential.helper

### Using cli:1.0.1


#### Distributed MQ

To use the cli 1.0.1, ensure the following:
* an AWS EIP
* AWS S3 bucket (mqfellow-us-east-1) with the following folder names: mq-certs, mq-installer, mq-output
* AWS IAM Role

```
$ docker run --env-file=../dqmanager-env-west.txt -it mqfellow/cli:1.0.1 create-vpc-public-subnet
$ docker run --env-file=../dqmanager-env-east.txt -it mqfellow/cli:1.0.1 create-vpc-public-subnet
```

#### Basic MQ

To use the cli 1.0.1, ensure the following:
* an AWS EIP
* AWS S3 bucket (mqfellow-us-east-1) with the following folder names: mq-certs, mq-installer, mq-output
* AWS IAM Role

```
cat ../basic-env-west.txt
AWS_DEFAULT_REGION=us-west-2
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
MF_KEYPAIR_NAME=
MF_AMI_ID=
MF_INSTANCE_TYPE=
MF_PUBLIC_IP1=
MF_IAM_ROLE=
MF_AVAILABILITY_ZONE=us-west-2a

$ docker run --env-file=../basic-env-west.txt -it mqfellow/cli:1.0.1 create-vpc-public-subnet
$ docker run --env-file=../basic-env-west.txt -it mqfellow/cli:1.0.1 describe-vpc-public-subnet
$ docker run --env-file=../basic-env-west.txt -it mqfellow/cli:1.0.1 delete-vpc-public-subnet

```

### Simple VPC with public subnet cli:1.0.0

To use the cli 1.0.1, create the following:
* an AWS EIP
* AWS S3 bucket (mqfellow-us-east-1) with the following folder names: mq-certs, mq-installer, mq-output
* AWS IAM Role


```

Create stack
$ docker run --env-file=../env.txt -it mqfellow/cli:1.0.0 create-simple-vpc-public-subnet dec27

cat env.txt
AWS_DEFAULT_REGION=us-west-2
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
MF_KEYPAIR_NAME=
MF_AMI_ID=
MF_INSTANCE_TYPE=
MF_PUBLIC_IP1=
MF_IAM_ROLE=
MF_AVAILABILITY_ZONE=us-west-2a

Describe stack
$ docker run --env-file=../env.txt -it mqfellow/cli:1.0.0 describe-simple-vpc-public-subnet dec27 

Delete stack
$ docker run --env-file=../env.txt -it mqfellow/cli:1.0.0 create-simple-vpc-public-subnet dec27


```


