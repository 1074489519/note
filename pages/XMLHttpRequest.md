- ```js
  function getJSON(url) {
    return new Promise(function (resolve, reject) {
      var xhr = new XMLHttpRequest()
      xhr.open("GET", url, true)
      xhr.onreadystatechange = function () {
        if (this.readyState === 4) {
          if (this.status === 200) {
            resolve(this.response)
          } else {
            reject(new Error(this.statusText))
          }
        }
      }
      xhr.onerror = function () {
        reject(new Error(this.statusText))
      }
      xhr.responseType = "json"
      xhr.setRequestHeader("Accept", "application/json")
      xhr.send(null)
    })
  }
  ```