[TOC]



## 1.GetNftMarket(ContractHash,AssetHash,NFTState)

**NftState**:  auction    sale    notlisted  

**Sort** :  timestamp (上架时间)   price（上架价格）    deadline（截止时间）

**Order**：-1:降序  +1：升序

按照合约分类ContractHash、币种(AssetHash)、NFT状态(NFTState)，获取NFT列表

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTMarket",
  "params": {     
      "ContractHash":"",
      "AssetHash":"",
      "SecondaryMarket":"0x1f594c26a50d25d22d8afc3f1843b4ddb17cf180",
	  "PrimaryMarket":"0x1ba667322022693c8629d87b804f5d7730d10779",
      "NFTstate":"",
      "Sort":"",
      "Order":-1,        
      "Skip":0,
      "Limit":20
     
       },
  "id": 1
}
```

对应状态判断条件：

**拍卖中**：amount >0 && auctionType =2 &&  owner=market && runtime <deadline

**出售中**：amount >0 && auctionType =1 && owner=market && runtime <deadline

**未上架**：amount >0 && owner != market  

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



##  2. GetNFTOwnedByAddress(Address,ContractHash，AssetHash,NFTstate,MarketHash,Sort,Order,Limit,Skip)

获取用户所有的nft列表,以及Nft状态

**NftState**: auction sale notlisted unclaimed,

**Sort** : timestamp price

**Order**：-1:降序 +1：升序

按照合约分类ContractHash、币种(AssetHash)、NFT状态(NFTState)，获取NFT列表

对应状态判断条件：

**拍卖中**：amount >0 && auctionType =2 && owner=market && runtime <deadline

**出售中**：amount >0 && auctionType =1 && owner=market && runtime <deadline

**未上架**：amount >0 && owner != market

**未领取**：amount >0 && runtime > deadline && owner=market 

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTOwnedByAddress",
  "params": {
      "Address":"0xdd58b7a05fd9b58a6ec36d6401a89ff2cda224a2", //(必填字段)
      "ContractHash":"0xc7b11b46f97bda7a8c82793841abba120e96695b",
      "AssetHash":"",
     " MarketHash":"0x1f594c26a50d25d22d8afc3f1843b4ddb17cf180",
      "NFTstate":"notlisted",
      "Sort":"",
      "Order":1,       
      "Skip":0,
      "Limit":10
     
       },
  "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "_id": "61b7254d79025710198f0e5b",
                "amount": "0",
                "asset": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
                "auctionAmount": "0",
                "auctionAsset": null,
                "auctionType": 0,
                "auctor": null,
                "bidAmount": "0",
                "bidder": null,
                "deadline": 0,
                "image": "",
                "market": null,
                "name": "sell-1",
                "owner": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                "state": "",
                "timestamp": 1639393728600,
                "tokenid": "b7mzAd/hhpBYX95Gq8eJwkoZdS9JssMHHhJztAQNCKs="
            }
        ],
        "totalCount": 1
    },
    "error": null
}
```



## 3. GetNFTRecordByAddress(Address,MarketContractHash) 

获取用户对Nft所有操作事件信息

事件信息汇总：

```
	Cancel T = "cancel" //卖家下架
	//直买直卖
	Sell_Listed  T = "sell_listed"  //卖家上架
	Sell_Expired T = "sell_expired" //卖家上架过期
	Sell_Sold    T = "sell_sold"    //卖家售出
	Sell_Buy     T = "sell_buy"     //买家购买
	//拍卖
	Auction_Listed  T = "auction_listed"  //卖家拍卖
	Auction_Expired T = "auction_expired" //卖家拍卖过期
	Aucion_Deal     T = "auction_deal"    //卖家成交

	Auction_Bid      T = "auction_bid"      //买家出价
	Auction_Return   T = "auction_return"   //买家出价退回
	Auction_Bid_Deal T = "auction_bid_deal" //买家竞价成功
	Auction_Withdraw T = "auction_withdraw" //买家领取

	//TRANSFER  
	Send    T = "send"    //发送
	Receive T = "receive" //接收
```

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTRecordByAddress",
  "params": {
      "Address":"0xf0a33d62f32528c25e68951286f238ad24e30032",
      "MarketHash": "0xdd58b7a05fd9b58a6ec36d6401a89ff2cda224a2"    
  },
  "id": 1
} 
```

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "asset": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
                "auctionAmount": 10,
                "auctionAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
                "event": "Claim",
                "from": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                "image": "",
                "name": "sell-1",
                "nonce": 5,
                "state": "sell_sold",
                "timestamp": 1639392588921,
                "to": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                "tokenid": "b7mzAd/hhpBYX95Gq8eJwkoZdS9JssMHHhJztAQNCKs=",
                "user": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e"
            },
            .....
        ],
        "totalCount": 7
    },
    "error": null
}
```



## 4.GetNFTRecordByContractHashTokenId(ContractHash,TokenId,MarketHash) 

获取某个Nft在用户之间的历史记录

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTRecordByContractHashTokenId",
  "params": {
      "ContractHash":"0xc7b11b46f97bda7a8c82793841abba120e96695b",   
      "TokenId":"BoN2dx2fSFeRuT7kp87u3e1Jewc3ZIqQ5U0dQSdxofA=",
      "MarketHash":""
  },
  "id": 1
}
```

<!--当auctionAmount和auctionAsset值为空时，则为普通账户之间转账-->

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "asset": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
                "auctionAmount": 100,
                "auctionAsset": "0x0daba9cbfa59cf4d43ff1b76d3691725da278450",
                "from": "0xf63cccfe6cfac7ee776dada552b976c74fe5b51a",
                "image": "",
                "name": "1",
                "timestamp": 1639380705777,
                "to": "0x78fed05e0ed095b47826bd7461da11c8281195f6",
                "tokenid": "BoN2dx2fSFeRuT7kp87u3e1Jewc3ZIqQ5U0dQSdxofA="
            },
            ......
        ],
        "totalCount": 7
    },
    "error": null
}
```



## 5.GetBidInfoByNFT(Address,AssetHash,TokenId,MarketHash)

获取NFT所有出价记录信息

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetBidInfoByNFT",
  "params": {
      "Address":"",
      "AssetHash": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
      "TokenId":"az2dNYa7xEzk2XAQoHnH22k6AbO5/RkyqMDK64VuuXE=",
      "MarketHash":""
  
  },
  "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "asset": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
                "auctionAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
                "bidAmount": "20",
                "bidder": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                "timestamp": 1639393572133,
                "tokenid": "az2dNYa7xEzk2XAQoHnH22k6AbO5/RkyqMDK64VuuXE="
            },
           ......
        ],
        "totalCount": 3
    },
    "error": null
}
```



## 6.GetNFTByContractHashTokenId(ContractHash,TokenIds,MarketHash)

通过ContractHash 和Tokenid 获取指定NFT的信息

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTByContractHashTokenId",
  "params": {
      "ContractHash":"0xc7b11b46f97bda7a8c82793841abba120e96695b",     
      "TokenIds":["LzKk2aeLybZTv83Hzw8djcvJJyVldIyi8oly1qqmqUo="],
      "MarketHash":""
      
      },
  "id": 1
}

```

