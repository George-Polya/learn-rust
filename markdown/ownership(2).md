# Rust - 소유권(Ownership) 2
### 소유권 규칙
- 러스트에서 각각의 값은 소유자(owner)가 정해져 있다
- 한 값의 소유자는 동시에 여럿 존재할 수 없다
- 소유자가 스코프 밖으로 벗어날때, 값은 버려진다

### 문자열 리터럴과 ```String```
```rust
let s = "hello" // 문자열 리터럴은 immutable

let mut str = String::from("hello") // String. 변경가능
```

- 문자열 리터럴 : 컴파일 타임에 내용을 알 수 있음
- ```String```타입은 힙에 메모리를 할당하는 방식

> 힙에 할당하는 방식은
- 실행 중 메모리 할당자로부터 메모리를 요청
- 사용을 마쳤을 때 메모리를 해제할 방법이 필요


***러스트에서는 변수가 자식이 소속된 스코프를 벗어나는 순간 자동으로 메모리를 해제한다***

```rust
    {
        let s = String::from("hello"); // s는 이 지점부터 유효합니다

        // s를 가지고 무언가 합니다
    }                                  // 이 스코프가 종료되었고, s는 더 이상
                                       // 유효하지 않습니다.

```
스코프 밖을 벗어나면 자동으로 ```drop```이라는 특별한 함수가 호출되어 메모리가 해제됨


#### 변수와 데이터 간의 상호작용 : 이동
```rust
let s1 = String::from("hello");
let s2 = s1; // 러스트에서는 기존 변수를 무효화하기 때문에 이동(move)라고 한다.
```

```s1```이 ```s2```로 이동되었다
![](https://velog.velcdn.com/images/tollea1234/post/5af810d6-44d3-4bbc-8a59-dd1247656d6d/image.png)

#### 변수와 데이터 간의 상호작용 : 클론
```rust
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);

```
![복사](https://velog.velcdn.com/images/tollea1234/post/ac23623c-7f46-4717-8fc8-39402917919f/image.png)


#### 스택에만 저장되는 데이터 : 복사
- 컴파일 타임에 크기가 고정되는 타입은 모두 스택에 저장된다.
- ```Copy```트레이트가 구현되어 있으면, 이 타입의 변수는 이동되지 않고 깊은 복사되고, 대입 연산 후에도 사용할 수 있다
- 하지만, 구현하려는 타입이나, 구현하려는 타입 중 일부에 ```Drop```트레이트가 구현된 경우엔 ```Copy```트레이트를 어노테이션할 수 없다.



```rust
let x = 5;
let y = x;

println!("x = {}, y={}",x,y);

```


#### 소유권과 함수
- 함수로 값을 전달하는 매커니즘은 변수에 값을 대입할때와 유사
- 함수에 변수를 전달하면 대입 연산과 마찬가지로 이동이나 복사가 일어남
- 힙에 저장된 변수 -> 이동, 스택에 저장된 변수 ->복사
```rust
fn main() {
    let s = String::from("hello");  // s가 스코프 안으로 들어옵니다

    takes_ownership(s);             // s의 값이 함수로 이동됩니다...
                                    // ... 따라서 여기서는 더 이상 유효하지 않습니다

    let x = 5;                      // x가 스코프 안으로 들어옵니다

    makes_copy(x);                  // x가 함수로 이동될 것입니다만,
                                    // i32는 Copy이므로 앞으로 계속 x를
                                    // 사용해도 좋습니다

} // 여기서 x가 스코프 밖으로 벗어나고 s도 그렇게 됩니다. 그러나 s의 값이 이동되었으므로
  // 별다른 일이 발생하지 않습니다.

fn takes_ownership(some_string: String) { // some_string이 스코프 안으로 들어옵니다
    println!("{}", some_string);
} // 여기서 some_string이 스코프 밖으로 벗어나고 `drop`이 호출됩니다.
  // 메모리가 해제됩니다.

fn makes_copy(some_integer: i32) { // some_integer가 스코프 안으로 들어옵니다
    println!("{}", some_integer);
} // 여기서 some_integer가 스코프 밖으로 벗어납니다. 별다른 일이 발생하지 않습니다.
```

### 참고자료
[The Rust Programming Language - 소유권이 뭔가요?](https://doc.rust-kr.org/ch04-01-what-is-ownership.html)