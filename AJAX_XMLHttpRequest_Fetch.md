# AJAX

Ajax(Asynchronous Javascript And Xml)

웹 클라이언트에서 웹 서버와 비동기적으로 데이터를 교환하는 방법

## XMLHttpRequest
가장 원초적으로 요청을 날리는 방법이며 요즘은 잘 안 쓴다고 한다. 
Promise 기반이 아닌 Event 기반이며, 가독성도 좋지 않기 때문이다.
```javascript
let xhr = new XMLHttpRequest();
xhr.open('GET', 'http://domain/service');

// request state change event 요청이 완료되면 수행하는 함수.
xhr.onreadystatechange = function() {

  // request completed?
  if (xhr.readyState !== 4) return;

  if (xhr.status === 200) {
    // request successful - show response
    console.log(xhr.responseText);
  }
  else {
    // request error
    console.log('HTTP error', xhr.status, xhr.statusText);
  }
};

// start request
xhr.send();
```

## Fetch
```javascript
fetch(
    'http://domain/service',
    { method: 'GET' }
  )
  .then( response => response.json() )
  .then( json => console.log(json) )
  .catch( error => console.error('error:', error) );
```


#Reference

https://futurists.tistory.com/51
https://yceffort.kr/2020/01/think-about-fetch
https://velog.io/@lingodingo/ES6-XMLHttpRequest