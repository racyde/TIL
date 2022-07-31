# 숫자가 혼합된 문자열을 숫자로 변환하기

숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 s가 매개변수로 주어집니다.
s가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.

### 결과 예시
* "one4seveneight"  ->  1478
* "23four5six7" ->  234567
* "2three45sixseven"  ->	234567
* "123"	  ->  123


### 제한사항
* 1 ≤ s의 길이 ≤ 50
* s가 "zero" 또는 "0"으로 시작하는 경우는 주어지지 않습니다.
* return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 s로 주어집니다.

### 풀이방법
- String.prototype.replace() 및 정규식 사용

```jsx
function solution(s) {
    var answer = 0;
    let toNum = 0;
    const reg = [/zero/gi ,/one/gi, /two/gi, /three/gi, /four/gi, /five/gi, /six/gi, /seven/gi, /eight/gi, /nine/gi]  //정규식을 담은 배열
    
    toNum=s;
    for(let i=0; i<=9; i++){
        toNum= toNum.replace(reg[i], i) // toNum에 교체된 새로운 문자열을 넣음
    }
    answer = Number(toNum); // 자료형 변환
    
    return answer;
}

```