<!--tokenids为空时，则获取所有-->

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "_id": "61b724ab0b1347a931f4cae2",
                "amount": "1",
                "asset": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
                "auctionAmount": "10",
                "auctionAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
                "auctionType": 2,
                "auctor": "0xc65e19cfa66b61800ce582d1b55f4e93fa214b17",
                "bidAmount": "20",
                "bidder": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                "deadline": 1639565840451,
                "market": "0xf63cccfe6cfac7ee776dada552b976c74fe5b51a",
                "owner": "0xf63cccfe6cfac7ee776dada552b976c74fe5b51a",
                "state": "auction",
                "timestamp": 1639393572133,
                "tokenid": "az2dNYa7xEzk2XAQoHnH22k6AbO5/RkyqMDK64VuuXE="
            }
        ],
        "totalCount": 1
    },
    "error": null
}
```



## 7.GetAllBidInfoByNFT(AssetHash,TokenId,MarketHash)

获取指定NFT所有历史竞价信息记录

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetAllBidInfoByNFT",
  "params": {      
      "AssetHash": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
      "TokenId":"b7mzAd/hhpBYX95Gq8eJwkoZdS9JssMHHhJztAQNCKs=",
      "MarketHash":""
  },
  "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "asset": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
                "bidAmount": [
                    20,
                    15,
                    10
                ],
                "bidder": [
                    "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                    "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                    "0xf0a33d62f32528c25e68951286f238ad24e30032"
                ],
                "nonce": 9,
                "tokenid": "b7mzAd/hhpBYX95Gq8eJwkoZdS9JssMHHhJztAQNCKs="
            }
        ],
        "totalCount": 1
    },
    "error": null
}
```

## 8.GetNFTClass(AssetHash,MarketHash)

获取一级市场的分类

**Request**

```
{"jsonrpc":"2.0",
    "method":"GetNFTClass", 
    "params":{
        "AssetHash":"0x6a2893f97401e2b58b757f59d71238d91339856a",
        "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1"
        "NFTState" :"sale/auction"
    },
    "id":1}
```

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
          {
                "asset": "0x6a2893f97401e2b58b757f59d71238d91339856a",
                "claimed": 3,
                "deadline": "1661396823663",
                "image": "",
                "name": "HASH",
                "number": 1,
                "currentBidAmount": "15144534",  //竞拍价格
                "currentBidAsset": "0x85deac50febfd93988d3f391dea54e8289e43e9e", //竞拍token
                "auctionAmount": "15144534",                //起拍价格/直买直卖价格
                "auctionAsset": "0x85deac50febfd93988d3f391dea54e8289e43e9e",    //起拍token/直买直卖token
                "series": "THE POLE BLOCK",
                "supply": "3",
                "thumbnail": "",
                "auctionType":"0/1",   //0 直买直卖  1 竞拍 
                "video" :"",
                "count":"",
                
            },
            ....
        ],
        "totalCount": 2
    },
    "error": null
}
```

## 9.GetMarketIndexByAsset(AssetHash)

获取二级市场某一系列的指标

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetMarketIndexByAsset",
  "params": {     
      "AssetHash":"0xc7b11b46f97bda7a8c82793841abba120e96695b"
      },      
  "id": 1
}
```

**Response**

```

{
    "id": 1,
    "result": {
        "auctionAmount":  "5",   //地板价价格   nep17
        "auctionAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
        "conAmount": 3.2919624466782094,    //地板价价格     usd  
        "totalowner": 6,   //owner总量
        "totalsupply": 28, // NFT系列总量
        "totaltxamount":277.03192125227014   // 交易总额  usd
    },
    "error": null
}
```

## 10.GetMarketTokenidList(Account,MarketHash，AssetHash,SubClass,Limit,Skip)

