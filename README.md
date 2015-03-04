## JS-SDK
编写日期: 2015.3.4

目的:解决html5在IOS无法分享微信朋友圈问题.

微信JS-SDK官方说明文档
http://mp.weixin.qq.com/wiki/7/aaa137b55fb2e0456bf8dd9148dd613f.html
官方文档很长,下面我为大家把精要总结出来.

### 一. 所需变量名词解释
1. URL:要分享朋友圈的地址.
2. token:通过微信公众号APPID和secret所生成,用于生成ticket.
3. ticket:由token生成出来的票,用于生成签名.
4. timestamp:时间戳,由当前年月日时分秒转换所得,Demo有方法提供实现.
5. nonceStr:随机常量,Demo有方法提供实现.
6. signature:签名,由URL+ticket+timestamp+nonceStr4个变量生成所得.

### 二. Java分享微信朋友圈实现思路
下面html5简称h5,微信简称wx,后端接口程序简称Service.

#### 第1步.Service访问生成token的URL地址:
https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=APPID&secret=APPSECRET
返回token:

	{
	  "access_token": "i5SIDe-EsdBBslFYpOTjBnZLXRyVexmTaru-hgpZykTDNAn5diFlr-a-HAmfn5fXPN5XMOnR0Rd6BwOFF3HgupumyZiEdpQisF7QQmNNq2I",
	  "expires_in": 7200
	}


#### 第2步.通过第1步生成的token,访问生成ticket的URL地址:
https://api.weixin.qq.com/cgi-bin/ticket/getticket?access_token=i5SIDe-EsdBBslFYpOTjBnZLXRyVexmTaru-hgpZykTDNAn5diFlr-a-HAmfn5fXPN5XMOnR0Rd6BwOFF3HgupumyZiEdpQisF7QQmNNq2I&type=jsapi
返回ticket:

	{
	  "errcode": 0,
	  "errmsg": "ok",
	  "ticket": "sM4AOVdWfPE4DxkXGEs8VE8_Ngph1J7_WCVMvfc9RZkXZwMWfrzmlFKHm2-we89MoawU2JXYmnqWd-qmJSeOJw",
	  "expires_in": 7200
	}

根据token获得ticket,返回tikit.
#### 第3步.ticket生成签名
根据前端传过来的URL,和刚生成tikit,timestamp,nonceStr共4个参数,调Demo里面Sign类的的sign方法,生成signature.

	signature: "e0d3425d214a3b5ad2959bebd66dfaa7bdbaf7f5"
	
#### 第4步.最终前端要拿到这个JSON,即可分享微信朋友圈.

	{
	signature: "e0d3425d214a3b5ad2959bebd66dfaa7bdbaf7f5"
	nonceStr: "ff288ada-9531-49d7-974e-506e8462e762"
	timestamp: "1422707482"
	}


### 三. 注意事项
token和ticket有效时间是7200秒,也就是2个小时,访问过多会出现死锁的情况,接口程序要缓存起来.而signature是根据时间戳变化,因此不用缓存..


### 四. 缓存方法建议
我实现的时候使用的是第4个方法,比较土,但能防止服务器down掉的情况.
1. hashmap.
2. session.
3. java定时任务.
4. 存数据库.

最后希望能帮到大家.
