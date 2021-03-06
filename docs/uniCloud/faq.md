### uniCloud和微信小程序云开发、支付宝小程序云开发有何区别？

微信、支付宝、百度的小程序，均提供了云开发。但它们都仅支持自家小程序，无法在其他端使用。

`uniCloud`和微信小程序云开发、支付宝小程序云开发使用相同的基础建设平台，微信小程序云开发背后是腾讯云的TCB团队，支付宝小程序云开发背后是阿里小程序云团队。`uniCloud`是DCloud和阿里小程序云团队、腾讯云的TCB团队直接展开深层次合作，在他们底层资源的基础上进行二次封装，提供的跨端云开发方案。

简单来说，uniCloud和微信小程序云开发、支付宝小程序云开发一样稳定健壮，但可以跨更多平台。不管你在uniCloud里选择了阿里还是腾讯的serverless，均可以跨端使用。

### uniCloud稳定吗？DCloud服务器异常会影响我的线上业务吗？

`uniCloud`是 DCloud 和阿里云、腾讯云等成熟云厂商合作推出的云服务产品，阿里云、腾讯云等提供云端基础资源，DCloud提供API设计、前端框架、IDE工具支持、管理控制台、插件生态等服务，开发者的云函数直接托管在阿里云等服务商平台。

用户终端上的应用在运行时，直连云服务商平台，不会经过DCloud服务器，开发者无需担心因DCloud服务器负载而影响自己业务的问题。

### 云函数 和 传统 Node.js 开发有何区别？

云函数相当于 Node.js + Serverless + DCloud改进。
- 传统Node.js开发需要购买服务器，安装Node.js环境，部署 pm2 等守护进程；云函数无需考虑服务器环境，只需专心实现业务代码，然后将云函数一键上传，云服务商负责云函数运行环境的准备。
- 传统Node.js开发模式，开发者需监控服务器参数，比如硬盘使用率，避免服务器负载过高导致业务中断；云函数模式下，开发者无需关心云函数运行的宿主环境，云厂商会实现服务调配及硬件监控。
- 用户量较大时，传统Node.js开发需考虑购买更多服务器并实现负载均衡；云函数模式下，云服务商自动弹性扩容，开发者无需担心服务器扛不住压力。
- 传统Node.js开发模式，需考虑安全防护，比如DDos攻击；云函数模式，云厂商的API网关会做拦截防护，开发者无需关心，并可节省高防IP等费用

总结一下，前端同学即便可熟练编写Node.js代码，但对于DB优化、弹性扩容、攻击防护、灾备处理等方面还是有经验欠缺的，但`uniCloud`将这些都封装好了，真正做到仅专注业务实现，其它都委托云厂商服务。

另外，在 Node.js 代码实现上，云函数每次执行的宿主环境（可简单理解为虚拟机或服务器硬件）可能相同，也可能不同，因此传统`Node.js`开发中将部分信息存储本地硬盘或内存的方案就不再适合，建议通过云数据库或云存储的方案替代。

### uniCloud只支持uni-app，怎么开发web界面？

