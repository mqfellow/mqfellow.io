# cli
Docker MQ CLI

Reset the login/pass - git config --global --unset credential.helper

## Simple VPC with public subnet

### docker run --env-file=../env.txt -it mqfellow/cli:latest create-simple-vpc-public-subnet dec27

```
Sample env file

AWS_DEFAULT_REGION=us-east-2
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=

MF_KEYPAIR_NAME=
MF_AMI_ID=
MF_INSTANCE_TYPE=
MF_PUBLIC_IP1=
MF_IAM_ROLE=
MF_AVAILABILITY_ZONE=us-west-2a

```

### docker run --env-file=../env.txt -it mqfellow/cli:latest describe-simple-vpc-public-subnet dec27 
```

```

### docker run --env-file=../env.txt -it mqfellow/cli:latest create-simple-vpc-public-subnet dec27
```

```


