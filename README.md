## Explain your use case ##

Created two tables, one as claim_by_party and another as party_by_claim to show claims made by subscribers and subscribers included in claims.

## Create your own tables on Astra ##

Example tables that we used in the workshop:

```
CREATE TABLE IF NOT EXISTS claim_by_party (
    partyid uuid,
    claimdate timeuuid,
    claimid uuid,
    party_name text,
    PRIMARY KEY ((partyid), claimid)
) WITH CLUSTERING ORDER BY (claimid DESC);

CREATE TABLE IF NOT EXISTS party_by_claim (
    claimid   uuid,
    claimdate timeuuid,
    partyid    uuid,
    claim_amt   text,
    PRIMARY KEY ((claimid), partyid)
) WITH CLUSTERING ORDER BY (partyid DESC);
```



## Insert some data ##

Here some example data that we used in the workshop:

```
INSERT INTO comments_by_user (userid, commentid, videoid, comment)
VALUES (11111111-1111-1111-1111-111111111111, NOW(), 12345678-1234-1111-1111-111111111111, 'I keep watching this video');

INSERT INTO comments_by_user (userid, commentid, videoid, comment)
VALUES (11111111-1111-1111-1111-111111111111, NOW(), 12345678-1234-1111-1111-111111111111, 'Soo many comments for the same video');
```

Show off your own data inserts, into your own tables:

```
Your data goes here
```

Now show the output, for example:

```
SELECT * FROM <your table>;
...
...
...
```

Include some screenshots!

## Experiment with CRUD and show the outputs: ##

Examples from the workshop:

```
UPDATE comments_by_video 
SET comment = 'This is fun!' 
WHERE videoid = 12345678-1234-1111-1111-111111111111 AND commentid = 494a3f00-e966-11ea-84bf-83e48ffdc8ac;

SELECT * FROM comments_by_video;
```

```
DELETE FROM comments_by_video 
WHERE videoid = 12345678-1234-1111-1111-111111111111 AND commentid = 494a3f00-e966-11ea-84bf-83e48ffdc8ac;

SELECT * FROM comments_by_video;
```

Show us something similar with your own tables.

Try something different:

Check out the CQL reference and try commands that we did not use in the workshop:

https://docs.datastax.com/en/cql-oss/3.3/cql/cql_reference/cqlReferenceTOC.html

Let us know what you find:

```
Update with your own examples
```

Or connect, read and write to your Astra database via other methods.

Tell us how you do it, we would love to know. 

```
Show your connection code
```

The starry sky is the limit: Build your own app with Astra and show it off for a chance to have it included with our Sample Galleries