查询一级市场在售NFT的tokenid 列表

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetMarketTokenidList",
  "params": {
      "Account":"0x2296bd323004b439f46d1557b7b58a8f6cfe36af",
      "AssetHash":"0x0e35991d4eaeea0ff35b4a849342d59c8091de18",
      "MarketHash":"0x0b92cf1c2f308d8084085dc446f7c033b753e959",     
      "SubClass": [
			[
				"TWV0YVBhbmFjZWEgIzEtMDE=",
				"TWV0YVBhbmFjZWEgIzEtMDU="
			],
			[
				"TWV0YVBhbmFjZWEgIzItMDE=",
				"TWV0YVBhbmFjZWEgIzItMDU="
			],
			[
				"TWV0YVBhbmFjZWEgIzMtMDE=",
				"TWV0YVBhbmFjZWEgIzMtMDU="
			],
			[
				"TWV0YVBhbmFjZWEgIzQtMDE=",
				"TWV0YVBhbmFjZWEgIzQtMTU="
			],
			[
				"TWV0YVBhbmFjZWEgIzUtMDE=",
				"TWV0YVBhbmFjZWEgIzUtMTU="
			],
			[
				"TWV0YVBhbmFjZWEgIzYtMDE=",
				"TWV0YVBhbmFjZWEgIzYtMTU="
			],
			[
				"TWV0YVBhbmFjZWEgIzctMDE=",
				"TWV0YVBhbmFjZWEgIzctMTU="
			],
			[
				"TWV0YVBhbmFjZWEgIzgtMDE=",
				"TWV0YVBhbmFjZWEgIzgtMTU="
			],
			[
				"TWV0YVBhbmFjZWEgIzktMDE=",
				"TWV0YVBhbmFjZWEgIzktMTU="
			],
			[
				"TWV0YVBhbmFjZWEgIzEwLTAx",
				"TWV0YVBhbmFjZWEgIzEwLTMz"
			],
			[
				"TWV0YVBhbmFjZWEgIzExLTAx",
				"TWV0YVBhbmFjZWEgIzExLTMz"
			],
			[
				"TWV0YVBhbmFjZWEgIzEyLTAx",
				"TWV0YVBhbmFjZWEgIzEyLTMz"
			],
			[
				"TWV0YVBhbmFjZWEgIzEzLTAx",
				"TWV0YVBhbmFjZWEgIzEzLTMz"
			],
			[
				"TWV0YVBhbmFjZWEgIzE0LTAx",
				"TWV0YVBhbmFjZWEgIzE0LTMz"
			],
			[
				"TWV0YVBhbmFjZWEgIzE1LTAx",
				"TWV0YVBhbmFjZWEgIzE1LTMz"
			],
			[
				"TWV0YVBhbmFjZWEgIzE2LTAx",
				"TWV0YVBhbmFjZWEgIzE2LTMz"
			],
			[
				"TWV0YVBhbmFjZWEgIzE3LTAx",
				"TWV0YVBhbmFjZWEgIzE3LTMz"
			],
			[
				"TWV0YVBhbmFjZWEgIzE4LTAx",
				"TWV0YVBhbmFjZWEgIzE4LTMz"
			]
		]
     
      
       },
  "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "id": 0,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzEtMDU=",
                    "TWV0YVBhbmFjZWEgIzEtMDQ=",
                    "TWV0YVBhbmFjZWEgIzEtMDI=",
                    "TWV0YVBhbmFjZWEgIzEtMDM=",
                    "TWV0YVBhbmFjZWEgIzEtMDE="
                ]
            },
            {
                "id": 1,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzItMDU=",
                    "TWV0YVBhbmFjZWEgIzItMDM=",
                    "TWV0YVBhbmFjZWEgIzItMDE=",
                    "TWV0YVBhbmFjZWEgIzItMDI=",
                    "TWV0YVBhbmFjZWEgIzItMDQ="
                ]
            },
            {
                "id": 2,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzMtMDI=",
                    "TWV0YVBhbmFjZWEgIzMtMDU=",
                    "TWV0YVBhbmFjZWEgIzMtMDM=",
                    "TWV0YVBhbmFjZWEgIzMtMDQ=",
                    "TWV0YVBhbmFjZWEgIzMtMDE="
                ]
            },
            {
                "id": 3,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzQtMTM=",
                    "TWV0YVBhbmFjZWEgIzQtMDk=",
                    "TWV0YVBhbmFjZWEgIzQtMTA=",
                    "TWV0YVBhbmFjZWEgIzQtMTE=",
                    "TWV0YVBhbmFjZWEgIzQtMTI=",
                    "TWV0YVBhbmFjZWEgIzQtMDQ=",
                    "TWV0YVBhbmFjZWEgIzQtMDg=",
                    "TWV0YVBhbmFjZWEgIzQtMDU=",
                    "TWV0YVBhbmFjZWEgIzQtMTQ=",
                    "TWV0YVBhbmFjZWEgIzQtMDE=",
                    "TWV0YVBhbmFjZWEgIzQtMDc=",
                    "TWV0YVBhbmFjZWEgIzQtMDM=",
                    "TWV0YVBhbmFjZWEgIzQtMDI=",
                    "TWV0YVBhbmFjZWEgIzQtMTU=",
                    "TWV0YVBhbmFjZWEgIzQtMDY="
                ]
            },
            {
                "id": 4,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzUtMTA=",
                    "TWV0YVBhbmFjZWEgIzUtMTQ=",
                    "TWV0YVBhbmFjZWEgIzUtMDE=",
                    "TWV0YVBhbmFjZWEgIzUtMTE=",
                    "TWV0YVBhbmFjZWEgIzUtMTM=",
                    "TWV0YVBhbmFjZWEgIzUtMTU=",
                    "TWV0YVBhbmFjZWEgIzUtMDI=",
                    "TWV0YVBhbmFjZWEgIzUtMDk=",
                    "TWV0YVBhbmFjZWEgIzUtMDY=",
                    "TWV0YVBhbmFjZWEgIzUtMDc=",
                    "TWV0YVBhbmFjZWEgIzUtMDU=",
                    "TWV0YVBhbmFjZWEgIzUtMTI=",
                    "TWV0YVBhbmFjZWEgIzUtMDM=",
                    "TWV0YVBhbmFjZWEgIzUtMDg=",
                    "TWV0YVBhbmFjZWEgIzUtMDQ="
                ]
            },
            {
                "id": 5,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzYtMDU=",
                    "TWV0YVBhbmFjZWEgIzYtMTA=",
                    "TWV0YVBhbmFjZWEgIzYtMTU=",
                    "TWV0YVBhbmFjZWEgIzYtMDk=",
                    "TWV0YVBhbmFjZWEgIzYtMTQ=",
                    "TWV0YVBhbmFjZWEgIzYtMDI=",
                    "TWV0YVBhbmFjZWEgIzYtMDQ=",
                    "TWV0YVBhbmFjZWEgIzYtMDc=",
                    "TWV0YVBhbmFjZWEgIzYtMTE=",
                    "TWV0YVBhbmFjZWEgIzYtMDg=",
                    "TWV0YVBhbmFjZWEgIzYtMTI=",
                    "TWV0YVBhbmFjZWEgIzYtMDY=",
                    "TWV0YVBhbmFjZWEgIzYtMDM=",
                    "TWV0YVBhbmFjZWEgIzYtMDE=",
                    "TWV0YVBhbmFjZWEgIzYtMTM="
                ]
            },
            {
                "id": 6,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzctMDk=",
                    "TWV0YVBhbmFjZWEgIzctMDc=",
                    "TWV0YVBhbmFjZWEgIzctMDE=",
                    "TWV0YVBhbmFjZWEgIzctMDI=",
                    "TWV0YVBhbmFjZWEgIzctMDg=",
                    "TWV0YVBhbmFjZWEgIzctMTI=",
                    "TWV0YVBhbmFjZWEgIzctMTA=",
                    "TWV0YVBhbmFjZWEgIzctMDQ=",
                    "TWV0YVBhbmFjZWEgIzctMDM=",
                    "TWV0YVBhbmFjZWEgIzctMTQ=",
                    "TWV0YVBhbmFjZWEgIzctMTU=",
                    "TWV0YVBhbmFjZWEgIzctMDU=",
                    "TWV0YVBhbmFjZWEgIzctMTE=",
                    "TWV0YVBhbmFjZWEgIzctMTM=",
                    "TWV0YVBhbmFjZWEgIzctMDY="
                ]
            },
            {
                "id": 7,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzgtMDM=",
                    "TWV0YVBhbmFjZWEgIzgtMTU=",
                    "TWV0YVBhbmFjZWEgIzgtMDY=",
                    "TWV0YVBhbmFjZWEgIzgtMTQ=",
                    "TWV0YVBhbmFjZWEgIzgtMDk=",
                    "TWV0YVBhbmFjZWEgIzgtMTE=",
                    "TWV0YVBhbmFjZWEgIzgtMDc=",
                    "TWV0YVBhbmFjZWEgIzgtMDI=",
                    "TWV0YVBhbmFjZWEgIzgtMDU=",
                    "TWV0YVBhbmFjZWEgIzgtMDg=",
                    "TWV0YVBhbmFjZWEgIzgtMTI=",
                    "TWV0YVBhbmFjZWEgIzgtMDQ=",
                    "TWV0YVBhbmFjZWEgIzgtMDE=",
                    "TWV0YVBhbmFjZWEgIzgtMTM="
                ]
            },
            {
                "id": 8,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzktMTA=",
                    "TWV0YVBhbmFjZWEgIzktMDU=",
                    "TWV0YVBhbmFjZWEgIzktMDQ=",
                    "TWV0YVBhbmFjZWEgIzktMDM=",
                    "TWV0YVBhbmFjZWEgIzktMDk=",
                    "TWV0YVBhbmFjZWEgIzktMDc=",
                    "TWV0YVBhbmFjZWEgIzktMTQ=",
                    "TWV0YVBhbmFjZWEgIzktMTI=",
                    "TWV0YVBhbmFjZWEgIzktMTE="
                ]
            },
            {
                "id": 9,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzEwLTE1",
                    "TWV0YVBhbmFjZWEgIzEwLTI5",
                    "TWV0YVBhbmFjZWEgIzEwLTMw",
                    "TWV0YVBhbmFjZWEgIzEwLTEy",
                    "TWV0YVBhbmFjZWEgIzEwLTI4",
                    "TWV0YVBhbmFjZWEgIzEwLTIx",
                    "TWV0YVBhbmFjZWEgIzEwLTMy",
                    "TWV0YVBhbmFjZWEgIzEwLTIw",
                    "TWV0YVBhbmFjZWEgIzEwLTEx",
                    "TWV0YVBhbmFjZWEgIzEwLTI0",
                    "TWV0YVBhbmFjZWEgIzEwLTAz",
                    "TWV0YVBhbmFjZWEgIzEwLTEw",
                    "TWV0YVBhbmFjZWEgIzEwLTI1",
                    "TWV0YVBhbmFjZWEgIzEwLTAy",
                    "TWV0YVBhbmFjZWEgIzEwLTE0",
                    "TWV0YVBhbmFjZWEgIzEwLTI3",
                    "TWV0YVBhbmFjZWEgIzEwLTIz",
                    "TWV0YVBhbmFjZWEgIzEwLTE3",
                    "TWV0YVBhbmFjZWEgIzEwLTE5",
                    "TWV0YVBhbmFjZWEgIzEwLTE4",
                    "TWV0YVBhbmFjZWEgIzEwLTIy",
                    "TWV0YVBhbmFjZWEgIzEwLTAx",
                    "TWV0YVBhbmFjZWEgIzEwLTEz",
                    "TWV0YVBhbmFjZWEgIzEwLTMz",
                    "TWV0YVBhbmFjZWEgIzEwLTE2"
                ]
            },
            {
                "id": 10,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzExLTI5",
                    "TWV0YVBhbmFjZWEgIzExLTIx",
                    "TWV0YVBhbmFjZWEgIzExLTE0",
                    "TWV0YVBhbmFjZWEgIzExLTAz",
                    "TWV0YVBhbmFjZWEgIzExLTEx",
                    "TWV0YVBhbmFjZWEgIzExLTI2",
                    "TWV0YVBhbmFjZWEgIzExLTAx",
                    "TWV0YVBhbmFjZWEgIzExLTMw",
                    "TWV0YVBhbmFjZWEgIzExLTI0",
                    "TWV0YVBhbmFjZWEgIzExLTAy",
                    "TWV0YVBhbmFjZWEgIzExLTE4",
                    "TWV0YVBhbmFjZWEgIzExLTE2",
                    "TWV0YVBhbmFjZWEgIzExLTE3",
                    "TWV0YVBhbmFjZWEgIzExLTI4",
                    "TWV0YVBhbmFjZWEgIzExLTIw",
                    "TWV0YVBhbmFjZWEgIzExLTMy",
                    "TWV0YVBhbmFjZWEgIzExLTEy",
                    "TWV0YVBhbmFjZWEgIzExLTEw",
                    "TWV0YVBhbmFjZWEgIzExLTIz",
                    "TWV0YVBhbmFjZWEgIzExLTMz",
                    "TWV0YVBhbmFjZWEgIzExLTE1"
                ]
            },
            {
                "id": 11,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzEyLTEw",
                    "TWV0YVBhbmFjZWEgIzEyLTE4",
                    "TWV0YVBhbmFjZWEgIzEyLTE1",
                    "TWV0YVBhbmFjZWEgIzEyLTAz",
                    "TWV0YVBhbmFjZWEgIzEyLTEz",
                    "TWV0YVBhbmFjZWEgIzEyLTI5",
                    "TWV0YVBhbmFjZWEgIzEyLTI0",
                    "TWV0YVBhbmFjZWEgIzEyLTE0",
                    "TWV0YVBhbmFjZWEgIzEyLTIz",
                    "TWV0YVBhbmFjZWEgIzEyLTE3",
                    "TWV0YVBhbmFjZWEgIzEyLTE5",
                    "TWV0YVBhbmFjZWEgIzEyLTAx",
                    "TWV0YVBhbmFjZWEgIzEyLTI2",
                    "TWV0YVBhbmFjZWEgIzEyLTIw",
                    "TWV0YVBhbmFjZWEgIzEyLTIy",
                    "TWV0YVBhbmFjZWEgIzEyLTMw",
                    "TWV0YVBhbmFjZWEgIzEyLTMy",
                    "TWV0YVBhbmFjZWEgIzEyLTMx",
                    "TWV0YVBhbmFjZWEgIzEyLTMz"
                ]
            },
            {
                "id": 12,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzEzLTMx",
                    "TWV0YVBhbmFjZWEgIzEzLTI5",
                    "TWV0YVBhbmFjZWEgIzEzLTMz",
                    "TWV0YVBhbmFjZWEgIzEzLTE1",
                    "TWV0YVBhbmFjZWEgIzEzLTAz",
                    "TWV0YVBhbmFjZWEgIzEzLTEx",
                    "TWV0YVBhbmFjZWEgIzEzLTI1",
                    "TWV0YVBhbmFjZWEgIzEzLTE5",
                    "TWV0YVBhbmFjZWEgIzEzLTI2",
                    "TWV0YVBhbmFjZWEgIzEzLTIw",
                    "TWV0YVBhbmFjZWEgIzEzLTIy",
                    "TWV0YVBhbmFjZWEgIzEzLTI0",
                    "TWV0YVBhbmFjZWEgIzEzLTIz",
                    "TWV0YVBhbmFjZWEgIzEzLTEz",
                    "TWV0YVBhbmFjZWEgIzEzLTE2",
                    "TWV0YVBhbmFjZWEgIzEzLTMw",
                    "TWV0YVBhbmFjZWEgIzEzLTI4",
                    "TWV0YVBhbmFjZWEgIzEzLTE0",
                    "TWV0YVBhbmFjZWEgIzEzLTMy",
                    "TWV0YVBhbmFjZWEgIzEzLTE3",
                    "TWV0YVBhbmFjZWEgIzEzLTI3",
                    "TWV0YVBhbmFjZWEgIzEzLTIx"
                ]
            },
            {
                "id": 13,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzE0LTE3",
                    "TWV0YVBhbmFjZWEgIzE0LTIz",
                    "TWV0YVBhbmFjZWEgIzE0LTE4",
                    "TWV0YVBhbmFjZWEgIzE0LTEz",
                    "TWV0YVBhbmFjZWEgIzE0LTE5",
                    "TWV0YVBhbmFjZWEgIzE0LTMx",
                    "TWV0YVBhbmFjZWEgIzE0LTAx",
                    "TWV0YVBhbmFjZWEgIzE0LTI5",
                    "TWV0YVBhbmFjZWEgIzE0LTI4",
                    "TWV0YVBhbmFjZWEgIzE0LTIx",
                    "TWV0YVBhbmFjZWEgIzE0LTAy",
                    "TWV0YVBhbmFjZWEgIzE0LTI1",
                    "TWV0YVBhbmFjZWEgIzE0LTI2",
                    "TWV0YVBhbmFjZWEgIzE0LTIy",
                    "TWV0YVBhbmFjZWEgIzE0LTEx",
                    "TWV0YVBhbmFjZWEgIzE0LTI0",
                    "TWV0YVBhbmFjZWEgIzE0LTAz",
                    "TWV0YVBhbmFjZWEgIzE0LTEw",
                    "TWV0YVBhbmFjZWEgIzE0LTE1",
                    "TWV0YVBhbmFjZWEgIzE0LTI3",
                    "TWV0YVBhbmFjZWEgIzE0LTE2",
                    "TWV0YVBhbmFjZWEgIzE0LTIw",
                    "TWV0YVBhbmFjZWEgIzE0LTMw",
                    "TWV0YVBhbmFjZWEgIzE0LTMz",
                    "TWV0YVBhbmFjZWEgIzE0LTE0"
                ]
            },
            {
                "id": 14,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzE1LTAx",
                    "TWV0YVBhbmFjZWEgIzE1LTEx",
                    "TWV0YVBhbmFjZWEgIzE1LTIw",
                    "TWV0YVBhbmFjZWEgIzE1LTI4",
                    "TWV0YVBhbmFjZWEgIzE1LTI1",
                    "TWV0YVBhbmFjZWEgIzE1LTI5",
                    "TWV0YVBhbmFjZWEgIzE1LTEz",
                    "TWV0YVBhbmFjZWEgIzE1LTAy",
                    "TWV0YVBhbmFjZWEgIzE1LTE1",
                    "TWV0YVBhbmFjZWEgIzE1LTEw",
                    "TWV0YVBhbmFjZWEgIzE1LTIy",
                    "TWV0YVBhbmFjZWEgIzE1LTE2",
                    "TWV0YVBhbmFjZWEgIzE1LTE4",
                    "TWV0YVBhbmFjZWEgIzE1LTI2",
                    "TWV0YVBhbmFjZWEgIzE1LTEy",
                    "TWV0YVBhbmFjZWEgIzE1LTIz",
                    "TWV0YVBhbmFjZWEgIzE1LTE5",
                    "TWV0YVBhbmFjZWEgIzE1LTI3",
                    "TWV0YVBhbmFjZWEgIzE1LTI0",
                    "TWV0YVBhbmFjZWEgIzE1LTE0",
                    "TWV0YVBhbmFjZWEgIzE1LTIx",
                    "TWV0YVBhbmFjZWEgIzE1LTE3",
                    "TWV0YVBhbmFjZWEgIzE1LTMw",
                    "TWV0YVBhbmFjZWEgIzE1LTMy",
                    "TWV0YVBhbmFjZWEgIzE1LTMz"
                ]
            },
            {
                "id": 15,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzE2LTAx",
                    "TWV0YVBhbmFjZWEgIzE2LTAy",
                    "TWV0YVBhbmFjZWEgIzE2LTE5",
                    "TWV0YVBhbmFjZWEgIzE2LTIz",
                    "TWV0YVBhbmFjZWEgIzE2LTE0",
                    "TWV0YVBhbmFjZWEgIzE2LTI4",
                    "TWV0YVBhbmFjZWEgIzE2LTE2",
                    "TWV0YVBhbmFjZWEgIzE2LTI2",
                    "TWV0YVBhbmFjZWEgIzE2LTIx",
                    "TWV0YVBhbmFjZWEgIzE2LTEy",
                    "TWV0YVBhbmFjZWEgIzE2LTI0",
                    "TWV0YVBhbmFjZWEgIzE2LTAz",
                    "TWV0YVBhbmFjZWEgIzE2LTMw",
                    "TWV0YVBhbmFjZWEgIzE2LTMx",
                    "TWV0YVBhbmFjZWEgIzE2LTMy",
                    "TWV0YVBhbmFjZWEgIzE2LTE1",
                    "TWV0YVBhbmFjZWEgIzE2LTEx",
                    "TWV0YVBhbmFjZWEgIzE2LTI5",
                    "TWV0YVBhbmFjZWEgIzE2LTE3",
                    "TWV0YVBhbmFjZWEgIzE2LTE4",
                    "TWV0YVBhbmFjZWEgIzE2LTI3",
                    "TWV0YVBhbmFjZWEgIzE2LTIw",
                    "TWV0YVBhbmFjZWEgIzE2LTI1",
                    "TWV0YVBhbmFjZWEgIzE2LTIy",
                    "TWV0YVBhbmFjZWEgIzE2LTEz"
                ]
            },
            {
                "id": 16,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzE3LTIw",
                    "TWV0YVBhbmFjZWEgIzE3LTIz",
                    "TWV0YVBhbmFjZWEgIzE3LTE5",
                    "TWV0YVBhbmFjZWEgIzE3LTEw",
                    "TWV0YVBhbmFjZWEgIzE3LTEy",
                    "TWV0YVBhbmFjZWEgIzE3LTE2",
                    "TWV0YVBhbmFjZWEgIzE3LTMw",
                    "TWV0YVBhbmFjZWEgIzE3LTE1",
                    "TWV0YVBhbmFjZWEgIzE3LTE0",
                    "TWV0YVBhbmFjZWEgIzE3LTIx",
                    "TWV0YVBhbmFjZWEgIzE3LTE3",
                    "TWV0YVBhbmFjZWEgIzE3LTI1",
                    "TWV0YVBhbmFjZWEgIzE3LTAx",
                    "TWV0YVBhbmFjZWEgIzE3LTMy",
                    "TWV0YVBhbmFjZWEgIzE3LTEz",
                    "TWV0YVBhbmFjZWEgIzE3LTIy",
                    "TWV0YVBhbmFjZWEgIzE3LTI2",
                    "TWV0YVBhbmFjZWEgIzE3LTAz",
                    "TWV0YVBhbmFjZWEgIzE3LTEx",
                    "TWV0YVBhbmFjZWEgIzE3LTI0",
                    "TWV0YVBhbmFjZWEgIzE3LTI5",
                    "TWV0YVBhbmFjZWEgIzE3LTE4"
                ]
            },
            {
                "id": 17,
                "tokenid": [
                    "TWV0YVBhbmFjZWEgIzE4LTI1",
                    "TWV0YVBhbmFjZWEgIzE4LTIx",
                    "TWV0YVBhbmFjZWEgIzE4LTAz",
                    "TWV0YVBhbmFjZWEgIzE4LTE4",
                    "TWV0YVBhbmFjZWEgIzE4LTE0",
                    "TWV0YVBhbmFjZWEgIzE4LTEy",
                    "TWV0YVBhbmFjZWEgIzE4LTEx",
                    "TWV0YVBhbmFjZWEgIzE4LTMz",
                    "TWV0YVBhbmFjZWEgIzE4LTE5",
                    "TWV0YVBhbmFjZWEgIzE4LTE3",
                    "TWV0YVBhbmFjZWEgIzE4LTI4",
                    "TWV0YVBhbmFjZWEgIzE4LTIw",
                    "TWV0YVBhbmFjZWEgIzE4LTEw",
                    "TWV0YVBhbmFjZWEgIzE4LTMx",
                    "TWV0YVBhbmFjZWEgIzE4LTAx",
                    "TWV0YVBhbmFjZWEgIzE4LTEz",
                    "TWV0YVBhbmFjZWEgIzE4LTMy",
                    "TWV0YVBhbmFjZWEgIzE4LTE1",
                    "TWV0YVBhbmFjZWEgIzE4LTE2",
                    "TWV0YVBhbmFjZWEgIzE4LTIz"
                ]
            }
        ],
        "totalCount": 18
    },
    "error": null
}
```

## 11.GetMarketWhiteList(MarketHash)

Request

```
{
  "jsonrpc": "2.0",
  "method": "GetMarketWhiteList",
  "params": {
      "MarketHash":"0x1f594c26a50d25d22d8afc3f1843b4ddb17cf180"     
       },
  "id": 1
}
```

Response

```
{
    "id": 1,
    "result": {
        "market": "0x0b92cf1c2f308d8084085dc446f7c033b753e959",
        "whiteList": [
            "0x48c40d4666f93408be1bef038b6722404d9a4c2a",
            "0x0e35991d4eaeea0ff35b4a849342d59c8091de18"
        ]
    },
    "error": null
}
```

## 12.GetWhiteListByMarketHash(MarketHash）

通过markethash 获取白名单及详细信息

Request

```
{
  "jsonrpc": "2.0",
  "method": "GetWhiteListByMarketHash",
  "params": {
      "MarketHash":"0x1f594c26a50d25d22d8afc3f1843b4ddb17cf180"
     
       },
  "id": 1
}
```

Response

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "asset": "0x48c40d4666f93408be1bef038b6722404d9a4c2a",
                "decimal": 8,
                "feeRate": "0",
                "rewardRate": "0",
                "rewardReceiveAddress": "0x481b5f71a738d3d43c5a9b621f93aa00f2a5acfd",
                "symbol": "bNEO",
                "tokenname": "BurgerNEO",
                "totalsupply": "2118188748700",
                "type": "NEP17"
            },
          ....
        ],
        "totalCount": 4
    },
    "error": null
}
```

