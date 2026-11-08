INFO:     Started server process [39548]

$ ssh -i "balerica-key.pem" ec2-user@ec2-51-24-95-188.eu-west-2.compute.amazonaws.com
ssh: connect to host ec2-51-24-95-188.eu-west-2.compute.amazonaws.com port 22: Connection timed out


Specu@Benjamin UCRT64 ~/downloads (master)
$ ssh -i "balerica-key.pem" ec2-user@ec2-18-134-16-239.eu-west-2.compute.amazonaws.com
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
Last login: Sun Nov  8 11:48:36 2026 from 82.5.197.100
  ____  _____ ___ ____     _____ ___  _   _ _   _ ____    _  _____ ___ ___  _   _ ____
 / ___|| ____|_ _|  _ \   |  ___/ _ \| | | | \ | |  _ \  / \|_   _|_ _/ _ \| \ | / ___|
 \___ \|  _|  | || |_) |  | |_ | | | | | | |  \| | | | |/ _ \ | |  | | | | | |  \| \___ \
  ___) | |___ | ||  _ <   |  _|| |_| | |_| | |\  | |_| / ___ \| |  | | |_| | |\  |___) |
 |____/|_____|___|_| \_\  |_|   \___/ \___/|_| \_|____/_/   \_\_| |___\___/|_| \_|____/

CHEWBACCA OPERATIONS CENTER  |  Welcome to your first EC2!
-----------------------------------------------------------------------
Hostname      : ip-172-31-41-244.eu-west-2.compute.internal
Uptime        : up 7 minutes
CPU           : 0% busy (approx.) | 1 vCPU(s)
Load average  : 0.01 0.22 0.16 (1, 5, 15 minutes)
Memory        : 230Mi used / 957Mi total
Root disk     : 2.1G used / 8.0G total (26% full)
Instance ID   : i-09d72a9da44f05b0a
Instance type : t2.micro
AWS zone      : eu-west-2b
-----------------------------------------------------------------------
COST REALITY CHECK
  CPU at 1% does NOT mean free. Charges depend on instance type,
  running time, storage, network usage, and your AWS account benefits.
  Check AWS Billing > Free Tier and Cost Explorer. Set a budget alert.
  Stop the instance when the lab is done (EBS storage can still cost).
-----------------------------------------------------------------------
RRRAAAWWWRRR! Chewbacca says: Verify first. Panic later.
Run seir-dashboard anytime to see this again.

[ec2-user@ip-172-31-41-244 ~]$ cd /opt/balerica-bank
whoami && TZ='America/Chicago' date && ec2-metadata -i && uptime -s
ec2-user
Sun Nov  8 05:55:11 CST 2026
instance-id: i-09d72a9da44f05b0a
2026-11-08 11:47:38
[ec2-user@ip-172-31-41-244 balerica-bank]$ cat data/accounts.dat
100001Alice Johnson                 ACTIVE  000000500000USD
100002Robert Smith                  ACTIVE  000000275000USD
100003Maria Garcia                  ACTIVE  000000810000USD
[ec2-user@ip-172-31-41-244 balerica-bank]$ ./cobol/bin/account_lookup 100001
ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD
[ec2-user@ip-172-31-41-244 balerica-bank]$ echo $?
0
[ec2-user@ip-172-31-41-244 balerica-bank]$ ./cobol/bin/account_lookup 999999
ERROR=ACCOUNT NOT FOUND
ACCOUNT=999999
[ec2-user@ip-172-31-41-244 balerica-bank]$ echo $?
4
[ec2-user@ip-172-31-41-244 balerica-bank]$ ./cobol/bin/payment_post TXN-BC-0001 100001 0.00 USD TEST-ZERO
TXN=TXN-BC-0001
ACCOUNT=100001
AMOUNT=0.00
CURRENCY=USD
STATUS=REJECTED
REASON=AMOUNT MUST BE GREATER THAN ZERO
[ec2-user@ip-172-31-41-244 balerica-bank]$ echo $?
8
[ec2-user@ip-172-31-41-244 balerica-bank]$ ./cobol/bin/payment_post TXN-BC-0002 100001 999999.00 USD TEST-BIG
TXN=TXN-BC-0002
ACCOUNT=100001
AMOUNT=999999.00
CURRENCY=USD
STATUS=REJECTED
REASON=INSUFFICIENT FUNDS
[ec2-user@ip-172-31-41-244 balerica-bank]$ echo $?
8
[ec2-user@ip-172-31-41-244 balerica-bank]$ cd ~
[ec2-user@ip-172-31-41-244 ~]$ /opt/balerica-bank/cobol/bin/account_lookup 100001
ERROR=CANNOT OPEN data/accounts.dat
FILE-STATUS=35
[ec2-user@ip-172-31-41-244 ~]$ echo $?
12
[ec2-user@ip-172-31-41-244 ~]$ cd /opt/balerica-bank
[ec2-user@ip-172-31-41-244 balerica-bank]$ cut -c1-6   data/accounts.dat
cut -c7-36  data/accounts.dat
cut -c37-44 data/accounts.dat
cut -c45-56 data/accounts.dat
cut -c57-59 data/accounts.dat
100001
100002
100003
Alice Johnson
Robert Smith
Maria Garcia
ACTIVE
ACTIVE
ACTIVE
000000500000
000000275000
000000810000
USD
USD
USD
[ec2-user@ip-172-31-41-244 balerica-bank]$ ./cobol/bin/payment_post TXN-BC-0003 100001 125.50 USD INV-BC1
cat data/payments.dat
./cobol/bin/account_lookup 100001
TXN=TXN-BC-0003
ACCOUNT=100001
AMOUNT=125.50
CURRENCY=USD
STATUS=POSTED
REASON=APPROVED
TXN0001             100001000000012550USDPOSTED  INV-1001
TXN-BC-0001         100001000000000000USDREJECTEDTEST-ZERO
TXN-BC-0002         100001000099999900USDREJECTEDTEST-BIG
TXN-BC-0003         100001000000012550USDPOSTED  INV-BC1
ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD
[ec2-user@ip-172-31-41-244 balerica-bank]$ which sqlite3 && sqlite3 data/bank.db ".tables" && sqlite3 data/bank.db ".schema"
/usr/bin/sqlite3
accounts      transactions
CREATE TABLE accounts (
    account_id TEXT PRIMARY KEY,
    customer_name TEXT NOT NULL,
    status TEXT NOT NULL,
    balance REAL NOT NULL,
    currency TEXT NOT NULL
);
CREATE TABLE transactions (
    transaction_id TEXT PRIMARY KEY,
    account_id TEXT NOT NULL,
    amount REAL NOT NULL,
    currency TEXT NOT NULL,
    status TEXT NOT NULL,
    reference TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
[ec2-user@ip-172-31-41-244 balerica-bank]$ ^C
[ec2-user@ip-172-31-41-244 balerica-bank]$
