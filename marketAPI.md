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

## 8.GetNFTClass(AssetHash,SubClass)

获取一级市场的分类

**State:**  mint  : NFT  mint出来但未上架

​            listed  : NFT  上架但未开售

​           selling : NFT  售卖中

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTClass",
  "params": {  
      "AssetHash":"0xc7b11b46f97bda7a8c82793841abba120e96695b",      
      "SubClass":[["VbdQL2cl8ngkJjITK8aNzeY07PLKiEyiXCORcgw+lfI=","sNU/EpLlV1GuiH4P0zet1rz+SlCb1/2YNucEanpVWIA="], ["79WdS6cDK2ZC74UPFlILgiZlus49WkhYo5z8XpR+ckg=","GSDIwJTkjsqbWMQG4eAkPkzCXrTv/390QciVb/B3cow="]],
      "MarketHash":"0xf63cccfe6cfac7ee776dada552b976c74fe5b51a"  //必填      
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
                "claimed": 6,  //已经卖掉的数量
                "image": "",
                "name": "sell-1",
                "price": "5",   //售卖价格
                "sellAsset": "0xd2a4cff31913016155e38e474a2c06d08be276cf"  //售卖资产
            },
            ....
        ],
        "totalCount": 2
    },
    "error": null
}
```

## 9.GetMarketIndexByAsset(MarketHash，AssetHash)

获取二级市场某一系列的指标

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetMarketIndexByAsset",
  "params": {     
      "MarketHash":"0xf63cccfe6cfac7ee776dada552b976c74fe5b51a",
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

## 10.GetMarketTokenidList(MarketHash，AssetHash,SubClass,Limit,Skip)

查询一级市场在售NFT的tokenid 列表

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetMarketTokenidList",
  "params": {
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