## 13.GetNFTByWords(SecondaryMarket,PrimaryMarket,Words,Limit,Skip）

通过关键词模糊搜索

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTByWords",
  "params": {
      "SecondaryMarket":"0x1f594c26a50d25d22d8afc3f1843b4ddb17cf180",
	  "PrimaryMarket":"0x22231899d6946802f66a0fb06ce0960ae88e9eb6",
      "Words":"Blind Box",
      "Skip":0,
      "Limit":2   
       },
  "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
           {
                "_id": "61d83d880506da899828e21a",
                "amount": "1",
                "asset": "0xd9e2093de3dc2ef7cf5704ceec46ab7fadd48e7f",
                "auctionAmount": "0",
                "auctionAsset": null,
                "auctionType": 0,
                "auctor": null,
                "bidAmount": "0",
                "bidder": null,
                "deadline": 0,
                "image": "https://neo.org/BlindBox.png",
                "market": null,
                "name": "Blind Box #97",
                "number": 97,
                "owner": "0xed369077652ddd55bd7696df93fe49c0bb40d3bc",
                "properties": {
                    "image": "https://neo.org/BlindBox.png",
                    "number": 97,
                    "video": "aHR0cHM6Ly9uZW8ub3JnL0JsaW5kQm94Lm1wNA=="
                },
                "state": "notlisted",
                "timestamp": 1632237992486,
                "tokenid": "QmxpbmQgQm94ICM5Nw=="
            },
            ......
            ],
        "totalCount": 1485
    },
    "error": null
}
```

## 14.GetNFSImgStatus(Url）

获取图片的状态

Request

```
{
  "jsonrpc": "2.0",
  "method": "GetNFSImgStatus",
  "params": {"Url":"https://http.testnet.fs.neo.org/C1UKxuvGNNjEHgtGi3YAFSthsfTC9zxJtBh8eXhCmMoi/9cWgnZe75d8X1jhbkYVSVa7ZkmDT5KkeQiiCNwjfJtxC2"},
  "id": 1
}
```

Response

```
{
    "id": 1,
    "result": {
        "ImageStatus": false
    },
    "error": null
}
```



## 15.GetOffersByAddress(Address,OfferState）

根据地址获取offer列表

**OfferState**:     (默认所有)

-  valid   （按时间排序）
- received（按offerAmount 排序）

**Request**

```
{  
    "jsonrpc": "2.0",
    "method": "GetOffersByAddress",
    "params": {
        "Address":"0xa4d82bd69da0fd3e0440fd94a2c0fa10b4f26a3a",        
        "OfferState":"received",   
        "Limit":50,  
        "Skip":0
        },
    "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "_id": "62c51eaab77e14d7e360637f",
                "asset": "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
                "blockhash": "0x14d9c0fa8b63311114e116b200d57551eb050b4eaf33329c06e152127d267213",
                "deadline": 1657172010331,
                "eventname": "Offer",
                "market": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
                "nonce": 17,
                "offerAmount": 200000000,
                "offerAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
                "originOwner": "0xa4d82bd69da0fd3e0440fd94a2c0fa10b4f26a3a",
                "timestamp": 1657085610331,
                "tokenid": "TWV0YVBhbmFjZWEgIzIwLTAx",
                "txid": "0x591195ddcfeef6c8aa593561a14f691e5e8add1299e964e64a3fa4e50074f316",
                "user": "0xa4d82bd69da0fd3e0440fd94a2c0fa10b4f26a3a"
            },
           .......
        ],
        "totalCount": 2
    },
    "error": null
}
```

## 16.GetOffersByNFT(Asset,TokenId,MarketHash）

根据NFT获取offer列表

**Request**

```
{  
    "jsonrpc": "2.0",
    "method": "GetOffersByNFT",
    "params": {
        "Asset":"0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",        
        "TokenId":"TWV0YVBhbmFjZWEgIzIwLTAx",
        "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1",
        "Limit":50,  
        "Skip":0
        },
    "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "_id": "62c51eaab77e14d7e360637f",
                "asset": "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
                "blockhash": "0x14d9c0fa8b63311114e116b200d57551eb050b4eaf33329c06e152127d267213",
                "deadline": "1657172010331",
                "eventname": "Offer",
                "market": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
                "nonce": 17,
                "offerAmount": "200000000",
                "offerAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
                "originOwner": "0xa4d82bd69da0fd3e0440fd94a2c0fa10b4f26a3a",
                "timestamp": 1657085610331,
                "tokenid": "TWV0YVBhbmFjZWEgIzIwLTAx",
                "txid": "0x591195ddcfeef6c8aa593561a14f691e5e8add1299e964e64a3fa4e50074f316",
                "user": "0xa4d82bd69da0fd3e0440fd94a2c0fa10b4f26a3a"
            },
           ......
        ],
        "totalCount": 16
    },
    "error": null
}
```

## 17.GetHighestOfferByNFT(Asset,TokenId,MarketHash）

根据NFT获取最高出价有效offer

**Request**

```
{  
    "jsonrpc": "2.0",
    "method": "GetHighestOfferByNFT",
    "params": {
        "Asset":"0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",        
        "TokenId":"TWV0YVBhbmFjZWEgIzE5LTAx",
        "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1"
        },
    "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "asset": "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
        "deadline": 1660465743694,
        "guarantee": 2000000000,
        "offerAmount": 3001000000,
        "offerAsset": "0x85deac50febfd93988d3f391dea54e8289e43e9e",
        "originOwner": "0xf0a33d62f32528c25e68951286f238ad24e30032",
        "tokenid": "TWV0YVBhbmFjZWEgIzE5LTAx",
        "usdAmount": 1,
        "user": "0xfa03cb7b40072c69ca41f0ad3606a548f1d59966"
    },
    "error": null
}
```

## 18.GetHighestOfferByNFTList(NFT,MarketHash）

根据NFT获取最高出价有效offer

**Request**

```
{  
    "jsonrpc": "2.0",
    "method": "GetHighestOfferByNFTList",
    "params": {            
        "NFT":[
            { "Asset":"0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56","TokenId":"TWV0YVBhbmFjZWEgIzIwLTAx"},
            { "Asset":"0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56","TokenId":"TWV0YVBhbmFjZWEgIzAtMDE="},
            { "Asset":"0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56","TokenId":"TWV0YVBhbmFjZWEgIzI2LTAz"}
        ],
        "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1"
        },
    "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56TWV0YVBhbmFjZWEgIzAtMDE=": {
            "asset": "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
            "deadline": 1658300654171,
            "guarantee": 4912000000,
            "offerAmount": 400000000,
            "offerAsset": "0x85deac50febfd93988d3f391dea54e8289e43e9e",
            "originOwner": "0x58456204c30be1bb479b301046c3932712c676bc",
            "tokenid": "TWV0YVBhbmFjZWEgIzAtMDE=",
            "usdAmount": 1,
            "user": "0xfa03cb7b40072c69ca41f0ad3606a548f1d59966"
        },
        "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56TWV0YVBhbmFjZWEgIzIwLTAx": {}
    },
    "error": null
}
```

## 19.GetNFTActivityByAsset(Asset,Market,State）

根据系列获取不同状态事件历史记录

State: offers       

​            sales  

​           listings

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTActivityByAsset",
  "params": {
      "Asset":"0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
      "Market":"0xc198d687cc67e244662c3b9c1325f095f8e663b1",
      "State":"offers",
      "Limit": 6,
      "Skip":0
      },
  "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "asset": "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
                "auctionAmount": "100000000",
                "auctionAsset": "0x85deac50febfd93988d3f391dea54e8289e43e9e",
                "event": "Offer",
                "from": "0xf0a33d62f32528c25e68951286f238ad24e30032",
                "image": "https://http.fs.neo.org/GD5YUdHWFQfSVmSzgZ55y9akuqHQ8oXVhXnArtv1fLKr/DTy3nj1wba3Nps8tQDjntKEVbNQjtsGB6DsLLZhfYZJc",
                "market": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
                "name": "MetaPanacea #19",
                "nonce": 204,
                "state": "offer_expired",
                "timestamp": 1660214173878,
                "to": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                "tokenid": "TWV0YVBhbmFjZWEgIzE5LTAy"
            },
            ......
        ],
        "totalCount": 6
    },
    "error": null
}
```

