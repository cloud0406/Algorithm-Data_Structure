# Big O Notation

여러가지 코드를 일반적으로 서로 비교하고 성능을 평가하는 방법.

- 한 문제에 대한 여러가지 해결책을 비교하고 성능이 어떤지 이해하는데 도움.
- 여러 접근법의 장단점을 얘기할때 유용 -> ex) 시간은 더 오래 걸려도 처리하는 시간에 변동이 없다던지 / 모든 접근법에는 장단점이 있기 때문에 반드시 하나의 접근법만이 최고의 방법은 아닐 수 있다.
- 빅오를 이해하면 디버그시 어디서 문제가 나타나며 어떤 부분이 비효율적인 코드인지 찾는데 도움이 된다.

## 좋은 코드란 무엇일까?

- 빠른 코드?
- 메모리를 덜 쓰는 코드?
- 더 읽기 쉬운 코드?

코드를 비교할때 단순히 걸리는 시간을 통해 비교하는 방법은 좋은 방법이라 할 수 없음

- 다른 기계라면 다른 시간이 나옴
- 같은 기계예도 반복해보면 결과가 다르게 기록될 수 있음

따라서 코드가 실행될때 걸리는 정확한 시간을 초로 측정하는것보다는 컴퓨터가 처리해야하는 연산 갯수를 세는 것이 정확 (어떤 컴퓨터를 사용하든 연산 개수는 동일하기 때문에)

같은 연산을 하는 두개의 코드 비교

```js
function addUpToFirst(n) {
  var toatl = 0;
  for (var i = 0; i <= n; i++) {
    total += i;
  }
  return total;
  ㄴ;
}
```

n에 크기에 따라 반복문의 반복 횟수가 달라지기 때문에 n이 10억처럼 매우 커질경우 그만큼 오래걸림

아래 코드를 보자

```js
function addUpToSecond(n) {
  return (n * (n + 1)) / 2;
}
```

같은 연산을 하지만 n에 크기에 상관없이 연산을 동일하게 3번함 (\*, +, /)
n이 커질수록 첫 번째 코드보다 두 번째 코드가 연산 횟수가 현저히 적음을 확인할 수 있음.

위의 두 함수를 빅오로 표현하면 다음과 같음
첫 번째 함수인 addUpToFirst는 O(1) -> 어떠한 입력값 n이 들어와도 n에 상관없이 연산 수가 동일
두 번째 함수인 addUpToSecond는 O(n) -> 입력값 n에 따라 반복 횟수가 선형적으로 증가

```js
function countUpAndDown(n) {
  console.log("Going UP!");
  for (let i = 0; i < n; i++) {
    console.log(i);
  }

  console.log("At the top! Going Down...");
  for (let j = n - 1; j >= 0; j--) {
    console.log(j);
  }
}
```

위의 함수는 같은 작업을 거꾸로하는 두 반복문이 있음. 그렇기 때문에 O(2n)이라 생각할 수 있지만 큰 틀에서만 생각하면 O(n)임.

```js
function printAllPairs(n) {
  for (var i = 0; i < n; i++) {
    for (var j = 0; j < n; j++) {
      console.log(i, j);
    }
  }
}
```

위의 함수도 마찬가지로 n이 커질수록 시간이 증가되지만 반복문이 2개여서 O(2n)으로 표현하는 것이 아니라 중첩되어 있기 때문에 O(n\*n) -> 즉, O(n<sup>2</sup>)이 된다. 이 경우에는 n이 커질수록 실행 시간이 n제곱의 값으로 늘어나게 된다.

위의 예시들에서부터 빅오란 기본적으로 입력인 n이 커질수록 알고리즘이 얼마나 효율적으로 표현하는 방식이라는 것을 기억하자.

## 빅오 표현식 단순화

- O(2n) -> O(n)
- O(500) -> O(1)

O(500)이든 O(1)이든 입력값(n)에 상관없이 어떤 상황에든 같은 연산을 반복함. 따라서 굳이 O(500)이라 표현하지 않고 O(1)과 같이 단순하게 표현 -> 입력값에 따라 걸리는 시간이 어떻게 변하는지에 초점 (입력값 n이 무한히 커지는 경우를 생각해볼 것)

마찬가지로 O(n+10) , O(1000n+50) -> O(n) / O(n<sup>2</sup>+5n+8) -> O(n<sup>2</sup>) 로 단순화하여 표현

### 빅오의 규칙

1. 산수는 상수 (덧셈, 뺄셈, 나눗셈, 곱셈 포함) -> n의 값과 상관이 없음

   - 컴퓨터가 2+2와 100만+2를 처리하는 시간은 비슷

2. 변수 배정도 상수.

   - 변수에 값을 배정하는데 걸리는 시간은 비슷 (x=100, x=200000)

3. 인덱스를 사용해 배열 엘리먼트에 접근하는 것 역시 상수

   - 배열에서 첫 번째 요소를 찾든, 10번째 요소를 찾던 인덱슬르 사용하면 같은 시간이 걸림
   - 마찬가지로 객체를 다루고 데이터를 접근하기 위해 키가 있다면 그것도 실행 시간이 상수

4. 루프가 있다면 복잡도가 (루프의 길이 x 루프 안의 연산들)

```js
function logAtLeast5(n) {
  for (var i = 1; i <= Math.max(5, n); i++) {
    console.log(i);
  }
}
```

위의 함수는 입력값 n에 따라 걸리는 시간이 n에 비례해 증가하므로 O(n)으로 표현 가능

