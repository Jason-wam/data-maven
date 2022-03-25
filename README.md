### 资源猫自定义 数据源|直播源

##### 数据格式
```
{
  "name": "直播源大全", //数据源名称
  "cover": "http://inews.gtimg.com/newsapp_bt/0/13265109400/1000", //数据源图片
  "type": "2022-03-06 / 直播源 / 混合资源", //数据源信息（可空）
  "description":"道德三皇五帝，功名夏侯商周。", //数据源简介（可空）
  "playlist": [ //可以是多个对象，id必须是唯一的
    {
      "id": "1", //播放分组ID，必须设置
      "name": "播放列表1", //播放分组名称
      "isLive": true, //不是直播时不用添加，默认为false
      "entities": [
        {
          "id": 1, //ID必须唯一
          "name": "CCTV1",
          "type": "Play",   //播放链接类型: <Play,Sniff,Decode,Cloud> , 具体类型介绍请看下文<播放链接类型>介绍
          "address": "http://39.135.138.60:18890/PLTV/88888910/224/3221225618/index.m3u8"
        },
        {
          "id": 2,
          "name": "我要幸福",
          "type": "Sniff",
          "address": "https://video.weibo.com/show?fid=1034:4711863733387391",
          "sniffRules": [   //嗅探规则，可以省略，如果媒体格式特殊可以自行添加额外规则
			 "start:http > contain:video.weibocdn.com > contain:.mp4"  //匹配规则，具体看下文<嗅探规则>介绍
		   ]
        }
      ]
    },
    {
      "id": "2",
      "name": "播放列表2",
      "isLive": true,
      "entities": [
        //同上，可以是多个
      ]
    }
  ]
}
```
##### 示例文件
[点击查看直播源示例](https://raw.githubusercontent.com/miantiaox/data-maven/master/%E6%B7%B7%E5%90%88%E7%9B%B4%E6%92%AD%E6%BA%90.cat.json)
<br/>
[点击查看嗅探源示例](https://raw.githubusercontent.com/miantiaox/data-maven/master/%E9%83%AD%E5%BE%B7%E7%BA%B2%E7%9B%B8%E5%A3%B0%E9%9B%86.cat.json)
...
<br/>
##### 播放链接类型
```
Play: 直接调用APP内部播放器播放

Sniff:APP自动嗅探网站播放页视频,嗅探规则请看下文

Cloud:直接调用APP内部浏览器打开播放

Decode:
需要解析接口支持，
<链接格式    ： http://解析接口/url?=视频地址> 
<成功JSON格式： {code:200    , url="播放地址"} > //成功必须返回200
<失败JSON格式： {code:错误码 , msg:"错误信息"} >
```

##### 嗅探规则
###### 01.举个栗子：
```
规则             ->>>  "start:http > contain:video.weibocdn.com > contain:.mp4"
匹配对应媒体链接 ->>>  https://f.video.weibocdn.com/VZ8y4bATlx07MWiZlXuo0104120j4ZMY0E070.mp4
规则解析         ->>>  链接必须以http开头，链接必须包含video.weibocdn.com，链接必须包含.mp4
```
###### 02.再举个栗子
```
规则              ->>>  "host:www.baidu.com > contain:video > end:.mp4"
匹配对应媒体链接  ->>>  https://www.baidu.com/video/VZ8y4bATlx07MWiZlXuo0104120j4ZMY0E070.mp4
规则解析          ->>>  链接域名必须是www.baidu.com，链接必须包含video，链接必须包含.mp4
```
###### 
```
host:      ->>匹配指定域名
start:     ->>匹配指定开头
end:       ->>匹配指定结尾
contain:   ->>匹配包含指定字符串
clear:     ->> <clear: 字符串,clear: 正则表达式，用于清除指定字符串，如果放在开头则匹配清除后的链接，如果放在结尾则清除匹配到的链接中的指定字符串>

!host:     ->>排除指定域名
!start:    ->>排除指定开头
!end:      ->>排除指定结尾
!contain:  ->>排除包含指定字符串

PS: 每个规则必须以 > 号分割
PS: 软件优先执行嗅探规则，如果没有填写规则软件将自动匹配媒体

//每个规则都可以是多次使用的，例如可以是多个contain ->>> contain:video.weibocdn.com > contain:.mp4
//规则也可以加上!<小写感叹号>号取反：例如 ->> !contain ,代表不能包含指定字符串。!end ,代表不能以某字符串开头，!host排除指定域名
```

##### 数据编写完毕后,如何将数据导入资源猫
```
方案1: 请将文件名修改为 xxx.cat.json 即可直接调用资源猫导入,必须以 .cat.json 结尾
方案2: 网页导入http链接 <a href="outside://www.baidu.com/test.cat.json">点击导入</a>
方案3: 网页导入https链接 <a href="outsides://www.baidu.com/test.cat.json">点击导入</a>

PS: 方案2和方案3需要将http修改为outside，https修改为outsides
PS: 可以直接发送文件给好友导入，例如：手机QQ选择使用其它应用打开
```

[测试导入](https://miantiaox.github.io/data-maven/intent.html)

