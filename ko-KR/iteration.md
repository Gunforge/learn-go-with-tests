<!-- # Iteration -->
# 이터레이션

<!-- **[You can find all the code for this chapter here](https://github.com/quii/learn-go-with-tests/tree/master/for)** -->
**[이 챕터에서 사용된 코드는 여기서 다운로드 받을 수 있습니다](https://github.com/quii/learn-go-with-tests/tree/master/for)**

<!-- To do stuff repeatedly in Go, you'll need `for`. In Go there are no `while`, `do`, `until` keywords, you can only use `for`. Which is a good thing! -->
Go에서 어떤 작업을 반복하기 위해서는 `for`문을 사용해야 합니다. Go에서는 `while`, `do`, `until`같은 키워드들이 없기 때문에 `for`만 써야 합니다.

<!-- Let's write a test for a function that repeats a character 5 times. -->
문자를 5번 반복해서 출력하는 함수에 대한 테스트를 작성합니다.

<!-- There's nothing new so far, so try and write it yourself for practice. -->
전에까지 한 것과 크게 달라지는 점은 없기 때문에 연습삼아서 스스로 해 보는 것을 추천합니다.

<!-- ## Write the test first -->
## 테스트 먼저 작성

```go
package iteration

import "testing"

func TestRepeat(t *testing.T) {
    repeated := Repeat("a")
    expected := "aaaaa"

    if repeated != expected {
        t.Errorf("expected %q but got %q", expected, repeated)
    }
}
```

<!-- ## Try and run the test -->
## 테스트 실행

`./repeat_test.go:6:14: undefined: Repeat`

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 테스트를 통과시키기 위한 최소한의 코드를 작성한뒤 출력을 확인

<!-- _Keep the discipline!_ You don't need to know anything new right now to make the test fail properly. -->
_규율을 지키세요_. 테스트를 실패시키기 위해서 새로 배워야 할 사항은 없습니다.

<!-- All you need to do right now is enough to make it compile so you can check your test is written well. -->
지금 해야 할 것은 테스트가 잘 짜였는지 확인하기 위해서 컴파일 되는지 확인 하는 것 입니다.

```go
package iteration

func Repeat(character string) string {
    return ""
}
```

<!-- Isn't it nice to know you already know enough Go to write tests for some basic problems? This means you can now play with the production code as much as you like and know it's behaving as you'd hope. -->
간단한 문제에 대한 테스트를 Go로 작성하는데 충분한 지식을 이미 가지고 있다는 것이 놀랍지 않나요? 이는 실전에서도 생각한대로 동작하는 코드를 작성할 수 있음을 의미합니다.

`repeat_test.go:10: expected 'aaaaa' but got ''`

<!-- ## Write enough code to make it pass -->
## 테스트를 패스 시키기 위한 최소한의 코드 작성

The `for` syntax is very unremarkable and follows most C-like languages.
`for`은 C와 같은 언어들과 동일하게 동작합니다.

```go
func Repeat(character string) string {
    var repeated string
    for i := 0; i < 5; i++ {
        repeated = repeated + character
    }
    return repeated
}
```

<!-- Unlike other languages like C, Java, or JavaScript there are no parentheses surrounding the three components of the for statement and the braces { } are always required. You might wonder what is happening in the row -->
하지만 C, Java, Javscript 같은 언어들과는 다르게 괄호를 사용하지 않고, 중괄호는 상황과 관계없이 언제나 사용해야 합니다.

```go
    var repeated string
```

<!-- as we've been using `:=` so far to declare and initializing variables. However, `:=` is simply [short hand for both steps](https://gobyexample.com/variables). Here we are declaring a `string` variable only. Hence, the explicit version. We can also use `var` to declare functions, as we'll see later on. -->
지금 까지는 변수를 선언하고 초기화 하기 위해서 `:=`를 사용해 왔습니다. 하지만, `:=`는 단순히 [두 작업에 대한 단축어](https://gobyexample.com/variables)에 불과합니다. 여기서는 `string`변수를 선언만 하고 있습니다. 이쪽이 좀 더 명시적인 방법입니다. 나중에 확인 할 것이지만 `var`는 함수를 선언하는데에도 사용할 수 있습니다.

<!-- Run the test and it should pass. -->
테스트를 실행하면 패스할 겁니다.

<!-- Additional variants of the for loop are described [here](https://gobyexample.com/for). -->
for문을 어떻게 쓰는지 좀 더 자세하게 확인하고 싶다면 [여기](https://gobyexample.com/for)를 확인하세요

<!-- ## Refactor -->
## 리팩토링

<!-- Now it's time to refactor and introduce another construct `+=` assignment operator. -->
리팩토링을 하면서 `+=` 라는 대입 연산자를 소개합니다.

```go
const repeatCount = 5

func Repeat(character string) string {
    var repeated string
    for i := 0; i < repeatCount; i++ {
        repeated += character
    }
    return repeated
}
```

<!-- `+=` the Add AND assignment operator, adds the right operand to the left operand and assigns the result to left operand. It works with other types like integers. -->
덧셈과 대입을 동시에 하는 `+=` 연산자는 오른쪽 피연산자를 왼쪽 피연산자에 더하고 그 결과를 왼쪽 피연산자에 대입합니다. 이 연산자는 int 와 같은 다른 타입에서도 똑같이 동작합니다.

<!-- ### Benchmarking -->
### 벤치마킹

<!-- Writing [benchmarks](https://golang.org/pkg/testing/#hdr-Benchmarks) in Go is another first-class feature of the language and it is very similar to writing tests. -->
Go에서 [벤치마크](https://golang.org/pkg/testing/#hdr-Benchmarks)를 작성하는 기능은 언어에 포함된 first-class 기능입니다. 테스트와 사용하는 방법은 거의 유사합니다. 

```go
func BenchmarkRepeat(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Repeat("a")
    }
}
```

<!-- You'll see the code is very similar to a test. -->
코드의 형식이 테스트와 매우 비슷한 것을 확인 할 수 있습니다.

<!-- The `testing.B` gives you access to the cryptically named `b.N`. -->
`testing.B`는 암호같은 `b.N`을 제공합니다.

<!-- When the benchmark code is executed, it runs `b.N` times and measures how long it takes. -->
벤치마크 코드가 실행되면, 이 코드는 `b.N`번 실행되고 얼마나 걸렸는지 측정하게 됩니다.

<!-- The amount of times the code is run shouldn't matter to you, the framework will determine what is a "good" value for that to let you have some decent results. -->
프레임워크가 알아서 "괜찮은" 횟수를 정해서 실행시켜 주기때문에, 코드를 쓸때는 신경쓰지 않아도 됩니다.

<!-- To run the benchmarks do `go test -bench=.` (or if you're in Windows Powershell `go test -bench="."`) -->
벤치마크를 실행시키기 위해서는 `go test -bench=.`를 실행힙나다 (윈도우 파워쉘 환경에서는 `go test -bench="."`)

```text
goos: darwin
goarch: amd64
pkg: github.com/quii/learn-go-with-tests/for/v4
10000000           136 ns/op
PASS
```

<!-- What `136 ns/op` means is our function takes on average 136 nanoseconds to run \(on my computer\). Which is pretty ok! To test this it ran it 10000000 times. -->
`136ns/op`가 의미하는 바는 \(제 컴퓨터에서\) 우리의 함수가 실행되는데 평균 136 nanoseconds (나노초) 가 걸렸다는 것을 의미합니다. 꽤 괜찮은 숫자네요! 이를 테스트하기 위해서 10000000 번 실행된 것을 알 수 있습니다.

<!-- _NOTE_ by default Benchmarks are run sequentially. -->
_주의_ 기본값은 벤치마크들이 순서대로 실행되도록 되어있습니다.

## Practice exercises

* Change the test so a caller can specify how many times the character is repeated and then fix the code
* Write `ExampleRepeat` to document your function
* Have a look through the [strings](https://golang.org/pkg/strings) package. Find functions you think could be useful and experiment with them by writing tests like we have here. Investing time learning the standard library will really pay off over time.

## Wrapping up

* More TDD practice
* Learned `for`
* Learned how to write benchmarks
