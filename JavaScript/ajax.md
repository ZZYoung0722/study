# ajax

## ajax?

자바스크립트와 XML을 이용한 비동기적 데이터 처리
요즘은 XML 보다 JSON을 주로 사용한다.

비동기 처리 => 특정 코드의 연산이 끝날 때까지 코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 자바스크립트의 특성을 의미

**특징**

- 페이지의 새로고침 없이 서버에 요청이 가능
- 따라서 전체 페이지가 아닌 페이지 일부분만 업데이트 가능
- 서버로부터 데이터를 받고 작업 수행

### Promise

promise => 자바스크립트 비동기 처리에 사용되는 객체 (약속)


```javascript
var promise = testFun();

  promise.done(function (result) {

  })

});

function testFun() {
  var deferred = $.Deferred();    // $.Deferred() 끝났을때 다음 함수를 실행한다.

  $.ajax({
      url: '/noticelistTest',
      type: 'get',
      dataType: "json",
      success: function (result) {
          deferred.resolve(result);
          debugger
      },
      error: function () {
          deferred.reject();
      }
  });

  return deferred.promise();

}
```

promise  객체가 완료되었을 때 done(=resolve) 메소드 호출,

실패 fail(=reject), 완료 되었건 실패했건 행동이 끝났으면 always 호출

콜백 형식은 점점 더 코드가 복자바해지는 문제를 발생시키기 때문에,
콜백이 여러번 중첩될 것 같으며녀 promise 형식을 사용
