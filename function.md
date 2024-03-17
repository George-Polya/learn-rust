# Rust - function
러스트에서 함수나 변수는 snake_case로 작성한다
러스트 특유의 타입 선언을 위해서 파라미터가 독특한걸 제외하면 다른 언어의 함수(메소드)와 비슷하다

```rust
fn main() {
    print_labeled_measurement(5, 'h');
}

// 독특한 파라미터의 예시
fn print_labeled_measurement(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}

```

## 구문과 표현식
러스트는 표현식 기반의 언어. 다른 언어들은 구문과 표현식을 구분하지 않는다
- **구문**은 어떤 동작을 수행하고 값을 반환하지 않는 명령
- **표현식**은 결과값을 평가한다

사실 무슨 말인지 모르겠다

### 구문
```let```키워드로 변수를 만들고 값을 할당하는 것은 대표적인 구문의 예시이다.

아래의 ```let y = 6;```은 구문이다
```rust
fn main(){
	let y = 6;
}
```
함수 정의도 구문이며, 구문은 값을 반환하지 않는다. 따라서 다음 코드는 에러가 난다
```rust
fn main() {
    let x = (let y = 6);
}
```

### 표현식
표현식은 값을 평가한다.
```let y = 6;```이라는 구문에서 ```6```은 ```6```이라는 값을 평가하는 표현식이다.
아래 예제처럼 중괄호로 만들어진 새로운 스코프 블록도 표현식
```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}

```


### 반환 값을 갖는 함수
다음 코드는 에러가 나지 않지만
```rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {x}");
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```
아래의 코드는 에러가 난다.
```rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {x}");
}

fn plus_one(x: i32) -> i32 {
    x + 1;
}
```
그 이유는 ```plus_one``` 함수의 정의에는 ```i32```값을 반환한다고 되어있지만, 구문은 값을 평가하지 않기에 ```()```라는 유닛 타입으로 표현된다
따라서 아무것도 반환하지 않게 되고, 함수가 정의된 내용과 상충하게 되어 에러가 발생한다.

둘 중 하나로 해결할 수 있다.

1. 세미콜론을 제거해서 구문을 표현식으로 변경
  ```rust
  fn plus_one(x: i32) -> i32 {
      x + 1
  }
  ```

2. 명시적 반환 사용하기 : return 키워드 사용
  ```rust
  fn plus_one(x: i32) -> i32 {
      return x + 1;
  }

  ```

### 참고자료
[러스트 프로그래밍 공식 가이드 - 함수](https://doc.rust-kr.org/ch03-03-how-functions-work.html)
