# 本仓库已经过期

为了更好的管理，本仓库已经被迁移到：[https://github.com/wxpusher/wxpusher-docs](https://github.com/wxpusher/wxpusher-docs)

欢迎提交PR，一起完善文档。

# 介绍

> 什么是WxPusher

**WxPusher** (微信推送服务)是一个使用微信公众号作为通道的，实时信息推送平台，你可以通过调用API的方式，把信息推送到微信上，无需安装额外的软件，即可做到信息实时通知。
你可以使用**WxPusher**来做服务器报警通知、抢课通知、抢票通知，信息更新提示等。
# demo演示程序
 
你可以访问演示程序，体验功能：[http://wxpusher.zjiecode.com/demo/](http://wxpusher.zjiecode.com/demo/)

# 效果预览
类型|获取用户ID|普通发送|带链接的消息|长文本消息|模版消息
:--:|:--:|:--:|:---:|:---:|:---:
预览|![创建应用](imgs/getid.jpg  ':size=250')|![创建应用](imgs/type_1.jpg  ':size=250')|![创建应用](imgs/type_2.jpg  ':size=250')|![创建应用](imgs/type_3.jpg  ':size=250')|![创建应用](imgs/type_4.jpg  ':size=250')

# 名词解释
- 应用

对应你的一个项目 ，主要用来做鉴权，资源隔离等（类比使用高德地图SDK、微信登录等，都会先新建一个应用），每个应用拥有独立的名字，二维码，回调地址，调用资源，鉴权信息等，发送消息第一步，需要先新建一个应用。

简单的理解，你有一个抢火车票的项目，抢到票了需要给用户发送信息；你还有一个服务器报警的项目，服务器有异常的时候，给相关负责人发送信息，这2个的用途是不一样的，你就可以创建2个应用来分别发送他们的信息。

用户可以通过二维码或者链接关注这个应用，关注我们会把用户的UID回调给你指定的服务器，你可以通过UID给这个用户发送信息。
- 主题(Topic)

**目前还在开发中...**

主题(Topic)是应用下面，一类消息的集合，比如创建了一个优惠相关的应用，用来给用户推送各种优惠信息，但是不同的用户关注的优惠信息不同，一部分人关注淘宝的，一部分人关注京东的。这种场景下，你就可以创建一个淘宝的主题，再创建一个京东的主题，发送信息的时候，直接发送到对应的主题即可，每个主题都有对应的订阅链接和二维码，用户订阅这个主题以后，就能接收这个主题下的信息了。

用户关注以后，无回调信息 。
- 应用和主题(Topic)的对比

项目|应用|主题(Topic)
:--:|:--:|:---:
概念|应用是一个独立的个体|主题属于应用，调用主题需要使用对应应用的APP_TOKEN授权
关注方式|二维码和链接|二维码和链接
发送群体|通过UID一对一发送|消息发送主题后，主题再分发给关注主题的用户，属于群发

- 各种二维码

项目|应用二维码|主题二维码
:--:|:--:|:---:
用途|用于微信用户关注应用，用户只有关注了你的应用，<br />你才能拿到他的UID，才能给他发送信息|用于订阅主题，用户订阅主题以后，你不能拿到它的UID
动静态|默认动态二维码|默认动态二维码

**动态二维码**：二维码链接不会变，但是二维码图形会变 ，因此只能使用动态二维码链接，不对截图、打印等。

**静态二维码**：二维码链接和图形都不变，可以随意使用。

- APP_TOKEN

应用的身份标志，这个只能开发者你本人知道 ，拥有APP_TOKEN，就可以给对应的应用的用户发送消息 ，所以请严格保密，不要发送到github之类的地方。
- UID
  
微信用户标志，在单独给某个用户发送消息时，来说明要发给哪个用户。

# 快速接入

## 注册并且创建应用
  
[http://wxpusher.zjiecode.com](http://wxpusher.zjiecode.com) ，使用微信扫码登录，无需注册，新用户首次扫码自动注册。

创建一个应用，如下图：

![创建应用](imgs/create_app.png  ':size=250')

回调地址：可以不填写，不填写用户关注的时候，就不会有回调，你不能拿到用户的UID，参考<a href="#/?id=callback">回调说明</a>。

设置URL：可以不填写，填写以后，用户在微信端打开「我的订阅」，可以直接跳转到这个地址，并且会携带uid作为参数，方便做定制化页面展示。

联系方式：可以不填写，告诉用户，如何联系到你，给你反馈问题。

关注提示：用户关注或者扫应用码的时候发送给用户的提示，你可以不填写，Wxpusher会提供一个默认文案。你也可以在用户关注回调给你UID的时候，再主动推送一个提示消息给用户。

说明：描述一下，你的应用，推送的是啥内容，用户通过链接关注，或者在微信端查看的时候可以看到。
## 扫码关注应用
创建应用以后，你可以看到应用的应用码和关注链接，你可以让你的用户通过下面2种方式来关注你的应用，关注你的应用以后，你就可以给他发送消息了。

![创建应用](imgs/subscribe.png  ':size=350')

## 获取UID
> 老版本的ID，不能在新版本上面使用，需要重新获取。
  
## 发送消息
拿到UID以后，配合应用的appToken，然后调用发送接口发送消息。

# HTTP调用
## 发送消息
- POST接口
  POST接口是功能完整的接口，推荐使用。

  ContentType:application/json
  
  地址：http://wxpusher.zjiecode.com/api/send/message

  请求数据放在body里面，具体参数如下：

  ```json
  {
    "appToken":"AT_xxx",
    "content":"Wxpusher祝你中秋节快乐!",
    "contentType":1,//内容类型 1表示文字  2表示html(只发送body标签内部的数据即可，不包括body标签) 3表示markdown 
    "topicIds":[ //发送目标的topicId，是一个数组！！！
        123
    ],
    "uids":[//发送目标的UID，是一个数组！！！
        "UID_xxxx"
    ],
    "url":"http://wxpusher.zjiecode.com" //原文链接，可选参数
}
  ```
- GET接口
  GET接口是对POST接口的阉割，主要是为了某些情况下调用方便，只支持对文字（contentType=1）的发送，举例：
  ```
  http://wxpusher.zjiecode.com/api/send/message/?appToken=AT_qHT0cTQfLwYOlBV9cJj9zDSyEmspsmyM&content=123&uid=c1BcpqxEbD8irqlGUh9BhOqR2BvH8yWZ&url=http%3a%2f%2fwxpusher.zjiecode.com
  ```
  请求参数：appToken、uid、topicId、content、url ，其中content和url请进行urlEncode编码。

## 查询状态
消息发送给Wxpusher，Wxpusher会缓存下来，后台异步推送给微信再分发给用户，当消息数量庞大的时候，可能会有延迟，你可以根据发送消息返回的messageId，查询消息的发送状态

请求方式：GET

说明：查询消息状态，消息缓存有时效性，目前设置缓存时间为7天，7天后查询消息，可能会返回消息不存在

请求地址：http://wxpusher.zjiecode.com/api/send/query/{messageId}

## 创建参数二维码
有一种场景，就是需要知道当前是谁扫描的二维码，比如：论坛帖子有新消息需要推送给用户，这个如果用户扫码关注，你需要知道是谁扫的二维码，把论坛用户ID和Wxpusher用户的UID绑定，当论坛用户ID有新消息时，推送给Wxpusher用户。这种场景就需要带参数的二维码。

请求方式：POST

请求地址：http://wxpusher.zjiecode.com/api/fun/create/qrcode

ContentType：application/json

说明：创建带参数二维码，用户扫码以后，会在回调里面带上参数，参考<a href="#/?id=callback">回调说明</a>

请求body:

```json
{
    "appToken":"xxx",   //必填，appToken,前面有说明，应用的标志
    "extra":"xxx",      //必填，二维码携带的参数，最长64位
    "validTime":1800    //可选，二维码的有效期，默认30分钟，最长30天，单位是秒
}
 
```

## 查询App的关注用户
你可以通过本接口，分页查询到所有关注你App的微信用户。

请求方式：GET

说明：获取到所有关注应用的微信用户用户信息

请求地址：http://wxpusher.zjiecode.com/api/fun/wxuser

请求参数：
 - appToken 应用密钥标志
 - page  请求数据的页码
 - pageSize 分页大小

返回数据：
```json
{
    "page":1, //当前数据页码
    "pageSize":50, //当前页码大小 
    "records":[
        {
            "createTime":1572755754416, //用户关注时间
            "enable":true, //是否可用，也就是用户是否开启接收消息
            "headImg":"xxxxxx",//用户头像
            "nickName":"0XFF",//用户昵称
            "uid":"xxxxxxx"//用户的UID
        }
    ],
    "total":3//所有的用户数量
}
```


# Java SDK

为了方便快速接入，开发了Java的接入SDK ，[https://github.com/zjiecode/wxpusher-client](https://github.com/zjiecode/wxpusher-client).

目前只封装了Java的版本，其他语言请使用Http，如果你有进行封装，也欢迎PR。

# 回调说明 :id=callback
给用户发送消息，需要知道用户的UID，有2种途径知道用户的UID：
- 用户关注公众号以后，在菜单里面，找到「获取UID」就可以看到自己的UID了。
- 如果你在创建应用的时候，写了回调地址，当用户扫描你的应用二维码关注你创建的应用时，WxPusher会对你设置的地址发起HTTP调用，把用户的UID推送给你。
回调的使用POST方法，数据格式如下：
```json
{
    "action":"app_subscribe",//动作，app_subscribe 表示用户关注应用回调
    "data":{
        "appKey":"AK_xxxxxx", //关注应用的appKey
        "appName":"应用名字",
        "source":"scan", //用户关注渠道，scan表示扫码关注
        "time":1569416451573, //消息发生时间
        "uid":"UID_xxxxxx", //用户uid
        "extra":"xxx"    //用户扫描带参数的二维码，二维码携带的参数。扫描默认二维码为空
    }
}
```


# 迁移升级

> 新版发布以后，鼓励从老版本升级过来，因为新版本体验更好，更稳定。当然，未迁移之前，老版本是可以正常使用的。

- UID说明
  
    升级以后，UID是以**UID_**开头，新UID只能调用新接口，老ID只支持老的接口，无法交叉调用。
- 消息开关
  
    消息开关已经迁移，现在消息开关，只能控制新版本的API，老版本的消息开关，不可修改。
