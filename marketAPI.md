[TOC]



## 1.GetNftMarket(ContractHash,AssetHash,NFTState)

**NftState**:  auction    sale    notlisted  

**Sort** :  timestamp    price     

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
      "NFTstate":"auction",
      "Sort":"",
      "Order":1,       
      "Skip":0,
      "Limit":4
     
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



##  2. GetNFTOwnedByAddress(Address,ContractHash，AssetHash,NFTstate,Sort,Order,Limit,Skip)

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
      "MarketContractHash": "0xdd58b7a05fd9b58a6ec36d6401a89ff2cda224a2"    
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



## 4.GetNFTRecordByContractHashTokenId(ContractHash,TokenId) 

获取某个Nft在用户之间的历史记录

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTRecordByContractHashTokenId",
  "params": {
      "ContractHash":"0xc7b11b46f97bda7a8c82793841abba120e96695b",   
      "TokenId":"BoN2dx2fSFeRuT7kp87u3e1Jewc3ZIqQ5U0dQSdxofA="  
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



## 5.GetBidInfoByNFT(AssetHash,TokenId)

获取NFT所有出价记录信息

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetBidInfoByNFT",
  "params": {
      "Address":"",
      "AssetHash": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
      "TokenId":"az2dNYa7xEzk2XAQoHnH22k6AbO5/RkyqMDK64VuuXE="                      
  
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



## 6.GetNFTByContractHashTokenId(ContractHash,TokenIds)

通过ContractHash 和Tokenid 获取指定NFT的信息

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetNFTByContractHashTokenId",
  "params": {
      "ContractHash":"0xc7b11b46f97bda7a8c82793841abba120e96695b",     
      "TokenIds":["LzKk2aeLybZTv83Hzw8djcvJJyVldIyi8oly1qqmqUo="]
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



## 7.GetAllBidInfoByNFT(AssetHash,TokenId)

获取指定NFT所有历史竞价信息记录

**Request**

```
{
  "jsonrpc": "2.0",
  "method": "GetAllBidInfoByNFT",
  "params": {      
      "AssetHash": "0xc7b11b46f97bda7a8c82793841abba120e96695b",
      "TokenId":"b7mzAd/hhpBYX95Gq8eJwkoZdS9JssMHHhJztAQNCKs="  
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

