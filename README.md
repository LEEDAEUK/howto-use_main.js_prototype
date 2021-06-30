> main.js에 전역설정한 $prototype 사용법


```jsx
//main.js

let socket = io('https://azure-ats.azurewebsites.net', {
  withCredentials: true,
  "forceNew": true,
  // "reconnectionAttempts": "Infinity", //avoid having user reconnect manually in order to prevent dead clients after a server restart
  // "timeout": 10000, //before connect_error and connect_timeout are emitted.
  "transports": ["websocket"]
});
socket.on("connect", ()=>{
  console.log("client socket on")
})

Vue.prototype.$socket = socket;
```

main.js에서 Vue.prototype.$socket로 선언함으로서 다른 vue에서 접근 가능

```jsx
//!!.vue

this.$socket.on("notify", (msg) => {
      console.log("getting socket msg");
      console.log(msg.message);
      this.get_socket_message = msg.message;
    });
```

혹은

main.js에 추가하고, 전체 컴포넌트에서 이걸 함수처럼 쓰고싶다면

```jsx
new Vue({
  el: '#app',
  router,
  components: { App },
  store,
  template: '<App/>',
  methods: {

		//이부분부터

    getFormatDate(inDate) {
      var date = new Date(inDate)
      var year = date.getFullYear();              //yyyy
      var month = (1 + date.getMonth());          //M
      month = month >= 10 ? month : '0' + month;  //month 두자리로 저장
      var day = date.getDate();                   //d
      day = day >= 10 ? day : '0' + day;          //day 두자리로 저장

      var hh = date.getHours()
      hh = hh >= 10 ? hh : '0' + hh;

      var mm = date.getMinutes()
      mm = mm >= 10 ? mm : '0' + mm;

      var ss = date.getSeconds()
      ss = ss >= 10 ? ss : '0' + ss
      return  year + '-' + month + '-' + day + ' ' + hh + ':' + mm + ':' + ss;       //'-' 추가하여 yyyy-mm-dd 형태 생성 가능
    }

		//이부분까지를 .vue파일에서 불러올 땐

  }
})
```

이런식으로 $root로 접근 가능하다

```jsx
<td data-label="取得時間">
  {{$root.getFormatDate(data.get_time)}}
</td>
```
