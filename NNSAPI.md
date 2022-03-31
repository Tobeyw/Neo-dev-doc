## 1.GetNNSNameByAdmin(Asset,Admin,Skip,Limit)

根据管理员获取域名列表

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNNSNameByAdmin",
  "params": {
      "Asset":"0x152fa9ceeb2c83f40e3d3d6da6c1f8898dd4891a",
      "Admin":"0xf0a33d62f32528c25e68951286f238ad24e30032",
      "Skip":0,
      "Limit":20   
       },
  "id": 1
}
```



**Reponse**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "_id": "61b722130b1347a931f4ca19",
                "amount": "1",
                "asset": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
                "auctionAmount": "10",
                "auctionAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
                "auctionType": 2,
                "auctor": "0xf0a33d62f32528c25e68951286f238ad24e30032",
                "bidAmount": "20",
                "bidder": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                "deadline": 1639566850621,
                "image": "",
                "market": "0xf63cccfe6cfac7ee776dada552b976c74fe5b51a",
                "name": "sell-1",
                "owner": "0xf63cccfe6cfac7ee776dada552b976c74fe5b51a",
                "state": "auction",
                "timestamp": 1639466059477,
                "tokenid": "b7mzAd/hhpBYX95Gq8eJwkoZdS9JssMHHhJztAQNCKs="
            },
            ....
            
        ],
        "totalCount": 2
    },
    "error": null
}
```




