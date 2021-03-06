WMQ
===

Start instance: `strmqm -x <QMGR>`

Stop instance: `endmqm <QMGR>`

Show QMGR status: `dspmq -x -o all`

Create QMGR: `crtmqm -q <QMGR>` (-lc circular logs or -ll linears logs)

Delete QMGR: `dltmqm <QMGR>`

Kill MQM: `ps -ef | grep amq`

Order to kill process:

1. amqzmuc0
1. amqzxma0
1. amqfuma
1. amqzlaa0
1. amqzlsa0
1. amqzmur0
1. amqzmgr0
1. amqrmppa
1. amqrrmfa
1. amqzdmaa
1. amqpcsea

Run commands on QMGR: `runmqsc <QMGR>`

Dry Run commands on QMGR: `runmqsc -v <QMGR>`

Define Queue Local:

    > define ql(MY_QUEUE) replace +
                          descr('Test queue') +
                          put(enabled) get(enabled) +
                          defpsist(yes) share +
                          maxdepth(1000) maxmsgl(2000) +
                          npmclass(hish)

Define queue with the same parameters of an other queue: `> def ql(XXX) like(MY_QUEUE)`

Define queue with default queue parameters: `> def ql(XXX)`

Display queue:

    > dis q(XXX) descr get put
    > dis q(YYY) maxdepth curdepth
    > dis q(SYSTEM*)

Display QMGR: `> dis QMGR`

Define Queue Alias: `> def qa(AAA) targq(XXX)`

Define Queue Remote: `> def qr(BBB) rname(YYY) rqmname(QMGR2)`

Define Queue Template: `> def qmodel(ANSQ) deftype(TEMPDYN)`

Disable Put on QL: `> alter ql5XXX) put(disabled)`

Update Target Q for QA: `> alter qa(AAA) targq(YYY)`

Change Description of QMGR: `> alter qmgr descr('New description')`

Delete QL or QR:

    > del ql(XXX)
    > del qr(BBB)

Delete all message on a QL: `> clear ql(XXX)`

Stop MQM: `endmqm -c QMGR`

 - -c: controlled
 - -w: waiting
 - -i: immediate
 - -p: preemptive

Put Message: `amqsput QNAME [QMGR]`

Get Message: `amqsget QNAME [QMGR]`

Browse Message without delete: `amqsbcg QNAME [QMGR]`

Browse Message and display data only: `amqsgbr QNAME [QMGR]`

Backup QL: `rcdmqimg -m $QMGR -t qlocal $QL`

Recover QL: `rcrmqobj -m $QMGR -t qlocal $QL`

Dump logs: `dmpmqlog`

Test Chl: `> ping channel($CHL)`

Define Chl SDR: `> def chl($CHL) chltype(SDR) trptype(TCP) conname(${IP Chl RCVR}) xmitq($xmitq)`

Define Chl RCVR: `> def chl($CHL) chltype(RCVR) trptype(TCP)`

Start chl: `> start channel($CHL)`

Control chl: `runmqchl -c $CHL`

Define xmitq: `> def ql($QL) usage(XMITQ)`

Define Listener:

    > def listener(LISTENER.TCP) trptype(TCP) port(1414) control(QMGR) replace
    > start listener(LISTENER.TCP)

Start Listener: `runmqlsr`

Stop Listener: `endmqlsr`

Reinit Chl Sequence number (if there is issue following a QMGR recovery): `> reset channel($CHL)`

Change msg number before sync point: `> alter channel($CHL) batchsz(50)`

Display Chl Status: `> dis chstatus($CHL)`

Define QR: `> def qr($QR) rname($RQL) rqmname($RQMGR) xmitq($XMITQ)`

Display PID Process using a QL: `> dis qs($QL) type(handle) pid`

Check Commit of msg in QL and infos of Process atteched to QL: `> dis qstatus($QL) type(queue) all`


Cluster
-------

Add a cluster member: `> alter qmgr repos($CLUSTER)`

Define Cluster chl RCVR: `> def chl(TO.$QMGR) chltype(CLUSRCVR) trptype(TCP) conname($LocaleIP) cluster($CLUSTER)`

Define Cluster chl SDR: `> def chl(TO.$QMGR) chltype(CLUSSDR) trptype(TCP) conname($RQMGRIP) cluster($CLUSTER)`

Define Cluster QL: `> def ql($QL) cluster($CLUSTER)`

Verify Cluster: `> dis clusqmgr(*)`

Remove QMGR from Cluster: `> suspend qmgr`

Re-add QMGR to Cluster: `> resume qmgr`

Rebuilt Cluster obj from full repos: `> refresh cluster`

Remove QMGR from Cluster forced from Full Repos: `> reset cluster`