```js
function logAtMost5(n) {
  for (var i = 1; i <= Math.min(5, n); i++) {
    console.log(i);
  }
}
```

위의 함수는 입력값에 따라 0~5번 반복하므로 n이 커질수록 빅오는 상수라고 단순화 할 수 있음. 즉 O(1)로 표현 (반복문이 있다고 무조건 O(n)으로 표현하는 것이 아님)

### Quiz1

다음 빅오 표현식을 간단히 해라.

1. O(n+10)

   -> O(n)

2. O(100\*n)

   -> O(n)

3. O(25)

   -> O(1)

4. O(n^2 + n^3)

   -> O(n^3)

5. O(n+n+n+n)

   -> O(n)

### Quiz2

아래 함수에 대한 시간 복잡도를 구해라

1. O(n)
   ```js
   function logUpTo(n) {
     for (var i = 1; i <= n; i++) {
       console.log(i);
     }
   }
   ```
2. O(1)
   ```js
   function logAtMost10(n) {
     for (var i = 1; i <= Math.min(n, 10); i++) {
       console.log(i);
     }
   }
   ```
3. O(n)
   ```js
   function logAtLeast10(n) {
     for (var i = 1; i <= Math.max(n, 10); i++) {
       console.log(i);
     }
   }
   ```
4. O(n)
   ```js
   function onlyElementsAtEvenIndex(array) {
     var newArray = Array(Math.ceil(array.length / 2));
     for (var i = 0; i < array.length; i++) {
       if (i % 2 === 0) {
         newArray[i / 2] = array[i];
       }
     }
     return newArray;
   }
   ```
5. O(n^2)
   ```js
   function subtotals(array) {
     var subtotalArray = Array(array.length);
     for (var i = 0; i < array.length; i++) {
       var subtotal = 0;
       for (var j = 0; j <= i; j++) {
         subtotal += array[j];
       }
       subtotalArray[i] = subtotal;
     }
     return subtotalArray;
   }
   ```

## 공간 복잡도

입력이 커질수록 알고리즘의 실행 속도가 어떻게 바뀌는가를 시간 복잡도라고 한다. 이와 마찬가지로 입력이 커질수록 알고리즘이 얼마나 많은 공간을 차지하는지를 공간 복잡도라고 한다. (입력 되는 것을 제외한 알고리즘 자체가 필요로 하는 공간을 보조 공간 복잡도라 한다.)

- 불린, 숫자, undefined, null은 JS에서 모두 불변 공간이다. -> 입력의 크기와 상관업시 숫자가 1이든 1000이든, 불린 값이 참이든 거짓이든 모두 불변 공간이라 여긴다.
- 문자열은 O(n) 공간이 필요하다. -> 만약 n이 문자열의 길이라면, 길이가 1인 문자열보다 길이가 50인 문자열이 50배 더 많은 공간을 차지한다.
- 문자열과 마찬가지로 refernece 타입, 배열, 객체 모두 대부분 O(n)이라 생각한다. (n은 배열의 길이, 객체의 키 갯수)

다음 예시를 보자

```js
function sum(arr) {
  let total = 0;
  for (let i = 0; i < arr.length; i++) {
    total += arr[i];
  }
  return total;
}
```

시간은 상관하지 않고 공간을 할당한 수를 보면 number가 둘 할당 되었다. (total, i)
'+' 등 연산은 기존의 할당한 공간을 가지고 처리하기 때문에 상관이 없다.
즉, 상수 공간 -> O(1)

다음 예시를 보자.

```js
function double(arr) {
  let newArr = [];
  for (leti = 0; i < arr.length; i++) {
    newArr.push(2 * arr[i]);
  }
  return newArr;
}
```

입력된 arr 배열의 길이에 따라 새로운 배열에 저장 되는 아이템 개수가 정해진다. 즉, 차지 하는 공간은 입력된 배열의 크기와 비례해 커지게 된다. -> O(n)

### Quiz3

아래 함수에 대한 공간 복잡도는?

1. O(1) -> 선언 부분(공간 할당)이 i=1 부분밖에 없음

```js
function logUpTo(n) {
  for (let i = 1; i <= n; i++) {
    console.log(i);
  }
}
```

2. O(1)

```js
function logAtMost10(n) {
  for (var i = 1; i <= Math.min(n, 10); i++) {
    console.log(i);
  }
}
```

3. O(1)

```js
function logAtMost10(n) {
  for (var i = 1; i <= Math.min(n, 10); i++) {
    console.log(i);
  }
}
```

4. O(n) -> 배열 array의 길이에 따라 공간 할당이 달라짐 (newArray = ~ )

```js
function onlyElementsAtEvenIndex(array) {
  var newArray = Array(Math.ceil(array.length / 2));
  for (var i = 0; i < array.length; i++) {
    if (i % 2 === 0) {
      newArray[i / 2] = array[i];
    }
  }
  return newArray;
}
```

5. O(n) -> 시간 복잡도가 아닌 공간 복잡도의 개념으로 보면 공간 할당 영역에 영향을 끼치는 것은 subtotalArray, i, j 선언 부분이며 subtotatlArray는 배열의 길이에 따라 공간 할당 영역이 바뀐다.

```js
function subtotals(array) {
  var subtotalArray = Array(array.length);
  for (var i = 0; i < array.length; i++) {
    var subtotal = 0;
    for (var j = 0; j <= i; j++) {
      subtotal += array[j];
    }
    subtotalArray[i] = subtotal;
  }
  return subtotalArray;
}
```