## 20.GetMarketDayVolumeByAsset(AssetHash,LastDays）

获取近任意天 的二级市场上的交易量

 

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetMarketDayVolumeByAsset",
  "params": {      
      "AssetHash":"0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
      "LastDays":7     
       },
  "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": [
        {
            "_id": "62fe0b7d3c6294484cdab024",
            "asset": "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
            "avgPrice": "100000000",
            "date": 1660176000000,
            "dayAmount": 2,
            "dayVolume": "200000000"
        },
       ......
    ],
    "error": null
}
```

## 21.GetMarketFloorPriceByAsset(AssetHash,Market）

获取地板价订单

 

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetMarketFloorPriceByAsset",
  "params": {    
      "AssetHash":"0x6a2893f97401e2b58b757f59d71238d91339856a",
      "Market":"0xc198d687cc67e244662c3b9c1325f095f8e663b1"
       },
  "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": {
        "_id": "62ff3496520d37da2b200860",
        "amount": "1",
        "asset": "0x6a2893f97401e2b58b757f59d71238d91339856a",
        "auctionAmount": "100000000",
        "auctionAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
        "auctionType": 1,
        "auctor": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
        "bidAmount": "0",
        "bidder": null,
        "deadline": 1661583509751,
        "market": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
        "owner": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
        "timestamp": 1660892309751,
        "tokenid": "QkxPQ0sgIzAx",
        "usdAmount": "1"
    },
    "error": null
}
```

