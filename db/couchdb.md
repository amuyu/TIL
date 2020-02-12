
# mango query
{
   "selector": {
      "docType": {
         "$eq": "STATUS"
      }
   }
}

# query
http://localhost:5984/{db}/_all_docs
http://localhost:5984/mychannel_emb-0_10_1_1/_all_docs


http://localhost:5984/mychannel_emb-0_10_1_1/_design/indexHistoryDoc


##  design document information
http://localhost:5984/mychannel_emb-0_10_1_1/_design/indexHistoryDoc/_info
{
    "name": "indexHistoryDoc",
    "view_index": {
        "updates_pending": {
            "minimum": 0,
            "preferred": 0,
            "total": 0
        },
        "waiting_commit": false,
        "waiting_clients": 0,
        "updater_running": false,
        "update_seq": 177,
        "sizes": {
            "file": 357389,
            "external": 3600,
            "active": 15944
        },
        "signature": "923bddfa4befb6d2ff2de52a54f20b0a",
        "purge_seq": 0,
        "language": "query",
        "disk_size": 357389,
        "data_size": 3600,
        "compact_running": false
    }
}

view
map function
reduce


