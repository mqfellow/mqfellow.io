## Certreq

Receives the certificae and adds it to key.db. Uploads the updated files to S3

Sample command:

```
cat ../mqfelloweast_host.crt | docker run \
-e "MF_CERTREQ_PREFIX=dec31" \
-e "MF_CERTREQ_LABEL=ibmwebspheremqqm01" \
-e "MF_CERTREQ_S3_BUCKET=mqfellow-us-east-1" \
-e AWS_DEFAULT_REGION \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-i mqfellow/cli:1.0.3 certrec

```

## Certreq

Generates csr and uploads it to S3

Sample command:

```
docker run \
-e "MF_CERTREQ_PASSWD=12345" \
-e "MF_CERTREQ_LABEL=ibmwebspheremqqm01" \
-e "MF_DN=CN=mqfelloweast.host,O=MQFellow,OU=IT,L=Springfield,ST=Illinois,C=US" \
-e "MF_CERTREQ_SIZE=2048" \
-e "MF_CERTREQ_PREFIX=dec31" \
-e "MF_CERTREQ_S3_BUCKET=mqfellow-us-east-1" \
-e AWS_DEFAULT_REGION \
-e AWS_ACCESS_KEY_ID \
-e AWS_SECRET_ACCESS_KEY \
-it mqfellow/cli:1.0.3 certreq

```



