# Cluster Setup - Partial/full repository

## MQSC Commands

```
https://www.youtube.com/watch?v=YIziCeceTkw
Clustering in MQ Tutorial

ip1-qmgr-a-full
crtmqm A
strmqm A
echo 'ALTER QMGR REPOS(ALPHA)' | runmqsc A
echo 'DEFINE QL(Q1) DEFBIND(NOTFIXED) CLWLUSEQ(ANY) CLUSTER(ALPHA)' | runmqsc A
echo 'DEFINE QL(Q2) DEFBIND(NOTFIXED) CLWLUSEQ(ANY) CLUSTER(ALPHA)' | runmqsc A
echo "DEFINE CHL(TO.A) CHLTYPE(CLUSRCVR) CONNAME('LOCALHOST(3131)') CLUSTER(ALPHA)" | runmqsc A
echo "DEFINE CHL(TO.B) CHLTYPE(CLUSSDR) CONNAME('LOCALHOST(3232)') CLUSTER(ALPHA)" | runmqsc A
echo "DEFINE LSTR(A.LSTR) TRPTYPE(TCP) PORT(3131)" | runmqsc A
echo "START LISTENER(A.LSTR)" | runmqsc A
echo "DIS LSSTATUS(A.LSTR)" | runmqsc A

ip2-qmgr-b-full
crtmqm B
strmqm B
echo 'ALTER QMGR REPOS(ALPHA)' | runmqsc B
echo 'DEFINE QL(Q1) DEFBIND(NOTFIXED) CLWLUSEQ(ANY) CLUSTER(ALPHA)' | runmqsc B
echo 'DEFINE QL(Q2) DEFBIND(NOTFIXED) CLWLUSEQ(ANY) CLUSTER(ALPHA)' | runmqsc B
echo "DEFINE CHL(TO.B) CHLTYPE(CLUSRCVR) CONNAME('LOCALHOST(3232)') CLUSTER(ALPHA)" | runmqsc B
echo "DEFINE CHL(TO.A) CHLTYPE(CLUSSDR) CONNAME('LOCALHOST(3131)') CLUSTER(ALPHA)" | runmqsc B
echo "DEFINE LSTR(B.LSTR) TRPTYPE(TCP) PORT(3232)" | runmqsc B
echo "START LISTENER(B.LSTR)" | runmqsc B
echo "DIS LSSTATUS(B.LSTR)" | runmqsc B


ip3-qmgr-c-partial
# no repo modification as this partial repo
crtmqm C
strmqm C
echo 'DEFINE QL(Q1) DEFBIND(NOTFIXED) CLWLUSEQ(ANY) CLUSTER(ALPHA)' | runmqsc C
echo 'DEFINE QL(Q2) DEFBIND(NOTFIXED) CLWLUSEQ(ANY) CLUSTER(ALPHA)' | runmqsc C
echo "DEFINE CHL(TO.C) CHLTYPE(CLUSRCVR) CONNAME('LOCALHOST(3333)') CLUSTER(ALPHA)" | runmqsc C
echo "DEFINE CHL(TO.A) CHLTYPE(CLUSSDR) CONNAME('LOCALHOST(3131)') CLUSTER(ALPHA)" | runmqsc C
echo "DEFINE LSTR(C.LSTR) TRPTYPE(TCP) PORT(3333)" | runmqsc C
echo "START LISTENER(C.LSTR)" | runmqsc C
echo "DIS LSSTATUS(C.LSTR)" | runmqsc C

ip4-qmgr-d-partial
crtmqm D
strmqm D
echo 'DEFINE QL(Q1) DEFBIND(NOTFIXED) CLWLUSEQ(ANY) CLUSTER(ALPHA)' | runmqsc D
echo 'DEFINE QL(Q2) DEFBIND(NOTFIXED) CLWLUSEQ(ANY) CLUSTER(ALPHA)' | runmqsc D
echo "DEFINE CHL(TO.D) CHLTYPE(CLUSRCVR) CONNAME('LOCALHOST(3434)') CLUSTER(ALPHA)" | runmqsc D
echo "DEFINE CHL(TO.B) CHLTYPE(CLUSSDR) CONNAME('LOCALHOST(3232)') CLUSTER(ALPHA)" | runmqsc D
echo "DEFINE LSTR(D.LSTR) TRPTYPE(TCP) PORT(3434)" | runmqsc D
echo "START LISTENER(D.LSTR)" | runmqsc D
echo "DIS LSSTATUS(D.LSTR)" | runmqsc D

```

## Steps

1. create 2 full 2 partial
2. create 2 q's for all qmgrs
3. rcvr channel for all qmgrs - to retrieve message
4. the port is different from all qmgrs 3131 to 3434. generic mqsc will not work.
The mqsc for each instance has to be defined or port can be parametrized and the full/partial flag
5. define sender. from A to B, and B to A. This is for full-repo communication. from C to A and from D to B. This is for partial repo communicating to one full-repo.
6. define listener for the channels.
7. in clustering, channel will start automatically. start the listener manually.
9. submit test data - 20 - to test load balancing from different qmgrs in cluster
```
cd /opt/mqm/samp/bin
./amqsput Q1 A
1
..
20
(mq:9.0)mqm@da75c6c72bad:/opt/mqm/samp/bin$ echo "DIS QL(Q1) CURDEPTH" | runmqsc A | grep CURDEPTH
     1 : DIS QL(Q1) CURDEPTH
   CURDEPTH(5)                          
(mq:9.0)mqm@da75c6c72bad:/opt/mqm/samp/bin$ echo "DIS QL(Q1) CURDEPTH" | runmqsc B | grep CURDEPTH
     1 : DIS QL(Q1) CURDEPTH
   CURDEPTH(5)                          
(mq:9.0)mqm@da75c6c72bad:/opt/mqm/samp/bin$ echo "DIS QL(Q1) CURDEPTH" | runmqsc C | grep CURDEPTH
     1 : DIS QL(Q1) CURDEPTH
   CURDEPTH(5)                          
(mq:9.0)mqm@da75c6c72bad:/opt/mqm/samp/bin$ echo "DIS QL(Q1) CURDEPTH" | runmqsc D | grep CURDEPTH
     1 : DIS QL(Q1) CURDEPTH
   CURDEPTH(5)                          
(mq:9.0)mqm@da75c6c72bad:/opt/mqm/samp/bin$ 

(mq:9.0)mqm@da75c6c72bad:/opt/mqm/samp/bin$ ./amqsget Q1 A
Sample AMQSGET0 start
message <1>
message <5>
message <9>
message <13>
message <17>

(mq:9.0)mqm@da75c6c72bad:/opt/mqm/samp/bin$ echo "DIS QL(Q1) CURDEPTH" | runmqsc A | grep CURDEPTH
     1 : DIS QL(Q1) CURDEPTH
   CURDEPTH(0)   
```