## 22.GetMarketOrdersByPrice(AssetHash,MarketHash,Token,MinAmount,MaxAmount）

给一个最小价格min，最大价格max。返回符合这个价格区间的所有订单。

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetMarketOrdersByPrice",
  "params": {    
      "AssetHash":"0x6a2893f97401e2b58b757f59d71238d91339856a",
      "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1",
      "Token":"0xd2a4cff31913016155e38e474a2c06d08be276cf",
      "MinAmount":100000000,
      "MaxAmount":1200000000
       },
  "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": [
        {
            "_id": "62ff3496520d37da2b200860",
            "amount": "1",
            "asset": "0x6a2893f97401e2b58b757f59d71238d91339856a",
            "auctionAmount": "100000000",
            "auctionAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
            "auctionType": 1,
            "auctor": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
            "bidAmount": "0",
            "bidder": null,
            "deadline": 1661583509751,
            "market": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
            "owner": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
            "timestamp": 1660892309751,
            "tokenAmount": "1e+08",
            "tokenid": "QkxPQ0sgIzAx",
            "usdAmount": "2.577495928670139"
        },
        .......
   
    ],
    "error": null
}
```

## 23.GetMarketCheapOrdersByAsset(AssetHash,MarketHash,Number）

给一个数字X，返回地板价及地板价往上X个订单

 

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetMarketCheapOrdersByAsset",
  "params": {    
      "AssetHash":"0x6a2893f97401e2b58b757f59d71238d91339856a",
      "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1",
      "Number":2
       },
  "id": 1
}
```

