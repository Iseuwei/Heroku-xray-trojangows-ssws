### 提示：UUID建议使用UUID生成器，谷歌一下：UUID生成。。。。如果无法连通，八成是UUID不符合规定！！
### UUID传送门： https://www.guidgenerator.com/online-guid-generator.aspx
 
### 提醒： 滥用可能导致账户被删除！！！ 

### 以下内容是根据原作者项目说明进行相应修改，方便初学者小白们理解！

### 详细视频教程YouTube：https://youtu.be/dE730hVgmUs
   
* 原作者的Heroku脚本为多协议共存脚本，该项目使用[xray](https://github.com/XTLS/Xray-core)+caddy，同时部署通过ws传输模式的vmess vless trojan-go shadowsocks socks等协议，默认已配置好伪装网站。  

## 服务端创建操作流程 
[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/YG-tsj/Heroku-xray-trojangows-ssws)  
点击上面紫色`Deploy to Heroku`，会跳转到heroku app创建页面，填上应用程序名、选择节点（美国或者欧洲）、自定义UUID码，其他建议保持默认，点击下面deploy，几秒后搞定！    

## vmess vless trojan-go shadowsocks对应客户端参数的参考如下,末尾带()里的内容仅为提示

## 1：Xray

### 代理协议：vless+ws+tls 或 vmess+ws+tls
* 服务器地址：自选ip（如：icook.tw）
* 端口：443
* 默认UUID：8f91b6a0-e8ee-11ea-adc1-0242ac120002   (务必创建时自定义UUID码)
* 加密：none
* 传输协议：ws
* 伪装类型：none
* 伪装host：****.workers.dev(CF Workers反代地址)
* SNI地址：****.workers.dev(CF Workers反代地址)
* path路径：/自定义UUID码-vless 或 /自定义UUID码-vmess    (注意：前有斜杠/)
* vmess额外id（alterid）：0
* 底层传输安全：tls
* 跳过证书验证：false

## 2：Trojan-Go+ws

* 服务器地址：自选ip（如：icook.tw）
* 端口：443
* 密码：8f91b6a0-e8ee-11ea-adc1-0242ac120002   (务必创建时自定义UUID码) 
* 传输协议：ws
* path路径：/自定义UUID码-trojan  (注意：前有斜杠/)
* SNI地址：****.workers.dev(CF Workers反代地址)
* 伪装host：****.workers.dev(CF Workers反代地址)

## 3：Shadowsocks+ws+tls

* 服务器地址: 应用程序名.herokuapp.com
* 端口: 443
* 密码：8f91b6a0-e8ee-11ea-adc1-0242ac120002   (务必创建时自定义UUID码) 
* 加密：chacha20-ietf-poly1305
* 插件选项: tls;host=应用程序名.herokuapp.com;path=/自定义UUID码-ss


### CloudFlare Workers反代代码（支持VLESS\VMESS\Trojan-Go的WS模式，可分别用两个账号的应用程序名（UUID与path保持一致），单双号天分别执行，那一个月就有550+550小时）

```

const _00 = 'xxxxx00a.herokuapp.com'
const _01 = 'xxxxx01a.herokuapp.com'
const _02 = 'xxxxx02a.herokuapp.com'
const _03 = 'xxxxx03a.herokuapp.com'
const _04 = 'xxxxx04a.herokuapp.com'
const _05 = 'xxxxx05a.herokuapp.com'
const _06 = 'xxxxx06a.herokuapp.com'
const _07 = 'xxxxx07a.herokuapp.com'
const _08 = 'xxxxx08a.herokuapp.com'
const _09 = 'xxxxx09a.herokuapp.com'

addEventListener(
    "fetch",event => {

        let nd = new Date();
        let x = nd.getDate()%2;
        nd = nd.getMinutes()/12;

		if(x)
		{
			switch (nd){
			case 0: host=_01 
			break
			case 1: host=_03
			break
			case 2: host=_05
			break
			case 3: host=_07
			break
			case 4: host=_09
			break
			default: host=_01
			}
		}
		else
		{
			switch (nd){
			case 0: host=_00
			break
			case 1: host=_02
			break
			case 2: host=_04
			break
			case 3: host=_06
			break
			case 4: host=_08
			break
			default: host=_00
			}
		}


        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)

```
### 原作者项目地址：https://github.com/mixool/xrayku

### CF反代代码经过微调，应该有略微提升。
