# Chapter 07 - 연산자

## 0. 용어 정리
![image](https://github.com/user-attachments/assets/aaac2239-2051-4470-9d05-1bb0804344f8) _이미지 출처: https://www.techtarget.com/whatis/definition/operand_
* JS **Operator(연산자):** 하나 이상의 표현식에 산술/할당/비교/논라/타입/지수 연산 등을 수행해 하나의 값을 만드는 기호.
* JS **Operand(피연산자):** 연산의 대상이 되는 표현식.
* **Side effect(부수 효과):** 표현식을 연산할 때, 연산자가 피연산자의 값을 변경해버리는 효과.(e.g., `score++`, `--score` 등)

## 1. Arithmetic Operators: 산술 연산자
피연산자로 수학적 계산을 수행해 새로운 숫자 값을 만드는 연산자.
<br>
산술 연산이 불가능한 경우, `NaN`을 반환한다.
1. **이항 산술 연산자**
    * `+`(덧셈), `-`(뻴셈), `*`(곱셈), `/`(나눗셈), `%`(나머지)
2. **단항 산술 연산자**
    * <img src="https://github.com/user-attachments/assets/f17e04eb-f69d-4aba-9f82-075a4ab84df2" width="500px" />
3. **문자열 연결 연산자**
    * 연산자 `+`는, 피연산자 중 **하나라도 문자열이면** 문자열 연결 연산자로 동작한다.
    * ```javascript
      '1' + 2;  // -> '12'
      1 + '2';  // -> '12'
      ```

## 2. Assignment Operators: 할당 연산자
우항에 있는 피연산자의 평가 결과를 좌항에 있는 변수에 할당하는 연산자.
<br>
좌항에 값을 할당하므로 변수 값이 변하는 부수 효과가 있다.
<br>
![image](https://github.com/user-attachments/assets/93dd51eb-e9b9-48dd-ace3-1a265cf3b048)

## 3. Comparison Operators: 비교 연산자
좌항과 우항의 피연산자를 비교 후 결과를 boolean 값으로 반환하는 연산자.
1. **동등/일치 비교 연산자**
    * <img src="https://github.com/user-attachments/assets/e3e50c3a-7093-4b41-b693-0c550a54318f" width="500px" />
    * **동등 비교**(`==`) 연산자는 좌항과 우항을 비교 시 **암묵적 타입 변환을 통해 타입을 일치시킨 후** 같은 값인지 비교한다. 이는 코드를 예측하기 어렵게 만든다.
    * 따라서 동등 비교 연산자는 되도록 지양하고, 일치 비교(`===`) 연산자를 사용하자.
    * **일치 비교**(`===`) 연산자는 좌항과 우항을 비교 시 **타입도 같고 값도 같아야지만 true를 반환**한다.
    * 주의사항: `NaN`은 자신과 일치하지 않는 유일한 값이다.
        * ```javascript
          NaN === NaN;  // -> false
          
          // 어떤 값이 NaN인지 보고 싶으면 빌트인 함수 Number.isNaN을 사용하자.
          Number.isNaN(NaN);  // -> true
          ```
2. **대소 관계 비교 연산자**
    * <img src="https://github.com/user-attachments/assets/4456fc15-4e2c-4eab-907b-055e8771c648" width="500px" />

## 4. Conditional/Ternary Operator: 삼항 조건 연산자
조건식의 평과 결과에 따라 반환할 값을 결정하는 연산자. 부수 효과는 없다.
<br>
<img src="https://github.com/user-attachments/assets/10570f15-db7b-4787-a77d-c277790db23f" width="500px" />

## 5. Logical Operators: 논리 연산자
우항과 좌항의 피연산자를 논리 연산하는 연산자.
<br>
![image](https://github.com/user-attachments/assets/8685a12c-6484-45fc-89cd-cc11bd73a4aa)
<br>
단, **부정 논리 연산자**(`!`)의 경우엔 우항의 피연산자를 연산하며, 언제나 boolean 값을 반환한다.

부정 논리 연산자의 피연산자는 반드시 boolean 값일 필요는 없다.
<br>
boolean 값이 아닌 경우엔 알아서 boolean 타입으로 **암묵적 타입 변환**을 해 주기 때문이다.
```javascript
!0;  // -> true
!'Hello';  // -> false
```

## 6. Comma Operator: 쉼표 연산자
왼쪽 피연산자부터 차례대로 피연산자를 평가하고, 마지막 피연산자의 평가가 끝나면 마지막 피연산자의 평가 결과를 반환하는 연산자.
```javascript
var x, y, z;
x = 1, y = 2, z = 3;  // -> 3
```

## 7. Grouping Operator: 그룹 연산자
피연산자를 소괄호(`()`)로 묶어 표현식 평가 순서를 정하는 연산자. 연산자 우선순위가 가장 높다.
```javascript
10 * 2 + 3;  // -> 23
10 * (2 + 3);  // -> 50
```

## 8. Unary Operators - typeof: typeof 연산자
피연산자의 데이터 타입을 문자열로 반환하는 연산자.
<br>
<img src="https://github.com/user-attachments/assets/718d20b9-e67a-462b-81d4-9d5600a1cf8e" width="500px" />

**주의사항:** `typeof null`은 `null`이 아닌 `object`를 반환하는 자바스크립트 언어 버그가 존재하므로,
<br>
값이 `null` 타입인지 확인하고자 할 땐 `typeof`가 아닌 **일치 비교 연산자**(`===`)를 쓰도록 하자.
```javascript
var foo = null;

typeof foo === null;  // -> false
foo === null;  // -> true
```

## 9. Arithmetic Operators - exponentiation operator: 지수 연산자
좌항을 base(밑)으로, 우항을 exponent(지수)로 거듭 제곱해 숫자 값을 반환하는 연산자. ES7에 도입됐다.
```javascript
2 ** 2;  // -> 4
2 ** 2.5;  // -> 5.65685425
2 ** 0;  // -> 1
2 ** -2;  // -> 0.25

// 지수 연산자가 도입되기 전까진 Math.pow 메서드를 사용했었음.
Math.pow(2, 2);  // -> 4
Math.pow(2, 2.5);  // -> 5.65685425
Math.pow(2, 0);  // -> 1
Math.pow(2, -2);  // -> 0.25
```

## 10. 그 외의 연산자(각 장에서 살펴볼 예정)
![image](https://github.com/user-attachments/assets/25244c27-57e0-42a6-8177-5b5504901106)

## 11. Side Effects: 연산자의 부수 효과
대부분은 그렇지 않지만, 아래처럼 일부 연산자는 다른 코드에 영향을 주는 '부수 효과'가 있다.
* 할당 연산자(`=`)
* 증가/감소 연산자(`++`/`--`)
* delete 연산자

## 12. Operator Precedence: 연산자 우선순위
연산자 간에도 연산자가 실행되는 순위가 다르다.
<br>
<img src="https://github.com/user-attachments/assets/169da452-406d-467b-877a-66ea2f35c536" width="500px" />

**그러나,** 이 모든 걸 기억할 수도 없을 뿐더러 기억하더라도 실수하기 쉽다.
<br>
따라서 우선순위를 암기하기보다는, **그룹 연산자**(`()`)**를 활용**해 우선순위를 명시적으로 조절하도록 하자.

## 13. Associativity: 연산자 결합 순서
![image](https://github.com/user-attachments/assets/6510ec39-112f-4da4-8154-ee4bb25c6e20)

끝.