10. Check other commands to validate.


```

echo "DISPLAY CLUSQMGR(*) QMTYPE" | runmqsc A

(mq:9.0)mqm@da75c6c72bad:/opt/mqm/samp/bin$ echo "DISPLAY CLUSQMGR(*) QMTYPE" | runmqsc A
5724-H72 (C) Copyright IBM Corp. 1994, 2016.
Starting MQSC for queue manager A.


     1 : DISPLAY CLUSQMGR(*) QMTYPE
AMQ8441: Display Cluster Queue Manager details.
   CLUSQMGR(A)                             CHANNEL(TO.A)
   CLUSTER(ALPHA)                          QMTYPE(REPOS)
AMQ8441: Display Cluster Queue Manager details.
   CLUSQMGR(B)                             CHANNEL(TO.B)
   CLUSTER(ALPHA)                          QMTYPE(REPOS)
AMQ8441: Display Cluster Queue Manager details.
   CLUSQMGR(C)                             CHANNEL(TO.C)
   CLUSTER(ALPHA)                          QMTYPE(NORMAL)
AMQ8441: Display Cluster Queue Manager details.
   CLUSQMGR(D)                             CHANNEL(TO.D)
   CLUSTER(ALPHA)                          QMTYPE(NORMAL)

echo "DISPLAY CLUSQMGR(A)" | runmqsc A

AMQ8441: Display Cluster Queue Manager details.
   CLUSQMGR(A)                             ALTDATE(2019-01-25)
   ALTTIME(09.17.13)                       BATCHHB(0)
   BATCHINT(0)                             BATCHLIM(5000) 
   BATCHSZ(50)                             CHANNEL(TO.A)
   CLUSDATE(2019-01-25)                    CLUSTER(ALPHA)
   CLUSTIME(09.17.13)                      CLWLPRTY(0)
   CLWLRANK(0)                             CLWLWGHT(50)
   COMPHDR(NONE)                           COMPMSG(NONE)
   CONNAME(LOCALHOST(3131))                CONVERT(NO)
   DEFTYPE(CLUSRCVR)                       DESCR( )
   DISCINT(6000)                           HBINT(300)
   KAINT(AUTO)                             LOCLADDR( )
   LONGRTY(999999999)                      LONGTMR(1200)
   MAXMSGL(4194304)                        MCANAME( )
   MCATYPE(THREAD)                         MCAUSER( )
   MODENAME( )                             MRDATA( )
   MREXIT( )                               MRRTY(10)
   MRTMR(1000)                             MSGDATA( )
   MSGEXIT( )                              NETPRTY(0)
   NPMSPEED(FAST)                          PASSWORD( )
   PROPCTL(COMPAT)                         PUTAUT(DEF)
   QMID(A_2019-01-25_09.17.12)             QMTYPE(REPOS)
   RCVDATA( )                              RCVEXIT( )
   SCYDATA( )                              SCYEXIT( )
   SENDDATA( )                             SENDEXIT( )
   SEQWRAP(999999999)                      SHORTRTY(10)
   SHORTTMR(60)                            SSLCAUTH(REQUIRED)
   SSLCIPH( )                              SSLPEER( )
   STATUS(RUNNING)                         SUSPEND(NO)
   TPNAME( )                               TRPTYPE(TCP)
   USEDLQ(YES)                             USERID( )
   VERSION(09000000)                       XMITQ( )

echo "DISPLAY CLUSQMGR(*) QMTYPE" | runmqsc C

(mq:9.0)mqm@da75c6c72bad:/opt/mqm/samp/bin$ echo "DISPLAY CLUSQMGR(*) QMTYPE" | runmqsc C
5724-H72 (C) Copyright IBM Corp. 1994, 2016.
Starting MQSC for queue manager C.


     1 : DISPLAY CLUSQMGR(*) QMTYPE
AMQ8441: Display Cluster Queue Manager details.
   CLUSQMGR(A)                             CHANNEL(TO.A)
   CLUSTER(ALPHA)                          QMTYPE(REPOS)
AMQ8441: Display Cluster Queue Manager details.
   CLUSQMGR(B)                             CHANNEL(TO.B)
   CLUSTER(ALPHA)                          QMTYPE(REPOS)
AMQ8441: Display Cluster Queue Manager details.
   CLUSQMGR(C)                             CHANNEL(TO.C)
   CLUSTER(ALPHA)                          QMTYPE(NORMAL)
AMQ8441: Display Cluster Queue Manager details.
   CLUSQMGR(D)                             CHANNEL(TO.D)
   CLUSTER(ALPHA)                          QMTYPE(NORMAL)
```
