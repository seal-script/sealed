# 语法提案


## 函数
```scala
let factorial(n: Natural): Fix[Natural] = { // symbol `=` is optional
    if n == 0 then 1 else n * factorial(n - 1)
};

// or
let factorial(n: Natural): Fix[Natural];
let factorial(n) = {
    // ...
};
```

## 绑定
```scala
let var = factorial(5);
let var { factorial(5) };
```

## 抽象
### 数据
```scala
struct Person {
    name: String,
    age: Natural,
    id: Int,
}

seal Person {
    getId(self): Int { // methods
        self.id
    }
}

enum Error {
    IOError(s: Message),
    ParsingError(rest: String),
}

```

### 类型类（Typeclass）
```scala
class StrictEqual[A] {
    (==)(a: A, b: A): Bool;
}

seal StrictEqual for Int {
    (a == b) = {
        a - b == 0
    }
}



class Equal {
    equals(self, other: Self): Bool;
}
```

## 类型声明
```scala
let fib(n: Int): Int;
let (>>=)[F, A, B](fa: F[A])(f: A -> F[B]): F[B];
```

## 计算效应
```scala
class FallableOps[F] {
    getMessage(): F[Message];
}

let useEffect[F](): F[String] 
    where
        FallableOps[F]
{
    msg <- getMessage();
    msg.toString()
}

let tryEffect_coroutine() {
    try useEffect[IO]()
    catch FallableOps[IO] {
        getMessage() = IO.of {
            "Hello there!"
        }
    }
}

let tryEffect_DI() {
    seal FallableOps[IO] {
        getMessage() = IO.of {
            "Hello there!"
        }
    }
    let s = useEffect[IO]();
    s
}
```









