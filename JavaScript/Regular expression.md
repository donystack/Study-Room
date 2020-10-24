원래는 펄프로그래밍에서 사용한 객체인데, 너무 유용해서 현재 많은 프로그래밍 언어에서 사용되는 객체입니다.

### 1. 정규 표현식 객체

정규 표현식 객체 생성
2가지 방법이 있으며 일반적으로 resgExp2 방식을 많이 사용
```
<script>
var regExp1 = new RegExp('expression'); // RegExp 생성자로 생성
var regExp2 = /expression/; // 정규 표현식 리터럴로 생성
</script>
```


#### 정규 표현식 객체의 메서드
- regexp.exec( string ) : 문자열에서 일치하는 부분을 찾게 되는 경우, 이 부분들을 추출하여 배열에 담아 반환
- regexp.test( string ) : 정규 표현식과 일치하는 문자열이 있으면 true, 아니면 false를 리턴합니다.

```
<script>
//변수 선언
var regExp = /script/;
var sentence = 'Hello Javascript !';

// 메서드 사용
var testResult = regExp.test(sentence);
var execResult = regExp.exec(sentence); 
</script>

결과 
testResult => true
execResult => script
```

#### 정규 표현식을 사용하는 문자열 String 객체의 메서드(패턴 매칭을 한다)
- string.match(regExp) : 정규 표현식과 일치하는 부분을 리턴합니다.
- string.replace(regExp) : 정규표현식과 일치하는 부분을 새로운 문자열로 바꿉니다.
- string.search(regExp) : 정규 표현식과 일치하는 부분의 위치를 리턴합니다.
- string.split(regExp) : 정규 표현식을 기준으로 문자열을 잘라 배열을 리턴합니다.

<br>

### 2. 플래그
정규 표현식을 사용하면 가장 먼저 패턴이 일치하는 문자열만 찾습니다. 모든 문자를 대체하려면 플래그 문자를 사용합니다.
```
g(global)  :  전역 비교를 수행합니다.
i(case-insensitive)  :  대소문자를 가리지 안하고 비교합니다.
m(multiline match)  :  여러 줄의 검사를 수행합니다.
```
#### 생성 

간단한 방식으로 생성할 때는 뒤에 붙여 사용하며, 생성자를 사용할 때는 두 번째 매개변수에 입력합니다.
```
var regExp = /Expression/im
var regExp = new regExp('Expression', 'im');
```
<br><br><br>
 ### 정규 표현식 패턴을 작성할 때는 숫자(0~9)와 알파벳(a~z, A~Z) 등의 <mark>일반 문자(리터럴 문자)</mark>와 '+', '.'등의 <mark>특수문자(메타 문자)</mark>를 사용합니다. 정규 표현식에서 사용하는 특수문자는 메타 문자(meta character)라고 하며, 이 메타 문자를 일반 문자(리터럴 문자)로서 사용할 때는 '\n'처럼 메타 문자 앞에 \문자를 붙여 주여야 합니다.

### 3. 일반 문자(리터럴 문자)

\n처럼 \문자로 시작하는 리터럴 문자는 문자로 표기할 수 없는 특수 문자입니다.
```
유니코드 문자  :  문자 그 자체를 뜻한다. 단, ^, $, \, ., *, +, ?, (, ), [, ], {, }, |는 제외한다.
\0  :  NULL 문자(\u0000)
\n  :  개행 문자(LF=\u000A)
\t  :  탭 문자(\u0009)
```
<br>

### 메타 문자

대체 문자 별로 중요하지도 않으니 아래로 내리고 문자클래스를 먼저 서술하자

### 4. 문자 클래스 [...]
문자 클래스는 특정 문자 집합 안의 모든 단일 문자와 일치합니다. 문자 클래스를 정의하려면 문자 집합의 요소가 되는 문자 리터럴을 나열하여 대괄호로 묶어 줍니다. 
```
.  :  아무 글자
[abc]  :  'a', 'b', 'c' 중 문자 한 개
[^...]는 문자 클래스인 대괄호 안에 들어 있지 않은 단ㅇ리 문자와 일치합니다.
[^abc]  :  괄호 안의 글자를 제외한 문자 한 개
문자 클래스인 대괄호 안에서 하이픈 (-)을 사용하면 문자의 범위를 지정할 수 있습니다.
[a-z]  :  알파벳 a부터 z까지
[A-Z]  :  알파벳 A부터 Z까지
[0-9]  :  숫자 0부터 9까지
```

### 문자 클래스의 단축 표기
숫자와 숫자 외의 문자 : \d, \D
단어 문자와 단어 문자 외의 문자 : \w, \W
\w는 모든 영어 단어 문자(알파벳, 숫자, 언더스코어)라는 뜻입니다. [a-zA-Z0-9_]의 단축 표기입니다.

공백 문자와 공백 문자 외의 문자 : \s, \S
[]

### 반복 패턴
이전의 예에서는 문자 클래스이ㅡ 단축 기호를 표기할 때 시각을 표현하는 숫자 네 개를 \d\d\d\d로 표기하고 있었습니다. 정규 표현식의 요소를 여러번 반복하도록 지정하여 더욱 간결하게 표기할 수 있습니다.

최소 m번, 최대 n번 반복 : {m,n}
/[a-z]{6,12}/


### 그룹화와 참조





### 5. 앵커 문자
문자열의 앞과 뒤를 구분해주는 정규 표현식 기호입니다.
```
^HELLO  :  맨 앞 문자가 HELLO
WORLD$  :  맨 뒤 문자가 WORLD 
```



### 4. 대체 문자
```
$&  :  일치하는 문자열
$`  :  일치하는 부분의 앞부분 문자열
$'  :  일치하는 부분의 뒷부분 문자열
$1, $2, $3  :  그룹
```

정규 표현식에 일치하는 문자열을 '+$&+'로 변경합니다.
```
<script>
var regExp = /l/;
var string = 'Hello Javascript!';

var result = string.replace(regExp, '+$&+');

console.log(result);
</script>

결과 : 'He+l+lo Javascript!'
```


정규 표현식을 괄호로 묶으면 각각의 문자는 $1, $2, $3 문자로 대체됩니다.

```
<script>
var regExp = /(e)(l)(l)/;
var string = 'Hello Javascript!';

var result = string.replace(regExp, '+$1=$2=$3+');

console.log(result);
</script>

결과 : 'H+e=l=l+o Javascript!'
각각의 문자가 대체되므로 $1, $2, $3을 직접 출력한 것이 아니라 e, l, l을 출력합니다.
```