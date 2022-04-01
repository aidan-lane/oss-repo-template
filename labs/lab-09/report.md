# Lab 09 Report - Introduction to Open Source Software

## Checkpoint 0
https://github.com/aidan-lane/oss-repo-template/wiki/Lab-09---Checkpoint-0

## Checkpoint 1
`http://localhost:5984`
![](c1.png)

## Checkpoint 2

**GET request:**

```
{
    "couchdb": "Welcome",
    "version": "3.2.1",
    "git_sha": "244d428af",
    "uuid": "fe59248c49978d0441161f75653a32a8",
    "features": [
        "access-ready",
        "partitioned",
        "pluggable-storage-engines",
        "reshard",
        "scheduler"
    ],
    "vendor": {
        "name": "The Apache Software Foundation"
    }
}
```

**Get Databases**

```
[
    "_global_changes",
    "_replicator",
    "_users"
]
```

**Add Baseball Database**

```
[
    "_global_changes",
    "_replicator",
    "_users",
    "baseball"
]
```

**Try adding db that already exists**

```
{
    "error": "file_exists",
    "reason": "The database could not be created, the file already exists."
}
```

**Add plankton db and get databases**

```
[
    "_global_changes",
    "_replicator",
    "_users",
    "baseball",
    "plankton"
]
```

**Delete plankton db**

```
[
    "_global_changes",
    "_replicator",
    "_users",
    "baseball"
]
```

**Create Doc**

![](c2doc.png)

**Show Docs**

![](c2faux.png)

**Query by single year**

![](c2_3.png)

**Index year and query by year in the 1980s**

![](c2_4.png)

**After replication**

![](c2_5.png)

## Checkpoint 3

**Add albums db and re-add**

```
{
    "error": "file_exists",
    "reason": "The database could not be created, the file already exists."
}
```

**curl with -v**

```
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to 127.0.0.1 (127.0.0.1) port 5984 (#0)
* Server auth using Basic with user 'admin'
> PUT /albums-backup HTTP/1.1
> Host: 127.0.0.1:5984
> Authorization: Basic YWRtaW46YmxhY2tvdXQ=
> User-Agent: curl/7.58.0
> Accept: */*
>
< HTTP/1.1 201 Created
< Cache-Control: must-revalidate
< Content-Length: 12
< Content-Type: application/json
< Date: Fri, 01 Apr 2022 17:02:46 GMT
< Location: http://127.0.0.1:5984/albums-backup
< Server: CouchDB/3.2.1 (Erlang OTP/23)
< X-Couch-Request-ID: 55b2be330d
< X-CouchDB-Body-Time: 0
<
{"ok":true}
* Connection #0 to host 127.0.0.1 left intact
```

**Add doc to albums**

```
{"ok":true,"id":"6e1295ed6c29495e54cc05947f18c8af","rev":"1-4b39c2971c9ad54cb37e08fa02fec636"}
```

**Get by ID**

```
{
    "_id": "6e1295ed6c29495e54cc05947f18c8af",
    "_rev": "1-4b39c2971c9ad54cb37e08fa02fec636",
    "title": "There is Nothing Left to Lose",
    "artist": "Foo Fighters"
}
```

**Conflict when same id used**

```
{
    "error": "conflict",
    "reason": "Document update conflict."
}
```

**Run query with -v**

```
* Server auth using Basic with user 'admin'
> PUT /albums/70b50bfa0a4b3aed1f8aff9e92dc16a0 HTTP/1.1
> Host: 127.0.0.1:5984
> Authorization: Basic YWRtaW46YmxhY2tvdXQ=
> User-Agent: curl/7.58.0
> Accept: */*
> Content-Length: 60
> Content-Type: application/x-www-form-urlencoded
>
} [60 bytes data]
* upload completely sent off: 60 out of 60 bytes
< HTTP/1.1 201 Created
< Cache-Control: must-revalidate
< Content-Length: 95
< Content-Type: application/json
< Date: Fri, 01 Apr 2022 17:07:41 GMT
< ETag: "1-d789a27476ed3962a24736e4138ff71f"
< Location: http://127.0.0.1:5984/albums/70b50bfa0a4b3aed1f8aff9e92dc16a0
< Server: CouchDB/3.2.1 (Erlang OTP/23)
< X-Couch-Request-ID: 5e130bd677
< X-CouchDB-Body-Time: 0
<
{ [95 bytes data]
100   155  100    95  100    60   6333   4000 --:--:-- --:--:-- --:--:-- 10333
* Connection #0 to host 127.0.0.1 left intact
{
    "ok": true,
    "id": "70b50bfa0a4b3aed1f8aff9e92dc16a0",
    "rev": "1-d789a27476ed3962a24736e4138ff71f"
}
```

**Attach and get artwork**

```
{
    "_id": "6e1295ed6c29495e54cc05947f18c8af",
    "_rev": "3-131533518",
    "title": "There is Nothing Left to Lose",
    "artist": "Foo Fighters",
    "year": "1997",
    "_attachments": {
        "artwork.jpg": {
            "stub": true,
            "content_type": "image/jpg",
            "length": 52450
        }
    }
}
```

**Create replica with curl**

