## 1.GetNNSNameByAdmin(Asset,Admin,Skip,Limit)

根据管理员获取域名列表

**Request**

```
{
    "jsonrpc": "2.0",
    "method": "GetNNSNameByAdmin",
    "params": {
        "Asset": "0x152fa9ceeb2c83f40e3d3d6da6c1f8898dd4891a",
        "Admin": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
        "Skip": 0,
        "Limit": 999999
    },
    "id": 1
}
```



**Reponse**

```
{
    "id": 1,
    "result": {
        "my.neo": "1680256193096"
    },
    "error": null
}
```



## 2.GetNNSNameByOwner(Asset,Owner,Skip,Limit)

根据持有者获取域名列表

**Request**

```
{
    "jsonrpc": "2.0",
    "method": "GetNNSNameByOwner",
    "params": {
        "Asset": "0x152fa9ceeb2c83f40e3d3d6da6c1f8898dd4891a",
        "Owner": "0x0011ec424fb8e1a24bda6b4c590ae31ff16e52ae",
        "Skip": 0,
        "Limit": 2
    },
    "id": 1
}
```



**Reponse**

```
{
    "id": 1,
    "result": [
        {
            "address": "0x0011ec424fb8e1a24bda6b4c590ae31ff16e52ae",
            "asset": "0x152fa9ceeb2c83f40e3d3d6da6c1f8898dd4891a",
            "expiration": "1672914472497",
            "name": "t5.neo",
            "tokenid": "dDUubmVv"
        }
    ],
    "error": null
}
```



