# 03-02 Data Types

러스트에서 사용되는 모든 값들은 특정한 *타입*을 갖는다. 타입은 데이터를 어떻게 다룰지 다룰지 알 수 있도록 러스트에게 어떤 유형의 데이터를 다루는 것인지 알려준다. 여기서 두 종류의 데이터 타입 하위 집합을 다룰 것이다 : 스칼라와 컴파운드.

러스트는 *정적 타입* 언어라는 것을 계속 염두에 두어야 한다. 이 말은 러스트가 모든 변수의 타입을 컴파일 타임에 알고 있어야 한다는 뜻이다. 컴파일러는 대체로 사용하는 값과 변수를 어떻게 사용하는지를 바탕으로 타입을 추론할 수 있다. 다만 여러 가지 타입이 가능한 경우에, 예를 들어 2장에서 `String`을 `parse`를 이용하여 숫자 타입으로 바꾸었을 때 처럼, 타입 어노테이션은 반드시 추가되어야 한다.

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

만약 여기 타입 어노테이션을 달아놓지 않는다면, 러스트는 다음과 같은 오류를 낼 것이다. 이 오류는 우리가 어떤 타입을 사용하길 원하는지 알기 위해서 컴파일러에게 더 많은 정보를 제공해줘야 함을 뜻한다.

```bash
error[E0282]: type annotations needed
 --> src/main.rs:2:9
  |
2 |     let guess = "42".parse().expect("Not a number!");
  |         ^^^^^
  |         |
  |         cannot infer type for `_`
  |         consider giving `guess` a type
```

다른 데이터 타입의 경우 다른 타입 어노테이션을 볼 수 있다.

## Scalar Types

### Integer
### Floating-Type
### Numeric Operations
### Booelan
### Character

## compound Types

### Tuple
### Array
