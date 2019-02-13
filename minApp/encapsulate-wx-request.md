# 封装小程序接口请求方法

```javascript
const baseURL = 'xxx'

const http = {}

/**
 * 接口请求方法
 * 调用时，建议请求路径中的query参数直接放在 config.url 中
 *
 * @param {object} config json对象，键值对如下列出
 * 
 * @param {string} url 路径，必填。不包含baseURL，前面的'/'可写可不写
 * @param {string} method 请求方法，选填。不填则默认GET方法，大小写均可
 * @param {string} baseURL 基础路径，选填。不填则是开发环境或生产环境下的某一个
 * @param {object} header 头信息，选填。
 * @param {object} data 请求体数据，选填
 * 
 * @returns
 */
http.request = function (config) {
  let url = config.url
  let method = config.method || 'GET'
  // 允许调用时使用自定义baseURL
  const base = config.baseURL || baseURL
  const header = config.header || {}
  const dataObj = config.data || {}

  // 调用时路径前面可以写正斜杠，也可以不写
  if (url[0] !== '/') {
    url = '/' + url
  }

  method = method.toUpperCase()

  // 对于这三种请求方法，请求头的content-type不一样
  const urlencodedMethods = ['POST', 'PUT', 'DELETE']
  if (urlencodedMethods.includes(method)) {
    header['content-type'] = 'application/x-www-form-urlencoded'
  }

  // 因为我们的一些请求使用formdata参数形式时，可能会有多个同名参数，
  // 使用小程序默认的转换方式已经不能满足需求，得自己完成
  let dataStr = ''
  if (method === 'GET') {
    dataStr = dataObj
  }
  if (urlencodedMethods.includes(method)) {
    for (let attr in dataObj) {
      if (dataObj[attr] instanceof Array) {
        for (let v of dataObj[attr]) {
          dataStr += `${attr}=${v}&`
        }
        continue
      }
      dataStr += `${attr}=${dataObj[attr]}&`
    }
    dataStr = dataStr.slice(0, dataStr.length - 1)
  }

  return new Promise((resolve, reject) => {
    wx.request({
      url: base + url,
      method,
      header,
      data: dataStr,
      success (res) {
        // 其他的状态码可根据需求做不同处理

        if (res.statusCode === 200) {
          resolve(res.data)
        }
      },
      fail (res) {
        reject(res)
      }
    })
  })
}

export default http
```