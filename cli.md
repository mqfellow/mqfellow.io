# cli
Docker MQ CLI

Reset the login/pass - git config --global --unset credential.helper

### Simple VPC with public subnet

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