uni-app本来也可以开发web界面，只是内置组件对宽屏没有自动适配而已。你可以：
1. 新建uni-app项目，但不使用内置组件，而是直接用三方ui库，比如elementUI。这些基于vue的、适合宽屏使用的ui库可以直接用。至于js api，仍然使用uni的，比如uni.setStorage等。有多个可参考插件[GraceAdmin](https://ext.dcloud.net.cn/plugin?id=1347)、[基于elementUI的uniCloud示例](https://ext.dcloud.net.cn/plugin?id=1585)，均是基于uniCloud的pc端管理后台框架。
2. 继续使用内置组件，自己处理pc适配：
    - 如果要多端适配界面，使用css的媒体查询处理适配。
    - 2.6.3起，uni内置组件支持了pc鼠标的滚动和drag。老版可以使用三方库替换touch的拖动为pc上的drag，比如touch-emulator.js。
    - uni-app的内置组件和api仅适配了webkit内核浏览器，ie和firefox可能有兼容问题。如有问题需自己写额外css或js适配。

后续DCloud会进一步强化内置组件和uni-ui对PC浏览器的适配。

### 可否通过http url方式访问云函数或云数据库？

- 场景1：比如App端微信支付，需要配服务器回调地址，此时需要一个HTTP URL。
- 场景2：非uni-app开发的系统，想要连接uniCloud，读取数据，也需要通过HTTP URL方式访问。

uniCloud提供了`云函数URL化`，来满足上述需求。[详见](https://uniapp.dcloud.io/uniCloud/http)

### 微信云开发支持客户端直接操作数据库，uniCloud不支持？
- 安全问题：客户端直接操作数据库，会有安全隐患。微信云开发利用了微信账户是强制登陆的特点，设计了一套基于微信账户的权限，可以让客户端直接操作数据库，但一旦离开微信环境这种方案就用不了。比如App和H5，大多是不需要登录账户也可以使用的。
- 性能问题：客户端直接操作数据库，会造成客户端的sdk体积变大、启动初始化变慢、整体联网消耗流量变大、数据获取权限控制复杂，并不好用。

综上，uni-app放弃了客户端直连数据库，所有数据库操作必须使用云函数。

### 云开发是nodejs+MongoDB组合，对比php+mysql的传统组合怎么样？
nodejs的性能高于php，MongoDB的性能也优于mysql。

对于前端而言，MongoDB这种类json的文档数据库更加易用，且有更高的灵活性。
操作MongoDB仍然使用js的方法，而无需学习sql语句。

对于喜欢传统数据库的开发者而言，仍然可以按传统方式设计数据库表结构。对于希望增加数据冗余以提高性能的开发者而言，nosql数据库则是利器。

php+mysql的优势在于生态，有很多现成的开源项目，可以大幅提高开发效率。而uniCloud将通过插件市场等一系列手段强化生态，给开发者提供更高效率的各种轮子。

### 支持websocket吗？
websocket的实时特性导致serverless化比较复杂。还需要继续寻找合适方案。如果使用三方sdk服务，比如推送、腾讯或声网等实时音视频方案，由于是连接三方服务器，不是连接uniCloud，这些方案仍然可以继续使用。

### 如何导入老数据库的数据？
- 方式1：可以在HBuilderX里用db_init.json来批量创建云数据库和插入表内容，[详见](https://uniapp.dcloud.io/uniCloud/cf-database?id=%e4%bd%bf%e7%94%a8db_initjson%e5%88%9d%e5%a7%8b%e5%8c%96%e9%a1%b9%e7%9b%ae%e6%95%b0%e6%8d%ae%e5%ba%93)
- 方式2：在云函数里，使用nodejs标准写法，连接老数据库，如使用mysql的[插件](https://ext.dcloud.net.cn/plugin?id=1925)，把数据读出来，再批量写入云数据库
- 方式3：将一个云函数URL化，用其他语言读取老数据库，通过http方式提交到云函数，云函数将接收到的数据存入云数据库

### 云函数访问时快时慢怎么回事？

云函数对应的资源，如果长时间不使用，会被阿里云或腾讯云平台从内存中释放。一旦被释放，启动云函数时会有一个冷启动的过程。

表现为：隔了很久不用，第一次用就会比较慢，然后立即访问第二次，则很快，毫秒级响应。

冷启动的速度一般不会超过1.5秒，如超过1.5秒应该是云函数写的有问题或网络有问题。

资源回收策略方面，阿里云是15分钟内没有第二次访问的云函数，就会被回收。腾讯云是半小时。

两家云厂商仍然在优化这个问题。目前如果开发者在意这个问题，给开发者的建议是：
1. 非高频访问的云函数，合并到高频云函数中。有的开发者使用纯单页方式编写云函数，即在一个云函数中通过路由处理实现了整个应用的所有后台逻辑。参考[插件](https://ext.dcloud.net.cn/search?q=%E8%B7%AF%E7%94%B1&cat1=7&orderBy=UpdatedDate)
2. 非高频访问的云函数，可以通过定时任务持续运行它（注意阿里云的定时任务最短周期大于资源回收周期）
3. 向service@dcloud.io发邮件，申请预留资源不销毁

### 发布H5时还得自己找个服务器部署前端网页，可以不用自己再找服务器吗？

已支持[前端网页托管](https://uniapp.dcloud.io/uniCloud/hosting)。

### uniCloud云数据库如何实现全文检索

查询数据时可以传入正则表达式进行查询，详情请参考[正则表达式查询](https://uniapp.dcloud.io/uniCloud/cf-database?id=regexp)

### uniCloud内如何使用formdata

nodejs本身不支持formdata，但是可以通过手动拼装formdata的方式来进行支持，[参考](https://www.npmjs.com/package/form-data)

结合`uniCloud.httpclient.request`使用的示例

```js
const FormData = require('form-data');
let form = new FormData();
form.append('my_field', 'my value');
form.append('my_buffer', new Buffer(10));

form.append('img', new Buffer(10), {
  filename: `${Date.now()}.png`,
  contentType: 'image/png'
})

uniCloud.httpclient.request('https://example.com',{
  content: form.getBuffer(),
  headers: form.getHeaders()
})
```

### 腾讯、阿里的serverless有什么大案例？

- 微信小程序云开发，已经有50万开发者，包括腾讯自有的很多大日活应用都构建在腾讯云serverless上，如微信生活缴费、乘车码、微信读书、腾讯新闻、腾讯相册等。
- 2019年双11，阿里部分业务已经迁移在serverless上。支付宝小程序也提供了云开发功能。

### uniCloud费用贵不贵？

目前uniCloud计费系统还未开发完毕，开发者目前可以免费使用uniCloud。未来uniCloud的商用定价，也会低于租用传统云主机的费用。

开发者无需顾忌DCloud会先免费后收割：
1. DCloud提供uniCloud的目标，就是给开发者更低门槛、更便宜的云开发能力，未来商用时的定价一定比传统云主机便宜，否则就失去做这个产品的意义；
2. serverLess的成本天然低于传统云主机；
3. DCloud是有信誉的大厂，拥有500万开发者和十亿手机活跃用户，多年来一直给开发者提供良心产品，从不失信。
4. 即便uniCloud计费后，也会长期给开发者提供2个免费的服务空间。

uniCloud免费期间，为避免资源滥用，有使用限制，见下。

**阿里云免费版限制如下**

|资源类目						|限制						|说明	|
|:-:							|:-:						|:-:	|
|云函数并发限制	|1000个/服务空间|-		|
|每个服务空间的云函数数量				|49个						|其实云函数支持单文件开发模式，即一个云函数内完成众多业务。		|

**腾讯云免费版限制如下**

|资源类别	|子类目						|限制					|说明																																							|
|:-:			|:-:							|:-:					|:-:																																							|
|云函数		|硬件资源用量			|4万GBs/月		|腾讯云最小计费粒度为256MB*100ms，即使用内存固定为256MB，运行时间以100ms为阶梯计算|
|					|外网出流量				|1GB/月				|-																																								|
|					|云函数并发限制		|1000个/云函数|超出此连接数的请求会直接失败。如有需求突破此限制，请发邮件到service@dcloud.io申请|
|					|云函数数目				|50个					|其实云函数支持单文件开发模式，即一个云函数内完成众多业务。														|
|云存储		|容量							|3GB					|-																																								|
|					|下载操作次数			|150万/月			|-																																								|
|					|上传操作次数			|60万/月			|-																																								|
|					|CDN回源流量			|5GB/月				|-																																								|
|CDN			|CDN流量					|4GB/月				|-																																								|
|云数据库	|容量							|2GB					|-																																								|
|					|读操作数					|5万次/天			|-																																								|
|					|写操作数					|3万次/天			|-																																								|


无论阿里云或腾讯云，如有需求突破资源限制，请发邮件到service@dcloud.io请求协助。如果属于标杆案例，可以申请特批免费；其他情况，可以手工付费。
