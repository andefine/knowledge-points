# 封装缓存(包含过期时间)

```javascript
/**
 * 设置缓存。
 * days为可选参数，若无，则会一直保存缓存
 *
 * @export
 * @param {*} key
 * @param {*} value
 * @param {*} [days=-1]
 */
export function setStorage (key, value, days = -1) {
  let expires = -1

  if (days > 0) {
    const now = Date.now()
    expires = now + days * 24 * 60 * 60 * 1000
  }

  // 这里并不需要JSON.stringify，小程序的setStorageSync会帮我们做，详细请看文档
  wx.setStorageSync(key, {
    expires,
    value
  })
}

/**
 * 获取缓存。
 * 返回结果的 expired 表示是否已过期，过期为true
 *
 * @export
 * @param {*} key
 * @returns { expired: Boolean, value: any }
 */
export function getStorage (key) {
  const data = wx.getStorageSync(key)

  // 没有该缓存
  if (!data) {
    return {
      expired: false,
      value: ''
    }
  }

  const expires = data.expires
  const value = data.value

  if (expires === -1) {
    return {
      expired: false,
      value
    }
  }

  const now = Date.now()
  if (expires < now) {
    wx.removeStorageSync(key)
    return {
      expired: true,
      value
    }
  }

  return {
    expired: false,
    value
  }
}

```