**Response**

```
{
    "id": 1,
    "result": [
        {
            "_id": "62ff3496520d37da2b200860",
            "amount": "1",
            "asset": "0x6a2893f97401e2b58b757f59d71238d91339856a",
            "auctionAmount": "100000000",
            "auctionAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
            "auctionType": 1,
            "auctor": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
            "bidAmount": "0",
            "bidder": null,
            "deadline": 1661583509751,
            "market": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
            "owner": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
            "timestamp": 1660892309751,
            "tokenid": "QkxPQ0sgIzAx",
            "usdAmount": "2.577495928670139"
        },
       .......
    ],
    "error": null
}
```

## 24.SetMarketCollectionWhitelist(MarketHash,MarketHash）

设置广告位白名单 

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "SetMarketCollectionWhitelist",
  "params": {
      "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1",
      "ContractHash":["0x9f344fe24c963d70f5dcf0cfdeb536dc9c0acb3a",
                      "0x50ac1c37690cc2cfc594472833cf57505d5f46de",
                      "0x6a2893f97401e2b58b757f59d71238d91339856a",
                      "0xaecbad96ccc77c8b147a52e45723a6b5886454e0",
                      "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56"
      ]
      },
  "id": 1
}
```

 **Response**

```
{
    "id": 1,
    "result": {
        "msg": "Insert document done!"
    },
    "error": null
}
```

## 25.SetMarketCollectionWhitelist(MarketHash,ContractHash）

设置广告位白名单 

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "SetMarketCollectionWhitelist",
  "params": {
      "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1",
      "ContractHash":["0x9f344fe24c963d70f5dcf0cfdeb536dc9c0acb3a",
                      "0x50ac1c37690cc2cfc594472833cf57505d5f46de",
                      "0x6a2893f97401e2b58b757f59d71238d91339856a",
                      "0xaecbad96ccc77c8b147a52e45723a6b5886454e0",
                      "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56"
      ]
      },
  "id": 1
}
```

 **Response**

```
{
    "id": 1,
    "result": {
        "msg": "Insert document done!"
    },
    "error": null
}
```

## 26.GetMarketCollections(MarketHash）

获取广告位NFT 列表信息

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetMarketCollections",
  "params": {
      "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1"     
       },
  "id": 1
}
```

  **Response**

```
{
    "id": 1,
    "result": {
         {
                "NFTList": [
                    {
                        "_id": "6352b07ce163e43d05df236d",
                        "amount": "1",
                        "asset": "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
                        "auctionAmount": "0",
                        "auctionAsset": null,
                        "auctionType": 0,
                        "auctor": null,
                        "bidAmount": "0",
                        "bidder": null,
                        "buyNowAmount": 0,
                        "buyNowAsset": 0,
                        "currentBidAmount": 0,
                        "currentBidAsset": 0,
                        "deadline": 0,
                        "image": "https://img.megaoasis.io/testnet/images/0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56/https:http.fs.neo.orgGD5YUdHWFQfSVmSzgZ55y9akuqHQ8oXVhXnArtv1fLKr3TgwkYd75WcfhefoNdw4zMLn1aJY1JCRLFGsuEV1Le5r",
                        "lastSoldAmount": 0,
                        "lastSoldAsset": 0,
                        "market": null,
                        "offerAmount": 0,
                        "offerAsset": 0,
                        "owner": "0x58456204c30be1bb479b301046c3932712c676bc",
                        "state": "notlist",
                        "thumbnail": "https://img.megaoasis.io/testnet/thumbnail/0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56/https:http.fs.neo.orgGD5YUdHWFQfSVmSzgZ55y9akuqHQ8oXVhXnArtv1fLKrCFhEqXK7xgCNduHiJBAiAuDZfYQTrfpZRkArbSgJzn51",
                        "timestamp": 1656561332794,
                        "tokenid": "TWV0YVBhbmFjZWEgIzAtMDE="
                    },
                    ...
                    ]
            }
    },
    "error": null
}
```

## 27.GetNFTList(SecondaryMarket，PrimaryMarket，NFTstate，Sort，Order，Skip，Limit）

获取NFT列表信息（含分组）

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTList",
  "params": {   
      "SecondaryMarket":"0xc198d687cc67e244662c3b9c1325f095f8e663b1",
      "PrimaryMarket":"0x6f1ef5147a00ebbb7de1cf82420485674c5c55bc",
      "NFTstate":"sale",
      "Sort":"timestamp",
      "Order":-1,   
      "Skip":0,
      "Limit":12
    
       },
  "id": 1
}
```

  **Response**

```
{
    "id": 1,
    "result": {
         {
                "NFTList": [
                     {
                "_id": "6353b45510706edbb0a2b081",
                "amount": "1",
                "asset": "0x6a2893f97401e2b58b757f59d71238d91339856a",
                "auctionAmount": "332200000000",
                "auctionAsset": "0x85deac50febfd93988d3f391dea54e8289e43e9e",
                "auctionType": 1,
                "auctor": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                "bidAmount": "0",
                "bidder": null,
                "buyNowAmount": 332200000000,
                "buyNowAsset": "0x85deac50febfd93988d3f391dea54e8289e43e9e",
                "class": "R0VORSBPRiBQT0xF",
                "currentBidAmount": 0,
                "currentBidAsset": 0,
                "deadline": 1668160127033,
                "image": "https://http.fs.neo.org//4x66NBAPzkS6EbvH3JjcfFEGPGbc5j1cUptyNiWP5RzX/3pdsXkoKHrNnM5p1TPHgLCUCTSStkmatN29jiiwEiGaK",
                "lastSoldAmount": 0,
                "lastSoldAsset": 0,
                "market": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
                "number": 2,
                "offerAmount": 0,
                "offerAsset": 0,
                "owner": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
                "properties": {
                    "asset": "0x6a2893f97401e2b58b757f59d71238d91339856a",
                    "class": "R0VORSBPRiBQT0xF",
                    "image": "https://http.fs.neo.org//4x66NBAPzkS6EbvH3JjcfFEGPGbc5j1cUptyNiWP5RzX/3pdsXkoKHrNnM5p1TPHgLCUCTSStkmatN29jiiwEiGaK",
                    "name": "BLOCK #02",
                    "number": 2,
                    "series": "R0VORSBPRiBQT0xF",
                    "supply": "NQ==",
                    "thumbnail": "aHR0cHM6Ly9odHRwLmZzLm5lby5vcmcvLzR4NjZOQkFQemtTNkVidkgzSmpjZkZFR1BHYmM1ajFjVXB0eU5pV1A1UnpYLzk5VWZIMkMzZnh0YWpVanI0QUhCRGV6eldETGtUWVVhU3E2ZDRHeG9mQjFN",
                    "tokenid": "QkxPQ0sgIzAy"
                },
                "state": "sale",
                "thumbnail": "aHR0cHM6Ly9odHRwLmZzLm5lby5vcmcvLzR4NjZOQkFQemtTNkVidkgzSmpjZkZFR1BHYmM1ajFjVXB0eU5pV1A1UnpYLzk5VWZIMkMzZnh0YWpVanI0QUhCRGV6eldETGtUWVVhU3E2ZDRHeG9mQjFN",
                "timestamp": 1667468927033,
                "tokenid": "QkxPQ0sgIzAy"
            },
                    ...
                    ]
            }
    },
    "error": null
}
```

