```js
'load' event --> Promise 'resolve' --> async + await


// resolve is the thing to be delayed
let dataA, dataB, dataC;

let xhrA = new XMLHttpRequest();
xhrA.open('GET', urlA);

xhrA.addEventListener('load', () => {
    dataA = xhrA.response; // the next request need the data from the first response
    let xhrB = new XMLHttpRequest();
    xhrB.open('POST', urlB);

    xhrB.addEventListener('load', () => {
        dataB = xhrB.response;
        let xhrC = new XMLHttpRequest();
        xhrC.open('POST', urlC);

        xhrC.addEventListener('load', () => {
          dataC = xhrC.response;
          // other operations
        })

        xhrC.send(dataB);
    })

    xhrB.send(dataA);
})

xhrA.send();

// Promise style

let dataC;

(new Promise((resolve, reject) => {
  let xhrA = new XMLHttpRequest();
  xhrA.open('GET', urlA);
  xhrA.addEventListener('load', () => {
    resolve(xhrA.response);
  })
  xhrA.send();
})).then((dataA) => {
  return new Promise((resolve, reject) => {
    let xhrB = new XMLHttpRequest();
    xhrB.open('POST', urlB);
    xhrB.addEventListener('load', () => {
        resolve(xhrB.response)
    })

    xhrB.send(dataA);
  })
}).then(() => {
  let xhrC = new XMLHttpRequest();
  xhrC.open('POST', urlC);

  xhrC.addEventListener('load', () => {
    dataC = xhrC.response;
    // other operations
  })

  xhrC.send(dataB);
})

// async + await



// XHR
function request_server_and_get_response_back(delay) {
  return new Promise((resolve, reject) => {
    setTimeout(() => { // XHR equivalent
      resolve({ 'name': 'Dave', 'age': 16 })
    }, delay);
  })
}

function renderData(data) {
  console.log(data);
}

request_server_and_get_response_back(2000).then((data) => renderData(data))
```
