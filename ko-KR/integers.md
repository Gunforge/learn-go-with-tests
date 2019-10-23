<!-- # Integers -->
# 정수

<!-- **[You can find all the code for this chapter here](https://github.com/quii/learn-go-with-tests/tree/master/integers)** -->
**[이번 장의 모든 코드는 이곳에서 확인할 수 있습니다](https://github.com/quii/learn-go-with-tests/tree/master/integers)**

<!-- Integers work as you would expect. Let's write an add function to try things out. Create a test file called `adder_test.go` and write this code. -->
정수는 우리가 기대한대로 작동합니다. 뭔가 해보기 위해 덧셈 함수를 작성해봅시다. 테스트 파일 `adder_test.go`를 만들고 이 코드를 작성합니다.

<!-- **note:** Go source files can only have one `package` per directory, make sure that your files are organised separately. [Here is a good explanation on this.](https://dave.cheney.net/2014/12/01/five-suggestions-for-setting-up-a-go-project) -->
**주목:** Go 소스 파일은 한 디렉토리 당 하나의 `package`를 가지니, 파일을 분리하여 관리하십시오. [여기 이에 대한 좋은 예시가 있습니다.](https://dave.cheney.net/2014/12/01/five-suggestions-for-setting-up-a-go-project)

<!-- ## Write the test first -->
## 먼저 테스트를 작성하세요

```go
package integers

import "testing"

func TestAdder(t *testing.T) {
    sum := Add(2, 2)
    expected := 4

    if sum != expected {
        t.Errorf("expected '%d' but got '%d'", expected, sum)
    }
}
```

<!-- You will notice that we're using `%d` as our format strings rather than `%s`. That's because we want it to print an integer rather than a string. -->
포맷 스트링으로 `%s` 대신 `%d`를 사용하는 걸 눈치챘을 겁니다. 바로 우리가 문장 대신 정수를 출력하길 원하기 때문입니다.

<!-- Also note that we are no longer using the main package, instead we've defined a package named integers, as the name suggests this will group functions for working with integers such as Add. -->
또한 우리가 더이상 main 패키지를 사용하지 않는 점에 주목하세요. 대신에, 우리는 integers라는 패키지를 정의했고, 이름이 의미하는 것처럼 이 패키지는 Add 같은 정수를 다루는 함수의 모음입니다.

<!-- ## Try and run the test -->
## 테스트 실행을 시도하세요

<!-- Run the test `go test` -->
`go test`를 사용하여 테스트를 작동하세요

<!-- Inspect the compilation error -->
컴파일 에러를 점검하세요

`./adder_test.go:6:9: undefined: Add`

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 실패한 테스트 출력을 실행하고 점검하기 위해 테스트에는 적은 양의 코드 작성하세요

<!-- Write enough code to satisfy the compiler _and that's all_ - remember we want to check that our tests fail for the correct reason. -->
컴파일 에러를 해결할 충분한 코드를 작성하세요 _그리고 그게 다에요_ - 우리가 원하는 건 테스트가 알맞은 이유로 실패하는지 점검하는 것입니다.

```go
package integers

func Add(x, y int) int {
    return 0
}
```

<!-- When you have more than one argument of the same type \(in our case two integers\) rather than having `(x int, y int)` you can shorten it to `(x, y int)`. -->
같은 타입의 변수를 하나 이상 가진다면, \(이 경우에는 두 개의 정수)\ `(x int, y int)` 대신 `(x, y int)`처럼 줄일 수 있습니다.

<!-- Now run the tests and we should be happy that the test is correctly reporting what is wrong. -->
이제 테스트를 실행하면 기쁘게도, 테스트가 무엇이 잘못되었는지 올바르게 알려줄 것입니다.

`adder_test.go:10: expected '4' but got '0'`

<!-- If you have noticed we learnt about _named return value_ in the [last](hello-world.md#one...last...refactor?) section but aren't using the same here. It should generally be used when the meaning of the result isn't clear from context, in our case it's pretty much clear that `Add` function will add the parameters. You can refer [this](https://github.com/golang/go/wiki/CodeReviewComments#named-result-parameters) wiki for more details. -->
여기에는 우리가 [저번](hello-world.md#one...last...refactor?) 섹션에서 배운 _named return value_를 사용하지 않았습니다. 이는 일반적으로 결과의 의미가 코드 안에서 명확하지 않을 때 사용되는데, 우리의 경우 `Add` 함수가 파라미터를 더한다는 게 확실하여 사용하지 않았습니다. 자세한 내용은 이 위키를 참조하십시오.

<!-- ## Write enough code to make it pass -->
## 테스트를 통과할 충분한 코드를 작성하세요

<!-- In the strictest sense of TDD we should now write the _minimal amount of code to make the test pass_. A pedantic programmer may do this -->
가장 엄격한 TDD 감각으로 우리는 이제 _테스트를 통과하는 가장 적은 양의 코드_ 작성해야 합니다. 형식주의적인 프로그래머는 아마 이렇게 할 것입니다.

```go
func Add(x, y int) int {
    return 4
}
```

<!-- Ah hah! Foiled again, TDD is a sham right? -->
미쳤습니까 휴먼? TDD가 장난입니까?

<!-- We could write another test, with some different numbers to force that test to fail but that feels like a game of cat and mouse. -->
우리는 다른 숫자를 출력해야 하는, 테스트를 실패하게 하는 테스트를 더 작성할 수 있습니다.

Once we're more familiar with Go's syntax I will introduce a technique called Property Based Testing, which would stop annoying developers and help you find bugs.
우리가 Go의 문법에 더 친숙하다면 나는 Property Based Testing을 소개할 것입니다. 개발자를 괴롭히지 않고, 버그 찾는 것을 도울

<!-- For now, let's fix it properly -->
이제, 적절하게 고쳐봅시다.

```go
func Add(x, y int) int {
    return x + y
}
```

<!-- If you re-run the tests they should pass. -->
다시 동작해보면, 테스트를 통과할 것입니다.

<!-- ## Refactor -->
## 리팩토링

<!-- There's not a lot in the _actual_ code we can really improve on here. -->
_실제_ 코드에는 개선할 점이 별로 없습니다.

<!-- We explored earlier how by naming the return argument it appears in the documentation but also in most developer's text editors. -->
반환 값이 문서와 텍스트 에디터에서 어떻게 보일지는 이전에 다루었습니다.

<!-- This is great because it aids the usability of code you are writing. It is preferable that a user can understand the usage of your code by just looking at the type signature and documentation. -->
이런 방식은 작성중인 코드의 재사용성을 돕기 때문에 좋습니다. 사용자가 반환 타입과 문서만 봐도 당신 코드의 사용법을 이해할 수 있기 때문에 선호할만하다.

<!-- You can add documentation to functions with comments, and these will appear in Go Doc just like when you look at the standard library's documentation. -->
당신은 함수에 주석으로 문서를 추가할 수 있고, 이러한 주석은 Go Doc에서 기본 라이브러리 문서와 같은 형태로 보일 것입니다.

```go
// Add takes two integers and returns the sum of them
func Add(x, y int) int {
    return x + y
}
```

<!-- ### Examples -->
### 예제

<!-- If you really want to go the extra mile you can make [examples](https://blog.golang.org/examples). You will find a lot of examples in the documentation of the standard library. -->
좀더 나아가길 원한다면 [예제](https://blog.golang.org/examples)를 만들 수 있습니다. 기본 라이브러리 문서에서 많은 예제를 찾을 수 있습니다.

<!-- Often code examples that can be found outside the codebase, such as a readme file often become out of date and incorrect compared to the actual code because they don't get checked. -->
종종 readme 같은, 코드베이스 바깥의 코드 예제는 관리되지 않기 때문에 최신이 아닐 수 있으며, 실제 코드와 비교했을 때 부정확할 수 있습니다.

<!-- Go examples are executed just like tests so you can be confident examples reflect what the code actually does. -->
Go 예제는 테스트와 같은 방식으로 실행되기 때문에, 예제가 어떤 실제 코드를 반영했는지 확인할 수 있습니다.

<!-- Examples are compiled \(and optionally executed\) as part of a package's test suite. -->
예제는 패키지 테스트 묶음으로 컴파일됩니다. \(그리고 종종 실행됩니다.\)

As with typical tests, examples are functions that reside in a package's \_test.go files. Add the following ExampleAdd function to the `adder_test.go` file.
보편적인 테스트와 마찬가지로, 예제 또한 패키지 안 \_test.go 파일에 존재하는 함수입니다. 아래 ExampleAdd 함수를 `adder_test.go` 파일에 추가하십시오.

```go
func ExampleAdd() {
    sum := Add(1, 5)
    fmt.Println(sum)
    // Output: 6
}
```

<!-- (If your editor doesn't automatically import packages for you, the compilation step will fail because you will be missing `import "fmt"` in `adder_test.go`. It is strongly recommended you research how to have these kind of errors fixed for you automatically in whatever editor you are using.) -->
(만약 텍스트 에디터가 패캐지를 자동으로 추가해주지 않는다면, `adder_test.go`에 `import "fmt"` 명령이 없어 컴파일에 실패할 것입니다. 사용하는 텍스트 에디터에서 이러한 에러를 자동으로 바로잡도록 하는 방법을 검색하십시오.)

<!-- If your code changes so that the example is no longer valid, your build will fail. -->
코드를 변경하여 예시가 더이상 유효하지 않다면, 빌드에 실패할 것입니다.

<!-- Running the package's test suite, we can see the example function is executed with no further arrangement from us: -->
패키지의 테스트 묶음을 돌리면, 어떤 요소도 요구하지 않고 예제 함수가 실행됩니다.

```bash
$ go test -v
=== RUN   TestAdder
--- PASS: TestAdder (0.00s)
=== RUN   ExampleAdd
--- PASS: ExampleAdd (0.00s)
```

<!-- Please note that the example function will not be executed if you remove the comment "//Output: 6". Although the function will be compiled, it won't be executed. -->
주석 "//Output: 6"를 지우면, 예제 함수는 실행되지 않습니다. 여전히 함수는 컴파일되지만, 실행되진 않습니다.

<!-- By adding this code the example will appear in the documentation inside `godoc`, making your code even more accessible. -->
이 코드를 추가하면 예제는 `godoc` 내부 문서에 표시되고, 코드의 접근성 또한 높아집니다.

<!-- To try this out, run `godoc -http=:6060` and navigate to `http://localhost:6060/pkg/` -->
이를 시도해보려면, `godoc -http=:6060`을 실행하고 `http://localhost:6060/pkg/`에 접속해보세요.

<!-- Inside here you'll see a list of all the packages in your `$GOPATH`, so assuming you wrote this code in somewhere like `$GOPATH/src/github.com/{your_id}` you'll be able to find your example documentation. -->
이 사이트에서 `$GOPATH` 내부 모든 패키지가 보일 것입니다. 때문에 만약 당신이 `$GOPATH/src/github.com/{your_id}` 같은 경로에 코드를 작성했다면, 예제 문서를 찾을 수 있을 것입니다.

<!-- If you publish your code with examples to a public URL, you can share the documentation of your code at [godoc.org](https://godoc.org). For example, here is the finalised API for this chapter [https://godoc.org/github.com/quii/learn-go-with-tests/integers/v2](https://godoc.org/github.com/quii/learn-go-with-tests/integers/v2). -->
예제와 코드를 퍼블릭 URL로 발행한다면, 코드에 대한 문서를 [godoc.org](https://godoc.org)에서 공유할 수 있습니다. 예를 들어, 여기에 이번 장의 완성된 API가 있습니다. [https://godoc.org/github.com/quii/learn-go-with-tests/integers/v2](https://godoc.org/github.com/quii/learn-go-with-tests/integers/v2)

<!-- ## Wrapping up -->
## 마무리하며

<!-- What we have covered: -->
무엇을 다뤘는가:

<!-- * More practice of the TDD workflow
* Integers, addition
* Writing better documentation so users of our code can understand its usage quickly
* Examples of how to use our code, which are checked as part of our tests -->
* TDD 작업 과정을 더 연습
* 정수, 더하기
* 사용자의 빠른 이해를 위한 더나은 문서 작성법
* 테스트로 검증되는 코드 사용 예제