## 27.GetNFTByAssetClass(Asset，Series，Limit，Skip）

根据class获取分组列表   class 字段为GetNFTList  接口返回的class 字段

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTByAssetClass",
  "params": {
      "Asset":"0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
      "Series":"U3Bpcml0",
      "Limit":1,
      "Skip":0  
       },
  "id": 1
}
```

  **Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "_id": "6353552e10706edbb098f7ba",
                "amount": "1",
                "asset": "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
                "auctionAmount": "200000000",
                "auctionAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
                "auctionType": 1,
                "auctor": "0xf0a33d62f32528c25e68951286f238ad24e30032",
                "bidAmount": "0",
                "bidder": null,
                "buyNowAmount": 0,
                "buyNowAsset": 0,
                "class": "U3Bpcml0",
                "currentBidAmount": 0,
                "currentBidAsset": 0,
                "deadline": 1661138196371,
                "image": "https://img.megaoasis.io/testnet/images/0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56/https:http.fs.neo.orgGD5YUdHWFQfSVmSzgZ55y9akuqHQ8oXVhXnArtv1fLKrDTy3nj1wba3Nps8tQDjntKEVbNQjtsGB6DsLLZhfYZJc",
                "lastSoldAmount": 0,
                "lastSoldAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
                "market": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
                "name": "MetaPanacea #19",
                "offerAmount": 0,
                "offerAsset": 0,
                "owner": "0xf0a33d62f32528c25e68951286f238ad24e30032",
                "series": "U3Bpcml0",
                "state": "notlist",
                "supply": "Mg==",
                "thumbnail": "https://img.megaoasis.io/testnet/thumbnail/0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56/https:http.fs.neo.orgGD5YUdHWFQfSVmSzgZ55y9akuqHQ8oXVhXnArtv1fLKr7aBxMeMW97DT6xXk19AK2AsbgS97W8mcMCvryMq7gRYq",
                "timestamp": 1660706196371,
                "tokenid": "TWV0YVBhbmFjZWEgIzE5LTAy"
            }
        ],
        "totalCount": 12
    },
    "error": null
}
```

## 27.GetInfoByNFT(Asset，Tokenid）



**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetInfoByNFT",
  "params": {
      "Asset":"0x50ac1c37690cc2cfc594472833cf57505d5f46de",
      "Tokenid":["bWluZHkubmVv"]
      
     
       },
  "id": 1
}
```

  **Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "_id": "6357a373e163e43d05fe7706",
                "amount": "1",
                "asset": "0x50ac1c37690cc2cfc594472833cf57505d5f46de",
                "auctionAmount": "100000000",
                "auctionAsset": "0x85deac50febfd93988d3f391dea54e8289e43e9e",
                "auctionType": 1,
                "auctor": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                "bidAmount": "0",
                "bidder": null,
                "buyNowAmount": 0,
                "buyNowAsset": 0,
                "currentBidAmount": 0,
                "currentBidAsset": 0,
                "deadline": 1667722166576,
                "lastSoldAmount": 0,
                "lastSoldAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf",
                "market": "0xc198d687cc67e244662c3b9c1325f095f8e663b1",
                "offerAmount": 0,
                "offerAsset": 0,
                "owner": "0x6fd49ab2f14a6bd9a060bb91fdbf29799a885a9e",
                "state": "notlist",
                "timestamp": 1667462966576,
                "tokenid": "bWluZHkubmVv"
            }
        ],
        "totalCount": 1
    },
    "error": null
}
```

## 28.GetCollectionsByAsset(MarketHash，Assets）

根据asset 获取hash信息 （艺术家）

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetCollectionsByAsset",
  "params": {
      "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1",
      "Assets":["0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56"]     
      },
  "id": 1
}
```

  **Response**

```
{
    "id": 1,
    "result": {
        "result": [
            {
                "NFTList": [
                    {
                        "_id": "6352b07ce163e43d05df236d",
                        "amount": "1",
                        "asset": "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56",
                        "auctionAmount": "0",
                        "auctionAsset": null,
                        "auctionType": 0,
                        "auctor": null,
                        "bidAmount": "0",
                        "bidder": null,
                        "buyNowAmount": 0,
                        "buyNowAsset": 0,
                        "currentBidAmount": 0,
                        "currentBidAsset": 0,
                        "deadline": 0,
                        "image": "https://img.megaoasis.io/testnet/images/0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56/https:http.fs.neo.orgGD5YUdHWFQfSVmSzgZ55y9akuqHQ8oXVhXnArtv1fLKr3TgwkYd75WcfhefoNdw4zMLn1aJY1JCRLFGsuEV1Le5r",
                        "lastSoldAmount": 0,
                        "lastSoldAsset": 0,
                        "market": null,
                        "offerAmount": 0,
                        "offerAsset": 0,
                        "owner": "0x58456204c30be1bb479b301046c3932712c676bc",
                        "state": "notlist",
                        "thumbnail": "https://img.megaoasis.io/testnet/thumbnail/0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56/https:http.fs.neo.orgGD5YUdHWFQfSVmSzgZ55y9akuqHQ8oXVhXnArtv1fLKrCFhEqXK7xgCNduHiJBAiAuDZfYQTrfpZRkArbSgJzn51",
                        "timestamp": 1656561332794,
                        "tokenid": "TWV0YVBhbmFjZWEgIzAtMDE="
                    },
                   ...
        ],
        "totalCount": 1
    },
    "error": null
}
```

## 29.GetCountNFTList(ContractHash，SecondaryMarket,PrimaryMarket）

根据nft列表总量

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetCountNFTList",
  "params": { 
      "ContractHash":"",  
      "SecondaryMarket":"0xc198d687cc67e244662c3b9c1325f095f8e663b1",
      "PrimaryMarket":"0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56"
    
       },
  "id": 1
}
```

  **Response**

```
{
    "id": 1,
    "result": {
        "auction": 2,
        "sale": 3
    },
    "error": null
}
```

## 30.SetPrimaryMarketPreSaleWhitelist(MarketHash，Address）

设置一级市场预售白名单

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "SetPrimaryMarketPreSaleWhitelist",
  "params": {
      "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1",
      "Address":["0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56"]
     
       },
  "id": 1
}
```

  **Response**

```
{
    "id": 1,
    "result": {
        "msg": "Insert document done!"
    },
    "error": null
}
```

## 31.GetPrimaryMarketPreSaleWhitelist(MarketHash）

获取一级市场预售白名单

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetPrimaryMarketPreSaleWhitelist",
  "params": {
      "MarketHash":"0xc198d687cc67e244662c3b9c1325f095f8e663b1"      
     
       },
  "id": 1
}
```

  **Response**

```
{
    "id": 1,
    "result": {
        "PreSaleWhitelist": [
            "0x4fb2f93b37ff47c0c5d14cfc52087e3ca338bc56"
        ],
        "_id": "637b18d5e9cedd7c8b5ed968",
        "market": "0xc198d687cc67e244662c3b9c1325f095f8e663b1"
    },
    "error": null
}
```

