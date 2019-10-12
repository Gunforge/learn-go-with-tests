<!-- # Hello, World -->
# 헬로 월드 (Hello, World)

<!-- **[You can find all the code for this chapter here](https://github.com/quii/learn-go-with-tests/tree/master/hello-world)** -->
**[이 챕터에서 사용한 코드는 이 링크에서 다운로드 할 수 있어요.](https://github.com/quii/learn-go-with-tests/tree/master/hello-world)**


<!-- It is traditional for your first program in a new language to be Hello, world. -->
새로운 프로그래밍 언어에 입문할때는 보통 Hello, world를 먼저 출력해보죠.

<!-- In the [previous chapter](install-go.md#go-environment) we discussed how Go is opinionated as to where you put your files. -->
[이전 챕터](install-go.md#go-environment)에서 우리는 Go를 사용할때 파일을 어떻게 정리해야 하는지 봤습니다.

<!-- Make a directory in the following path `$GOPATH/src/github.com/{your-user-id}/hello`. -->
`$GOPATH/src/github.com/{your-user-id}/hello` 에 디렉토리를 만듭니다.

<!-- So if you're on a unix based OS and your username is "bob" and you are happy to stick with Go's conventions about `$GOPATH` (which is the easiest way of setting up) you could run `mkdir -p $GOPATH/src/github.com/bob/hello`. -->
유닉스 계열 OS를 사용하고 있고, 유저이름이 "bob"이라면 `$GOPATH`를 사용해서 `mkdir -p $GOPATH/src/github.com/bob/hello`로 편리하게 디렉토리를 생성할 수 있습니다.

<!-- For subsequent chapters, you can make a new folder with whatever name you like to put the code in e.g `$GOPATH/src/github.com/{your-user-id}/integers` for the next chapter might be sensible. Some readers of this book like to make an enclosing folder for all the work such as "learn-go-with-tests/hello". In short, it's up to you how you structure your folders. -->
이 챕터 이후로는 각각에 별도의 이름을 붙여서 디렉토리를 만드는 것을 권장합니다. 다음 챕터를 예시로 들면 `면GOPATH/src/github.com/{your-user-id}/integers`와 같은 형식이 될 수 있습니다. 어떤 유저들은 모든 파일을 "lear을-go-with-tests/hello" 안에 넣기도 합니다. 결론적으로 어떻게 하는지는 여러분이 결정하시면 됩니다.

<!-- Create a file in this directory called `hello.go` and write this code. To run it type `go run hello.go`. -->
생성한 디렉토리에 `hello.go` 파일을 생성하고 아래의 코드를 씁니다. 실행시키기 위해서는 `go run hello.go` 커맨드를 입력합니다.

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, world")
}
```

<!-- ## How it works -->
## 동작 원리

<!-- When you write a program in Go you will have a `main` package defined with a `main` func inside it. Packages are ways of grouping up related Go code together. -->
Go에서 프로그램을 작성할때는 `main` 패키지 안에 `main` 함수를 정의해야 합니다. 패키지는 Go의 코드들을 그룹으로 묶는 역할을 합니다.

<!-- The `func` keyword is how you define a function with a name and a body. -->
`func` 키워드는 함수의 이름과 내용을 정의합니다.

<!-- With `import "fmt"` we are importing a package which contains the `Println` function that we use to print. -->
`import "fmt"`로 출력을 위한 `Println` 함수를 포함하고 있는 패키지를 가져올 수 있습니다.

<!-- ## How to test -->
# 테스트 작성법

<!-- How do you test this? It is good to separate your "domain" code from the outside world \(side-effects\). The `fmt.Println` is a side effect \(printing to stdout\) and the string we send in is our domain. -->
이 코드를 어떻게 테스트 할까요? 보통 코드를 \(부작용\)으로부터 보호하기 위해서 외부와 분리하는 것이 좋습니다. `fmt.Println`는 일종의 부작용으로서 \(stdout에 출력을 합니다\). 여기서 우리가 이 함수에 넘기는 스트링은 저희의 도메인 코드입니다.

<!-- So let's separate these concerns so it's easier to test -->
그러므로 테스트를 작성하기 위해서는 이 둘을 분리해야 합니다.

```go
package main

import "fmt"

func Hello() string {
    return "Hello, world"
}

func main() {
    fmt.Println(Hello())
}
```

<!-- We have created a new function again with `func` but this time we've added another keyword `string` in the definition. This means this function returns a `string`. -->
`func`키워드로 새 합수를 작성하고 함수의 정의에 `string` 을 보합시켰습니다. 이는 함수가 `strin가`를 반환하는 것을 의미합니다.

<!-- Now create a new file called `hello_test.go` where we are going to write a test for our `Hello` function -->
`Hello` 함수에 대한 테스트 코드를 작성하기 위해 `hello_test.go`라는 새 파일을 만듭니다.

```go
package main

import "testing"

func TestHello(t *testing.T) {
    got := Hello()
    want := "Hello, world"

    if got != want {
        t.Errorf("got %q want %q", got, want)
    }
}
```

<!-- Before explaining, let's just run the code. Run `go test` in your terminal. It should've passed! Just to check, try deliberately breaking the test by changing the `want` string. -->
설명하기 전에 먼저 코드를 실행시켜 봅시다. `go test`를 터미널에서 실행시킵니다. 통과할 것입니다. 확인 하기 위해서 `want` 스트링을 바꿔서 테스트를 실패시켜 봅시다.

<!-- Notice how you have not had to pick between multiple testing frameworks and then figure out how to install. Everything you need is built in to the language and the syntax is the same as the rest of the code you will write. -->
테스트를 작성하기 위해서 프레임 워크들을 비교하고 골라서 설치할 필요가 없습니다. 필요한 대부분의 기능은 이처럼 언어 안데 거의 포함되어서 신택스의 일관성을 지킬 수 있습니다.

<!-- ### Writing tests -->
### 테스트 쓰기

<!-- Writing a test is just like writing a function, with a few rules -->
몇가지 규칙을 준수해야 한다는 점을 제외하면 테스트를 작성하는 것은 일반적인 함수를 작성하는 것과다동일합니다

<!-- * It needs to be in a file with a name like `xxx_test.go` -->
* 파일네임은 `xxx_test.go`와 같은 형식이어야 합니다.
<!-- * The test function must start with the word `Test` -->
* 테스트 함수는 반드시 `Test`로 시작해야 합니다.
<!-- * The test function takes one argument only `t *testing.T` -->
* 테스트 함수는 반드시 인자로서 `t *testing.T` 하나만 받아야 합니다.

<!-- For now it's enough to know that your `t` of type `*testing.T` is your "hook" into the testing framework so you can do things like `t.Fail()` when you want to fail. -->
현시점으로서는 `*testing.T` 타입의 `t`가 `t.Fail()`와 같이 테스트 프레임 워크를 사용하는 "훅"이라고 이해하시면 됩니다.

<!-- We've covered some new topics: -->
우리는 여기서 몇가지 새로운 것들을 사용했습니다: 

<!-- #### `if` -->
#### `if`
<!-- If statements in Go are very much like other programming languages. -->
if문은 Go에서도 다른 프로그래밍 언어들과 같은 의미를 가집니다.

<!-- #### Declaring variables -->
#### 변수 선언

<!-- We're declaring some variables with the syntax `varName := value`, which lets us re-use some values in our test for readability. -->
`varName := vale`라는 새로운 신텍스를 사용해서 변수를 선언했습니다. 이는 값들을 재이용 함으로서 테스트의 가독성을 높입니다.

<!-- #### `t.Errorf` -->
#### `t.Errorf`

<!-- We are calling the `Errorf` _method_ on our `t` which will print out a message and fail the test. The `f` stands for format which allows us to build a string with values inserted into the placeholder values `%q`. When you made the test fail it should be clear how it works. -->
메시지를 출력하고 테스트를 실패시키기 위해서 `t`의 `Error의` _메소드_ 를 호출 했습니다. `f`는 `%q`를 사용해서 값들을 스트링 안에 하드 코드하는 것을 방지하는 포멧을 의미합니다. 테스트가 실패했을때 이 메소드의 의미는 분명해집니다. 

<!-- You can read more about the placeholder strings in the [fmt go doc](https://golang.org/pkg/fmt/#hdr-Printing). For tests `%q` is very useful as it wraps your values in double quotes. -->
[fmt go doc](https://golang.org/pkg/fmt/#hdr-Printing)에서 스트링에 대해서 더 읽을 수 있습니다. 테스트에서는 쌍따옴표안에 있는 값들을 표현하는 `%q`가 유용합니다.

<!-- We will later explore the difference between methods and functions. -->
나중에는 메소드와 함수의 차이에 대해서 이야기 할 것입니다.

<!-- ### Go doc -->
### Go 도큐먼트

<!-- Another quality of life feature of Go is the documentation. You can launch the docs locally by running `godoc -http :8000`. If you go to [localhost:8000/pkg](http://localhost:8000/pkg) you will see all the packages installed on your system. -->
Go에서 유용한 또다른 기능은 도큐먼트 입니트. `godoc -http :8000`을 실행시키는 것으로 도큐먼트를 로컬에서 실행시킬 수 있습니다. [localhost:8000/pkg](http://localhost:8000/pkg)에 들어가면 여러분의 시스템에 설치된 패키지들을 확인 할 수 있습니다.

<!-- The vast majority of the standard library has excellent documentation with examples. Navigating to [http://localhost:8000/pkg/testing/](http://localhost:8000/pkg/testing/) would be worthwhile to see what's available to you. -->
표준 라이브러리 대부분은 좋은 예제를 포함한 도큐먼트를 포함하고 있습니다. [http://localhost:8000/pkg/testing/](http://localhost:8000/pkg/testing/)에 들어가면 테스트 프레임워크에서 어떤 것들이 가능한지 확인 할 수 있습니다.

<!-- ### Hello, YOU -->
### Hello, YOU

<!-- Now that we have a test we can iterate on our software safely. -->
이제 우리는 프로그램을 대상으로 안전하게 실행가능한 테스트를 가지고 있습니다.

<!-- In the last example we wrote the test _after_ the code had been written just so you could get an example of how to write a test and declare a function. From this point on we will be _writing tests first_. -->
마지막 예시에서 우리는 테스트를 어떻게 작성하고 함수를 어떻게 선언하는지 보기 위해 테스트를 코드를 작성한 _다음에_ 정의했습니다. 지금부터는 _테스트를 먼저_ 정의할 것입니다. 

<!-- Our next requirement is to let us specify the recipient of the greeting. -->
다음 요구사항은 인사를 하는 대상을 특정하는 것입니다.

<!-- Let's start by capturing these requirements in a test. This is basic test driven development and allows us to make sure our test is _actually_ testing what we want. When you retrospectively write tests there is the risk that your test may continue to pass even if the code doesn't work as intended. -->
이 요구사항을 테스트에 반영하는 것에서 시작합니다. 이는 테스트가 _실제로_ 우리가 원하는 사항을 테스트 하도록하는 테스트 주도 개발의 기본 방식입니다. 코드를 작성한 뒤에 테스트를 작성하면, 의도와는 다른 기능을 테스트 할 위험이 있습니다.

```go
package main

import "testing"

func TestHello(t *testing.T) {
    got := Hello("Chris")
    want := "Hello, Chris"

    if got != want {
        t.Errorf("got %q want %q", got, want)
    }
}
```

<!-- Now run `go test`, you should have a compilation error -->
이제 `go test`를 실행하면, 다음과 같은 컴파일 에러가 발생할 것입니다.

```text
./hello_test.go:6:18: too many arguments in call to Hello
    have (string)
    want ()
```

<!-- When using a statically typed language like Go it is important to _listen to the compiler_. The compiler understands how your code should snap together and work so you don't have to. -->
Go와 같은 정적 타입 언어를 사용할 때는 _컴파일러의 출력을 예의주시_ 하는 것이 중요합니다. 컴파일러는 코드가 어떻게 연결되어야 하는지 이해해서 여러분이 그 작어블 하지 않아도 괜찮게 해 줍니다.

<!-- In this case the compiler is telling you what you need to do to continue. We have to change our function `Hello` to accept an argument. -->
이 경우에 컴파일러는 계속하기 위해서는 무엇을 해야 하는지 알려주고 있습니다. `Hello` 함수가 인수를 받도록 수정해야 합니다.

<!-- Edit the `Hello` function to accept an argument of type string -->
`Hello` 함수를 수정해서 스트링 타입의 인수를 받도록 만듭니다

```go
func Hello(name string) string {
    return "Hello, world"
}
```

<!-- If you try and run your tests again your `main.go` will fail to compile because you're not passing an argument. Send in "world" to make it pass. -->
다시 테스트를 실행시키면 파라미터를 건네지 않았기 때문에 `main.go`에서 컴파일이 실패합니다. "world"를 파라미터로 보내서 테스트가 통과하도록 만듭니다.

```go
func main() {
    fmt.Println(Hello("world"))
}
```

<!-- Now when you run your tests you should see something like -->
다시 테스트를 실행시키면 다음과 같은 문장이 출력될 겁니다.

```text
hello_test.go:10: got 'Hello, world' want 'Hello, Chris''
```

<!-- We finally have a compiling program but it is not meeting our requirements according to the test. -->
컴파일에는 성공했지만, 테스트에 따르면 프로그램의 동작이 요구사항에 맞춰져 있지 않습니다.

<!-- Let's make the test pass by using the name argument and concatenate it with `Hello,` -->
name 인수를 `Hello,`와 연결해서 테스트를 통과하게 만듭니다.

```go
func Hello(name string) string {
    return "Hello, " + name
}
```

<!-- When you run the tests they should now pass. Normally as part of the TDD cycle we should now _refactor_. -->
이제 테스트를 실행시키면 통과 할 것입니다. 테스트 주도 개발(이하 TDD)의 사이클에 따라서 다음에 해야할 일은 _리팩토링_ 입니다.

<!-- ### A note on source control -->
### 소스 컨트롤

<!-- At this point, if you are using source control \(which you should!\) I would -->
<!-- `commit` the code as it is. We have working software backed by a test. -->
소스 컨트롤을 사용하고 있다면 (\(사용해야만 합니다\)) 지금까지 작성한 코드를 `커밋`합니다.
현재 우리는 테스트로 검증된 동작하는 코드를 가지고 있습니다.

<!-- I _wouldn't_ push to master though, because I plan to refactor next. It is nice -->
<!-- to commit at this point in case you somehow get into a mess with refactoring - you can always go back to the working version. -->
하지만 리팩토링을 하지 않았기 때문에, 아직은 마스터 브랜치에 푸시하지 _않습니다_.
혹시 프로그램이 동작하지 않게 됐을때 되돌릴 수 있는 곳을 만들기 때문에 리팩토링 이전에 커밋하는 것을 추천합니다.

There's not a lot to refactor here, but we can introduce another language feature _constants_.
아직 리팩토링 할 것이 많지는 않지만, 언어의 새로운 기능인 _상수_ 를 소개합니다.

<!-- ### Constants -->
### 상수

<!-- Constants are defined like so -->
상수는 아래와 같이 정의 됩니다.

```go
const englishHelloPrefix = "Hello, "
```

<!-- We can now refactor our code -->
이제 우리는 코드를 아래처럼 리팩토링 할 수 있습니다.

```go
const englishHelloPrefix = "Hello, "

func Hello(name string) string {
    return englishHelloPrefix + name
}
```

<!-- After refactoring, re-run your tests to make sure you haven't broken anything. -->
리팩토링을 한 다음에는 테스트를 실행시켜서 프로그램이 제대로 동작하는지 확인합니다.

<!-- Constants should improve performance of your application as it saves you creating the `"Hello, "` string instance every time `Hello` is called. -->
상수는 `Hello`함수가 호출될 때마나 `"Hello, "`인스턴스가 생성되는 것을 방지하기 때문에 프로그램의 성능을 향상시킵니다.

<!-- To be clear, the performance boost is incredibly negligible for this example! But it's worth thinking about creating constants to capture the meaning of values and sometimes to aid performance. -->
이 예제에서의 성능 향상은 거의 없습니다. 하지만 성능 이외에도 어떤 값에 이름을 붙이는 것은 프로그램의 가독성을 높이기 때문에 추천되는 일입니다.

<!-- ## Hello, world... again -->
## 다시 Hello, world

<!-- The next requirement is when our function is called with an empty string it defaults to printing "Hello, World", rather than "Hello, ". -->
프로그램의 다음 요구사항은 우리의 함수가 인수 없이 호출되었을 때 "Hello, "대신 "Hello, World"가 출럭되게 하는 것입니다.

<!-- Start by writing a new failing test -->
실패하는 테스트를 작성하는 것에서 시작합니다.

```go
func TestHello(t *testing.T) {

    t.Run("saying hello to people", func(t *testing.T) {
        got := Hello("Chris")
        want := "Hello, Chris"

        if got != want {
            t.Errorf("got %q want %q", got, want)
        }
    })

    t.Run("say 'Hello, World' when an empty string is supplied", func(t *testing.T) {
        got := Hello("")
        want := "Hello, World"

        if got != want {
            t.Errorf("got %q want %q", got, want)
        }
    })

}
```

<!-- Here we are introducing another tool in our testing arsenal, subtests. Sometimes it is useful to group tests around a "thing" and then have subtests describing different scenarios. -->
여기서 우리는 subtests라는 새로운 테스팅 툴을 사용 했습니다. 상황에 따라서 테스트를 어떤 '그룹'으로 묶어서 다른 시나리오들을 테스트하는 서브테스트를 작성하는 것은 매우 유용합니다.

<!-- A benefit of this approach is you can set up shared code that can be used in the other tests. -->
그 장점중 하나는, 중복되는 부분의 코드를 다른 테스트에서도 사용할 수 있다는 점입니다.

<!-- There is repeated code when we check if the message is what we expect. -->
이 코드에서는 메시지가 예상한 값과 같은지 확인하는 부분이 중복되어 있습니다.

<!-- Refactoring is not _just_ for the production code! -->
리팩토링은 프로덕션 코드 _만_ 을 위한건 아니라는 것에 주의합니다.

<!-- It is important that your tests _are clear specifications_ of what the code needs to do. -->
테스트는 여러분이 하려고 하는 사항에 대한 _명확한 정의_ 여야 한다는 점에도 주의합니다. 

<!-- We can and should refactor our tests. -->
그러므로 우리는 테스트를 리팩토링 해야합니다.

```go
func TestHello(t *testing.T) {

    assertCorrectMessage := func(t *testing.T, got, want string) {
        t.Helper()
        if got != want {
            t.Errorf("got %q want %q", got, want)
        }
    }

    t.Run("saying hello to people", func(t *testing.T) {
        got := Hello("Chris")
        want := "Hello, Chris"
        assertCorrectMessage(t, got, want)
    })

    t.Run("empty string defaults to 'World'", func(t *testing.T) {
        got := Hello("")
        want := "Hello, World"
        assertCorrectMessage(t, got, want)
    })

}
```

<!-- What have we done here? -->
여기서 무엇을 한 걸까요?

<!-- We've refactored our assertion into a function. This reduces duplication and improves readability of our tests. In Go you can declare functions inside other functions and assign them to variables. You can then call them, just like normal functions. We need to pass in `t *testing.T` so that we can tell the test code to fail when we need to. -->
우리는 assertion들을 함수로 만듦으로서 리팩토링을 했습니다. 이는 중독을 줄여서 코드의 가독성을 높입니다. Go에서는 함수를 함수안에서 정의하고 변수 안에 대입하는 것도 가능합니다. 이렇게 대입된 함수는 다른 함수와 같은 방식으로 호출 할 수 있습니다. 테스트 조건에 실패했을 때 실패했다는 것을 알려주기 위해서 `t *testing.T`도 건냅니다.

<!-- `t.Helper()` is needed to tell the test suite that this method is a helper. By doing this when it fails the line number reported will be in our _function call_ rather than inside our test helper. This will help other developers track down problems easier. If you still don't understand, comment it out, make a test fail and observe the test output. -->
`t.Helper()`는 이 함수가 헬퍼 함수라는 것을 알리기 위해서 필요합니다. 이렇게 함으로서 테스트가 실패했을때 헬퍼 함수 안이 아니라 어떤 테스트가 실패했는지 제대로 출력할 수 있습니다. 이는 팀의 다른 개발자들이 에러를 더 빨리 발견할 수 있게 해줍니다. 아직 잘 모르겠다면, 테스트를 실패시켜봐서 어떻게 출력되는지 확인해 봅니다.

<!-- Now that we have a well-written failing test, let's fix the code, using an `if`. -->
이제 우리는 잘 짜인 실패하는 테스트를 가지고 있습니다, 이제 `if`를 사용해서 코드를 고칩니다.

```go
const englishHelloPrefix = "Hello, "

func Hello(name string) string {
    if name == "" {
        name = "World"
    }
    return englishHelloPrefix + name
}
```

<!-- If we run our tests we should see it satisfies the new requirement and we haven't accidentally broken the other functionality. -->
이제 테스트를 실행하면 우리는 새로운 요구사항을 구현했다는 사실과 동시에 다른 기능을 부수지 않았다는 것을 확인할 수 있습니다.

<!-- ### Back to source control -->
### 다시 소스 컨트롤

<!-- Now we are happy with the code I would amend the previous commit so we only -->
<!-- check in the lovely version of our code with its test. -->
코드에 문제가 없기 때문에 이전에 한 커밋을 그대로 둔채로 지금 상태를 커밋합니다.

<!-- ### Discipline -->
### 규율

<!-- Let's go over the cycle again -->
사이클을 다시 한번 확인해 봅시다.

<!-- * Write a test -->
* 테스트를 작성
<!-- * Make the compiler pass -->
* 컴파일 가능하게 코드를 작성
<!-- * Run the test, see that it fails and check the error message is meaningfu -->
* 테스트를 실행해서 에러 메시지가 말하는 정보를 확인
<!-- * Write enough code to make the test pass -->
* 테스트를 패스시키기 위한 코드를 작성
<!-- * Refactor -->
* 리팩토링

<!-- On the face of it this may seem tedious but sticking to the feedback loop is important. -->
처음 보면 어색하지만, 이 루프를 통해서 피드백을 지속적으로 받는게 중요합니다.

<!-- Not only does it ensure that you have _relevant tests_, it helps ensure _you design good software_ by refactoring with the safety of tests. -->
이 사이클을 지키면 프로그램과 _관련 있는 테스트_ 를 가지게 되는 것 이상으로 리팩토링을 지속적으로 함으로서 _좋은 소프트웨어 디자인_ 을 할 수 있게 됩니다. 

<!-- Seeing the test fail is an important check because it also lets you see what the error message looks like. As a developer it can be very hard to work with a codebase when failing tests do not give a clear idea as to what the problem is. -->
테스트를 실패시키는 것은, 에러메시지의 형식을 확인할 수 있게 해주는 면에서도 중요합니다. 프로그래머로서 실패하는 테스트가 무엇이 문제인지 보여주지 않으면 작업하는데 어려움을 겪기 때문입니다.

<!-- By ensuring your tests are _fast_ and setting up your tools so that running tests is simple you can get in to a state of flow when writing your code. -->
테스트를 간결하게 유지하면, 테스트를 실행시키는 속도가 _빨라지고_ 코드를 작성하는데 집중하기 쉬워집니다.

<!-- By not writing tests you are committing to manually checking your code by running your software which breaks your state of flow and you won't be saving yourself any time, especially in the long run. -->
테스트가 자동화 되어있지 않다면, 코드를 쓸떄마다 프로그램을 실행시켜서 의도한 대로 동작하는지 확인해야 할 것이고, 이는 집중을 흐트러뜨립니다.

<!-- ## Keep going! More requirements -->
## 요구 사항 추가하기

<!-- Goodness me, we have more requirements. We now need to support a second parameter, specifying the language of the greeting. If a language is passed in that we do not recognise, just default to English. -->
요구 사항이 늘어났습니다. 어떤 언어로 인사를 할지도 두번쨰 파라미터로 조정할 수 있어야 합니다. 언어를 지정하지 않았을 때는 영어를 기본값으로 합니다. 

<!-- We should be confident that we can use TDD to flesh out this functionality easily! -->
우리는 TDD로 이 요구사항을 쉽게 추가할 수 있습니다!

<!-- Write a test for a user passing in Spanish. Add it to the existing suite. -->
스페인어를 설정하는 유저에 대한 테스트를 작성합니다. 아래의 코드를 원래 있던 테스트에 추가합니다.

```go
    t.Run("in Spanish", func(t *testing.T) {
        got := Hello("Elodie", "Spanish")
        want := "Hola, Elodie"
        assertCorrectMessage(t, got, want)
    })
```

<!-- Remember not to cheat! _Test first_. When you try and run the test, the compiler _should_ complain because you are calling `Hello` with two arguments rather than one. -->
항상 _테스트를 먼저_ 작성하세요. 테스트를 실행하면 `Hello`를 하나가 아니라 두개의 인수로 호출하고 있기 때문에 컴파일러에서 에러가 발생할 것입니다.

```text
./hello_test.go:27:19: too many arguments in call to Hello
    have (string, string)
    want (string)
```

<!-- Fix the compilation problems by adding another string argument to `Hello` -->
`Hello`에 스트링 파라미터를 추가함으로서 컴파일 에러를 해결합니다.

```go
func Hello(name string, language string) string {
    if name == "" {
        name = "World"
    }
    return englishHelloPrefix + name
}
```

<!-- When you try and run the test again it will complain about not passing through enough arguments to `Hello` in your other tests and in `hello.go` -->
다시 테스트를 실행하면 `hello.go` 에 있는 다른 코드에서 `Hello`에 인수가 부족하다는 에러가 뜰 겁니다.

```text
./hello.go:15:19: not enough arguments in call to Hello
    have (string)
    want (string, string)
```

<!-- Fix them by passing through empty strings. Now all your tests should compile _and_ pass, apart from our new scenario -->
비어있는 스트링을 인수로 넘겨서 고칩니다. 이제 우리의 코드는 새로운 요구 사항을 제외하면 컴파일 되고 테스트도 통과할 겁니다.

```text
hello_test.go:29: got 'Hello, Elodie' want 'Hola, Elodie'
```

<!-- We can use `if` here to check the language is equal to "Spanish" and if so change the message -->
`if`를 사용해서 언어가 "Spanish"로 설정되어있는지 확인하고 메시지를 고칠 수 있습니다.

```go
func Hello(name string, language string) string {
    if name == "" {
        name = "World"
    }

    if language == "Spanish" {
        return "Hola, " + name
    }

    return englishHelloPrefix + name
}
```

<!-- The tests should now pass. -->
이제 테스트가 통과할 겁니다.

<!-- Now it is time to _refactor_. You should see some problems in the code, "magic" strings, some of which are repeated. Try and refactor it yourself, with every change make sure you re-run the tests to make sure your refactoring isn't breaking anything. -->
이제 _리팩토링_ 을 해야합니다. 여기에서 문제는 "마법" 스트링들이 있어서 반복되고 있다는 점입니다. 한번 스스로 해보세요. 무언가를 추가하거나 변경할 때마다 테스트를 다시 실행하고 리팩토링이 무언가를 부수지는 않았는지 확인합니다. 

```go
const spanish = "Spanish"
const englishHelloPrefix = "Hello, "
const spanishHelloPrefix = "Hola, "

func Hello(name string, language string) string {
    if name == "" {
        name = "World"
    }

    if language == spanish {
        return spanishHelloPrefix + name
    }

    return englishHelloPrefix + name
}
```

<!-- ### French -->
### 프랑스어

<!-- * Write a test asserting that if you pass in `"French"` you get `"Bonjour, "` -->
* `"French"`를 건네면 `"Bonjour, "`를 리턴하는지 확인하는 테스트 작성하기
<!-- * See it fail, check the error message is easy to read -->
* 실패하는지 확인하고, 에러 메시지를 확인하기
<!-- * Do the smallest reasonable change in the code -->
* 최소한의 변경으로 테스트를 통과시키기

<!-- You may have written something that looks roughly like this -->
여러분은 아래와 같이 코드를 썼을 겁니다.

```go
func Hello(name string, language string) string {
    if name == "" {
        name = "World"
    }

    if language == spanish {
        return spanishHelloPrefix + name
    }

    if language == french {
        return frenchHelloPrefix + name
    }

    return englishHelloPrefix + name
}
```

<!-- ## `switch` -->
## `switch`

<!-- When you have lots of `if` statements checking a particular value it is common to use a `switch` statement instead. We can use `switch` to refactor the code to make it easier to read and more extensible if we wish to add more language support later -->
특정한 값에 대해서 확인해야 하는 조건이 많아서 `if` 문이 늘어났을때는 `switch` 를 사용하는 것이 일반적입니다. `switch` 를 사용해서 코드의 가독성을 높이고 나중에 언어가 추가 됐을때 쉽게 편집할 수 있도록 리팩토링 합니다.

```go
func Hello(name string, language string) string {
    if name == "" {
        name = "World"
    }

    prefix := englishHelloPrefix

    switch language {
    case french:
        prefix = frenchHelloPrefix
    case spanish:
        prefix = spanishHelloPrefix
    }

    return prefix + name
}
```

<!-- Write a test to now include a greeting in the language of your choice and you should see how simple it is to extend our _amazing_ function. -->
임의의 언어에 대해서 테스트 케이스를 추가해보면 얼마나 새로운 언어를 추가하는게 쉬운 일이 되었는지 확인 할 수 있을 겁니다.

<!-- ### one...last...refactor? -->
### 마지막 리팩토링

<!-- You could argue that maybe our function is getting a little big. The simplest refactor for this would be to extract out some functionality into another function. -->
함수가 너무 커져 버린 것 같은 인상을 받았을 수 있습니다. 이를 리팩토링하는 가장 간단한 방법은 기능 중 일부를 다른 함수로 빼는 것 입니다.

```go
func Hello(name string, language string) string {
    if name == "" {
        name = "World"
    }

    return greetingPrefix(language) + name
}

func greetingPrefix(language string) (prefix string) {
    switch language {
    case french:
        prefix = frenchHelloPrefix
    case spanish:
        prefix = spanishHelloPrefix
    default:
        prefix = englishHelloPrefix
    }
    return
}
```

<!-- A few new concepts: -->
새로운 개념들:

<!-- * In our function signature we have made a _named return value_ `(prefix string)`. -->
* 함수의 시그네쳐에 `(prefix string)` 처럼 _네임드 리턴 값_ 을 만들 었습니다.
<!-- * This will create a variable called `prefix` in your function. -->
* 이는 함수안에 `prefix`라는 변수를 추가합니다.
  <!-- * It will be assigned the "zero" value. This depends on the type, for example `int`s are 0 and for strings it is `""`. -->
  * "zero" 값을 기본으로 가집니다. 이는 타입에 따라서 달라 집니다. 예를 들어 `int` 타입의 경우에는 0을, 스트링의 경우에는 `""`를 기본값으로 가집니다.
    <!-- * You can return whatever it's set to by just calling `return` rather than `return prefix`. -->
    * `return`만 호출해도 리턴할 수 있습니다. `return prefix`라고 쓰지 않아도 됩니다.
  <!-- * This will display in the Go Doc for your function so it can make the intent of your code clearer. -->
  * 이 시그니쳐는 Go Doc에 표시돼서 여러분이 작성한 함수의 의도를 명확하게 해줍니다.
<!-- * `default` in the switch case will be branched to if none of the other `case` statements match. -->
* switch 안의 `default`는 `case`의 조건 중 어느 것에도 매칭하지 않은 경우에 실행됩니다.
<!-- * The function name starts with a lowercase letter. In Go public functions start with a capital letter and private ones start with a lowercase. We don't want the internals of our algorithm to be exposed to the world, so we made this function private. -->
* 함수이름은 소문자로 시잡합니다. Go에서 퍼블릭 함수는 대문자로 시작하고 프라이빗 함수는 소문자로 시작합니다. 우리는 이 함수가 밖에서 보이는 것을 원하지 않기 때문에 프라이빗으로 선언합니다.

<!-- ## Wrapping up -->
## 정리

<!-- Who knew you could get so much out of `Hello, world`? -->
`Hello, world`에서 이만큼이나 배웠습니다.

<!-- By now you should have some understanding of: -->
이 시점에서 여러분은 아래의 것들을 이해하고 있습니다:

### Some of Go's syntax around

* Writing tests
* Declaring functions, with arguments and return types
* `if`, `const` and `switch`
* Declaring variables and constants

### The TDD process and _why_ the steps are important

* _Write a failing test and see it fail_ so we know we have written a _relevant_ test for our requirements and seen that it produces an _easy to understand description of the failure_
* Writing the smallest amount of code to make it pass so we know we have working software
* _Then_ refactor, backed with the safety of our tests to ensure we have well-crafted code that is easy to work with

In our case we've gone from `Hello()` to `Hello("name")`, to `Hello("name", "French")` in small, easy to understand steps.

This is of course trivial compared to "real world" software but the principles still stand. TDD is a skill that needs practice to develop but by being able to break problems down into smaller components that you can test you will have a much easier time writing software.
