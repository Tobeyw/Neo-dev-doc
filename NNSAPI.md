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
    "result": [
        {
            "name": [
                "my.neo"
            ]
        }
    ],
    "error": null
}
```



