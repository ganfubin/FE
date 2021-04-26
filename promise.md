 # promise的三种状态
 1.pengding 初始状态
 2.fulfilled 操作成功
 3.rejected 操作失败
 
 # promise的状态一旦改变就不可逆
 pengding 可转换为fulfilled
 pengding 可转换为rejected




`promise.all 全部成功就reslove，或者有失败就reject`
```javascript
export const promiseAll = (promiseArr) => {
  const  result = [];
  let count = 0
  if (!Array.isArray(promiseArr)) return
  return new Promise((resolve, reject) => {
    for (let i =0; i <promiseArr.length; i++ ) {
      Promise.resolve(promiseArr[i]).then((res) => {
        result[i] = res
        count += 1
        if (result.length === count) {
          return resolve(result)
        }
      }).catch((err) => {
        reject(err)
      })
    }
  })
}
```


`promise.race  成功一个就reslove，或者有失败就reject`
  
 ```javascript
export const promiseRace = (promiseArr) => {
  if (!Array.isArray(promiseArr)) return
  return new Promise((resolve, reject) => {
    for (let i = 0; i < promiseArr.length; i++ ) {
      Promise.resolve(promiseArr[i]).then((res) => {
        return resolve(res)
      }).catch((err) => reject(err))
    }
  })
}
```


`promise.allSettled  返回全部的状态，不管是成功还是失败`
```javascript
export const promiseAllSettled = (promiseArr) => {
  if (!Array.isArray(promiseArr)) return
  const result = [];
  let count = 0;
  return new Promise((resolve) => {
    for (let i = 0; i < promiseArr.length; i++) {
      Promise.resolve(promiseArr[i]).then((res) => {
        result[i] = {status: 'fulfilled', value: res}
        
      }).catch((err) => {
        result[i] = {status: 'rejected', reason: err}
      })
        .finally(() => {
          count += 1
          if (count === result.length) {
            resolve(result)
          }
        })
    }
  })
}
```
