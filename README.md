# 批量抓取网站图片并保存在本地

目标网站：[妹子图](http://www.mzitu.com/)

## 启动项目

```bash
npm i
npm start
```

> 配置文件

```js
// 配置相关
module.exports =  {
  originPath: 'http://www.mzitu.com', // 请求地址
  savePath: 'E:/meizi', // 存放图片路径
  maxPage: 20 // 可爬取的最大页码
}
```

## 技术点

> Axios: 发起 get 请求获得页面或者获得图片 stream

```js
// 获取页面
async getPage (url) {
  return {
    res: await axios.get(url)
  }
}
// 把获取的文件写入本地
await axios({
  method: 'get',
  url: imageSrc,
  responseType: 'stream',
  headers
}).then(function(response) {
  response.data.pipe(fs.createWriteStream(fileName))
})
```

> Cheerio: 覆盖了 jQuery dom 部分核心 API，可操作获取的 dom 对象

```js
// res.data： 获得的文档对象
let list = []
const $ = cheerio.load(res.data)
// 获取所有的相册
$('#pins li a').children().each((index, item) => {
  let album = {
    name: item.attribs.alt, // 相册名称
    url: item.parent.attribs.href // 相册地址
  }
  list.push(album)
})
```

## 爬虫结果截图

<img src="./static/meizi.png" width="600">

## 说明

此爬虫仅用于个人学习，如果侵权，即刻删除！！
