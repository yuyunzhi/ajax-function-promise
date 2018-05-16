<h2>Promise优化AJAX函数</h2>

```
window.jQuery.ajax = function({url,method,body,headers}){
    return new Promise(function(resolve,reject){
        let request = new XMLHttpRequest()
        request.open(method, url) // 配置request第一部分
        for(let key in headers){
            let value = headers[key];
            request.setRequestHeader(key,value)
        }
        request.onreadystatechange = ()=>{
            if(request.readyState===4){
                if(request.status >= 200 && request.status < 300){
                    resolve.call(undefined,request.responseText)        
                }else if(request.status >= 400){
                    rejcet.call(undefined,request)
                }
            }
        }
        request.send(body)//设置请求的第四部分
    })
}
```

<h2>函数调用</h2>

```
myButton.addEventListener('click', (e)=>{
    window.jQuery.ajax({
        url:'/xxx',
        method:'post',
        body:'a=1&b=2',
        headers:{
            'content-type':'application/x-www-form-urlencoded',
            'yukaka':'25'
        }    
    }).then(
        (text)=>{console.log("响应成功，调用第一个函数");return 1},//成功调用这个函数       
    ).then(
        (text)=>{console.log(text)},//成功调用这个函数，这里的参数是上一个then返回的值
        (request)=>{console.log('error')} //失败调用这个函数     
    ) 
})

```

<strong>注意，第二次使用then，成功的函数得到的参数是上一个成功函数返回的值</strong>
