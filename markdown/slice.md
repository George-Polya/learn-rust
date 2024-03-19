# 슬라이스(slice)
### 문자열 슬라이스
문자열 슬라이스는 ```String```의 일부를 가리키는 **참조자**
문자열 슬라이스를 나타내는 타입은 ```&str```로 작성

```rust
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
```
![](https://velog.velcdn.com/images/tollea1234/post/67017830-6a29-4666-b37e-92a6467b98fd/image.png)

### 슬라이스로써의 문자열 리터럴
```rust
let s = "Hello, world!"
```
```s```는 바이너리의 특정 지점을 가리키는 슬라이스므로 ```&str```타입. `&str`은 불변 참조자이므로 문자열 리터럴은 변경할 수 없다.

```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}


fn main() {
    let my_string = String::from("hello world");

    // `first_word`는 `String`의 일부 혹은 전체 슬라이스에 대해 작동합니다
    let word = first_word(&my_string[0..6]);
    let word = first_word(&my_string[..]);
    // 또한 `first_word`는 `String`의 전체 슬라이스와 동일한 `String`의
    // 참조자에 대해서도 작동합니다
    let word = first_word(&my_string);

    let my_string_literal = "hello world";

    // `first_word`는 문자열 리터럴의 일부 혹은 전체 슬라이스에 대해 작동합니다
    let word = first_word(&my_string_literal[0..6]);
    let word = first_word(&my_string_literal[..]);

    // 문자열 리터럴은 *곧* 문자열 슬라이스이므로,
    // 아래의 코드도 슬라이스 문법 없이 작동합니다!
    let word = first_word(my_string_literal);
}
```


### 그 외 슬라이스
```rust
let a = [1,2,3,4,5]
let slice = &a[1..3];
