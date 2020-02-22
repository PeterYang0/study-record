#### react native AsyncStorage 的使用

AsyncStorage 是一个简单的、异步的、持久化的 Key-Value 存储系统，它对于 App 来说是全局性的。这是官网上对它的介绍。可以知道，这个 asyncstorage 也是以键值对的形式进行存储数据的。

- 获取 storage

```
// AsyncStorage.getItem(key, callback)或者AsyncStorage.getItem(key).then(res=>{})
  fetchLocalData(url) {
    return new Promise((resolve, reject) => {
      AsyncStorage.getItem(url, (error, result) => {
        if (!error) {
          try {
            resolve(JSON.parse(result));
          } catch (e) {
            reject(e);
            console.error(e);
          }
        } else {
          reject(error);
          console.error(error);
        }
      });
    });
  }
```
- 更新同上(setItem)

- 删除
```
AsyncStorage.removeItem(key);
```
