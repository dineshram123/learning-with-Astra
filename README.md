## Explain your use case ##

Created two tables, one as claim_by_party and another as party_by_claim to show claims made by subscribers and subscribers included in claims.

## Create your own tables on Astra ##

Example tables that we used in the workshop:

```
CREATE TABLE IF NOT EXISTS claim_by_party (
    partyid text,
    claimdate timeuuid,
    claimid text,
    party_name text,
    PRIMARY KEY ((partyid), claimid)
) WITH CLUSTERING ORDER BY (claimid DESC);

CREATE TABLE IF NOT EXISTS party_by_claim (
    claimid   text,
    claimdate timeuuid,
    partyid    text,
    claim_amt   text,
    PRIMARY KEY ((claimid), partyid)
) WITH CLUSTERING ORDER BY (partyid DESC);
```



## Insert some data ##

Here some example data that we used in the workshop:

```
INSERT INTO claim_by_party (partyid, claimdate, claimid, party_name)
VALUES ('1234-2345', NOW(), '12345-67890', 'Jack');

INSERT INTO claim_by_party (partyid, claimdate, claimid, party_name)
VALUES ('2345-1212', NOW(), '12121-12121', 'Jill');

INSERT INTO claim_by_party (partyid, claimdate, claimid, party_name)
VALUES ('3456-1111', NOW(), '23232-23232', 'Rock');

INSERT INTO claim_by_party (partyid, claimdate, claimid, party_name)
VALUES ('4567-2222', NOW(), '11111-11111', 'Peter');

INSERT INTO party_by_claim (claimid, claimdate, partyid, claim_amt)
VALUES ('12345-67890', NOW(), '1234-2345', '250');

INSERT INTO party_by_claim (claimid, claimdate, partyid, claim_amt)
VALUES ('12121-12121',NOW(),'2345-1212', '100');

INSERT INTO party_by_claim (claimid, claimdate, partyid, claim_amt)
VALUES ('23232-23232',NOW(),'3456-1111' , '199');

INSERT INTO party_by_claim (claimid, claimdate, partyid, claim_amt)
VALUES ('11111-11111',NOW(),'4567-2222', '210');



```


KVUser@cqlsh:killrvideo> SELECT * FROM claim_by_party ;

 partyid   | claimid     | claimdate                            | party_name
-----------+-------------+--------------------------------------+------------
 1234-2345 | 12345-67890 | adfb38d0-04bb-11eb-b28c-5bdb865424ac |       Jack
 4567-2222 | 11111-11111 | f09f76b0-04bb-11eb-b28c-5bdb865424ac |      Peter
 2345-1212 | 12121-12121 | f0131530-04bb-11eb-b28c-5bdb865424ac |       Jill
 3456-1111 | 23232-23232 | f017a910-04bb-11eb-b28c-5bdb865424ac |       Rock

(4 rows)

KVUser@cqlsh:killrvideo> SELECT * FROM party_by_claim ;

 claimid     | partyid   | claim_amt | claimdate
-------------+-----------+-----------+--------------------------------------
 23232-23232 | 3456-1111 |       199 | c1307300-04bd-11eb-b28c-5bdb865424ac
 11111-11111 | 4567-2222 |       210 | c1c1dd90-04bd-11eb-b28c-5bdb865424ac
 12345-67890 | 1234-2345 |       250 | c12ec550-04bd-11eb-b28c-5bdb865424ac
 12121-12121 | 2345-1212 |       100 | c12f88a0-04bd-11eb-b28c-5bdb865424ac

(4 rows)

...
...
...
```



## Experiment with CRUD and show the outputs: ##

Examples from the workshop:

```
KVUser@cqlsh:killrvideo> UPDATE party_by_claim
              ... SET claim_amt = '300'
              ... WHERE claimid ='23232-23232' and partyid ='3456-1111';
KVUser@cqlsh:killrvideo> SELECT * FROM party_by_claim ;

 claimid     | partyid   | claim_amt | claimdate
-------------+-----------+-----------+--------------------------------------
 23232-23232 | 3456-1111 |       300 | c1307300-04bd-11eb-b28c-5bdb865424ac
 11111-11111 | 4567-2222 |       210 | c1c1dd90-04bd-11eb-b28c-5bdb865424ac
 12345-67890 | 1234-2345 |       250 | c12ec550-04bd-11eb-b28c-5bdb865424ac
 12121-12121 | 2345-1212 |       100 | c12f88a0-04bd-11eb-b28c-5bdb865424ac

(4 rows)
```

```
KVUser@cqlsh:killrvideo> DELETE from party_by_claim WHERE claimid ='12345-67890' and partyid ='1234-2345';
KVUser@cqlsh:killrvideo> SELECT * FROM party_by_claim ;

 claimid     | partyid   | claim_amt | claimdate
-------------+-----------+-----------+--------------------------------------
 23232-23232 | 3456-1111 |       300 | c1307300-04bd-11eb-b28c-5bdb865424ac
 11111-11111 | 4567-2222 |       210 | c1c1dd90-04bd-11eb-b28c-5bdb865424ac
 12121-12121 | 2345-1212 |       100 | c12f88a0-04bd-11eb-b28c-5bdb865424ac

(3 rows)
```

KVUser@cqlsh:killrvideo> ALTER TABLE party_by_claim add currency text;
KVUser@cqlsh:killrvideo> DESC party_by_claim ;

CREATE TABLE killrvideo.party_by_claim (
    claimid text,
    partyid text,
    claim_amt text,
    claimdate timeuuid,
    currency text,
    PRIMARY KEY (claimid, partyid)
) WITH CLUSTERING ORDER BY (partyid DESC)
    AND additional_write_policy = 'NONE'
    AND bloom_filter_fp_chance = 0.01
    AND caching = {'keys': 'ALL', 'rows_per_partition': 'NONE'}
    AND comment = ''
    AND compaction = {'class': 'org.apache.cassandra.db.compaction.SizeTieredCompactionStrategy', 'max_threshold': '32', 'min_threshold': '4'}
    AND compression = {'enabled': 'false'}
    AND crc_check_chance = 1.0
    AND default_time_to_live = 0
    AND gc_grace_seconds = 864000
    AND max_index_interval = 2048
    AND memtable_flush_period_in_ms = 0
    AND min_index_interval = 128
    AND nodesync = {'enabled': 'true', 'incremental': 'true'}
    AND read_repair = 'BLOCKING'
    AND speculative_retry = 'NONE';

KVUser@cqlsh:killrvideo> SELECT * FROM party_by_claim ;

 claimid     | partyid   | claim_amt | claimdate                            | currency
-------------+-----------+-----------+--------------------------------------+----------
 23232-23232 | 3456-1111 |       300 | c1307300-04bd-11eb-b28c-5bdb865424ac |     null
 11111-11111 | 4567-2222 |       210 | c1c1dd90-04bd-11eb-b28c-5bdb865424ac |     null
 12121-12121 | 2345-1212 |       100 | c12f88a0-04bd-11eb-b28c-5bdb865424ac |     null

(3 rows)
KVUser@cqlsh:killrvideo>


```