```
{"ok":true,"session_id":"392d75aa4bf8dabcdcaf6aa4ffce6b75","source_last_seq":"2-g1AAAACbeJzLYWBgYMpgTmEQTM4vTc5ISXIwNDLXMwBCwxyQVCJDUv3___-zMpgTGXOBAuyGaYlmqSkW2DTgMSaPBUgyNACp_yimpVmaWSaapmLTlwUAPkwowA","replication_id_version":4,"history":[{"session_id":"392d75aa4bf8dabcdcaf6aa4ffce6b75","start_time":"Fri, 01 Apr 2022 17:16:40 GMT","end_time":"Fri, 01 Apr 2022 17:16:41 GMT","start_last_seq":0,"end_last_seq":"2-g1AAAACbeJzLYWBgYMpgTmEQTM4vTc5ISXIwNDLXMwBCwxyQVCJDUv3___-zMpgTGXOBAuyGaYlmqSkW2DTgMSaPBUgyNACp_yimpVmaWSaapmLTlwUAPkwowA","recorded_seq":"2-g1AAAACbeJzLYWBgYMpgTmEQTM4vTc5ISXIwNDLXMwBCwxyQVCJDUv3___-zMpgTGXOBAuyGaYlmqSkW2DTgMSaPBUgyNACp_yimpVmaWSaapmLTlwUAPkwowA","missing_checked":2,"missing_found":2,"docs_read":2,"docs_written":2,"doc_write_failures":0}]}
* Connection #0 to host 127.0.0.1 left intact
```

## Checkpoint 4

**1.)**

```
{
    "docs": [
        {
            "_id": "00a271787f89c0ef2e10e88a0c0001f4",
            "_rev": "1-c7f3ec5e61c7a461d83da2d42f64a9e4",
            "type": "movie",
            "title": "My Neighbour Totoro",
            "year": 1988,
            "director": "miyazaki",
            "rating": 8.2
        },
        {
            "_id": "00a271787f89c0ef2e10e88a0c0003f0",
            "_rev": "1-5facb9c84b721a5fe8d667c1140a54d3",
            "type": "movie",
            "title": "Kikis Delivery Service",
            "year": 1989,
            "director": "miyazaki",
            "rating": 7.8
        },
        {
            "_id": "00a271787f89c0ef2e10e88a0c00048b",
            "_rev": "1-90be3f3651b72d82da71a875e63de9c0",
            "type": "movie",
            "title": "Princess Mononoke",
            "year": 1997,
            "director": "miyazaki",
            "rating": 8.4
        }
    ],
    "bookmark": "g2wAAAACaAJkAA5zdGFydGtleV9kb2NpZG0AAAAgMDBhMjcxNzg3Zjg5YzBlZjJlMTBlODhhMGMwMDA0OGJoAmQACHN0YXJ0a2V5bAAAAAFiAAAHzWpq"
}
```

**2.)**

```
{
    "docs": [
        {
            "_id": "00a271787f89c0ef2e10e88a0c0001f4",
            "_rev": "1-c7f3ec5e61c7a461d83da2d42f64a9e4",
            "type": "movie",
            "title": "My Neighbour Totoro",
            "year": 1988,
            "director": "miyazaki",
            "rating": 8.2
        },
        {
            "_id": "00a271787f89c0ef2e10e88a0c00048b",
            "_rev": "1-90be3f3651b72d82da71a875e63de9c0",
            "type": "movie",
            "title": "Princess Mononoke",
            "year": 1997,
            "director": "miyazaki",
            "rating": 8.4
        }
    ],
    "bookmark": "g1AAAABweJzLYWBgYMpgSmHgKy5JLCrJTq2MT8lPzkzJBYorGBgkGpkbmluYp1lYJhukphmlGhqkWlgkGiQbGBiYWCSB9HHA9BGlIwsAfPcdnw",
    "warning": "No matching index found, create an index to optimize query time."
}
```

**3.)**

**b-tree index**

```
{
   "index": {
      "fields": [
         "title"
      ]
   },
   "name": "title-json-index",
   "type": "json"
}
```

**Index creation result**

```
{"result":"created","id":"_design/5e0c1e3422759aefaa2aa14ed5a8323ba33f06e3","name":"title-json-index"}
```

**4.)**

**Rerun query result**

```
{
    "docs": [
        {
            "_id": "00a271787f89c0ef2e10e88a0c0001f4",
            "_rev": "1-c7f3ec5e61c7a461d83da2d42f64a9e4",
            "type": "movie",
            "title": "My Neighbour Totoro",
            "year": 1988,
            "director": "miyazaki",
            "rating": 8.2
        },
        {
            "_id": "00a271787f89c0ef2e10e88a0c00048b",
            "_rev": "1-90be3f3651b72d82da71a875e63de9c0",
            "type": "movie",
            "title": "Princess Mononoke",
            "year": 1997,
            "director": "miyazaki",
            "rating": 8.4
        }
    ],
    "bookmark": "g1AAAABneJzLYWBgYMpgSmHgKy5JLCrJTq2MT8lPzkzJBYorGBgkGpkbmluYp1lYJhukphmlGhqkWlgkGiQbGBiYWCSB9HHA9OUAdTCCtAkGFGXmJacWFyv45ucBYXZqVhYAgz0cvw"
}
```
