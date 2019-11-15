<!-- # Structs, methods & interfaces -->
# 구조체, 메소드 & 인터페이스

<!-- **[You can find all the code for this chapter here](https://github.com/quii/learn-go-with-tests/tree/master/structs)** -->
**[이번 장의 모든 코드는 여기서 찾아볼 수 있습니다.](https://github.com/quii/learn-go-with-tests/tree/master/structs)**

<!-- Suppose that we need some geometry code to calculate the perimeter of a rectangle given a height and width. We can write a `Perimeter(width float64, height float64)` function, where `float64` is for floating-point numbers like `123.45`. -->
사각형의 너비와 높이가 주어졌을 때 둘레를 계산하는 기하학 코드가 필요하다고 가정해봅시다. 우리는 `Perimeter(width float64, height float64)` 함수를 작성할 수 있습니다. 여기서 `float64`는 `123.45`와 같은 부동소수점 숫자를 나타냅니다.

<!-- The TDD cycle should be pretty familiar to you by now. -->
이제는 TDD 주기가 더 익숙할 것입니다.

<!-- ## Write the test first -->
## 첫 번째 테스트 작성하기

```go
func TestPerimeter(t *testing.T) {
    got := Perimeter(10.0, 10.0)
    want := 40.0

    if got != want {
        t.Errorf("got %.2f want %.2f", got, want)
    }
}
```

<!-- Notice the new format string? The `f` is for our `float64` and the `.2` means print 2 decimal places. -->
새로운 포맷 스트링을 눈치챘습니까? `f`는 `float64`를 지원하고 `.2`는 소수를 두 자리까지만 출력합니다.

<!-- ## Try to run the test -->
## 테스트 작동 시도하기

`./shapes_test.go:6:9: undefined: Perimeter`

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 테스트를 위한 최소 코드를 작성하고 실패한 테스트 결과물을 확인하기

```go
func Perimeter(width float64, height float64) float64 {
    return 0
}
```

<!-- Results in `shapes_test.go:10: got 0 want 40`. -->
결과는 `shapes_test.go:10: got 0 want 40`입니다.

<!-- ## Write enough code to make it pass -->
## 테스트를 통과하는 코드 작성하기

```go
func Perimeter(width float64, height float64) float64 {
    return 2 * (width + height)
}
```

<!-- So far, so easy. Now let's create a function called `Area(width, height float64)` which returns the area of a rectangle. -->
지금까지 간단했습니다. 이제 직사각형의 넓이를 반환하는 `Area(width, height float64` 함수를 만들어봅시다.

<!-- Try to do it yourself, following the TDD cycle. -->
TDD 절차를 따라 스스로 작성해보기 바랍니다.

<!-- You should end up with tests like this -->
테스트는 이런 결과에 도달해야 합니다.

```go
func TestPerimeter(t *testing.T) {
    got := Perimeter(10.0, 10.0)
    want := 40.0

    if got != want {
        t.Errorf("got %.2f want %.2f", got, want)
    }
}

func TestArea(t *testing.T) {
    got := Area(12.0, 6.0)
    want := 72.0

    if got != want {
        t.Errorf("got %.2f want %.2f", got, want)
    }
}
```

<!-- And code like this -->
코드는 이런 모습이어야 합니다.

```go
func Perimeter(width float64, height float64) float64 {
    return 2 * (width + height)
}

func Area(width float64, height float64) float64 {
    return width * height
}
```

<!-- ## Refactor -->
## 리팩토링

<!-- Our code does the job, but it doesn't contain anything explicit about rectangles. An unwary developer might try to supply the width and height of a triangle to these functions without realising they will return the wrong answer. -->
우리 코드는 작동하지만, 직사각형에 대한 어떤 정보도 담고 있지 않습니다. 부주의한 개발자는 아마 틀린 값을 반환하는지도 모른채 삼각형에 대한 너비와 높이를 입력할 것입니다.

<!-- We could just give the functions more specific names like `RectangleArea`. A neater solution is to define our own _type_ called `Rectangle` which encapsulates this concept for us. -->
우리는 `RectangleArea`처럼 함수를 더 명확하게 제공할 필요가 있습니다. 좀더 정돈된 해결책은 이런 개념을 캡슐화하는 새로운 _자료형_ `Rectangle`을 정의하는 겁니다.

<!-- We can create a simple type using a **struct**. [A struct](https://golang.org/ref/spec#Struct_types) is just a named collection of fields where you can store data. -->
**struct**를 사용하여 간단한 타입을 만들 수 있습니다. [구조체](https://golang.org/ref/spec#Struct_types)는 정보를 저장할 수 있는 명명된 필드 모음입니다.

<!-- Declare a struct like this -->
아래와 같은 구조체를 정의하세요.

```go
type Rectangle struct {
    Width float64
    Height float64
}
```

<!-- Now lets refactor the tests to use `Rectangle` instead of plain `float64`s. -->
이제 기본 `float64` 대신에 `Rectangle`를 사용하여 테스트를 리팩토링 해봅시다.

```go
func TestPerimeter(t *testing.T) {
    rectangle := Rectangle{10.0, 10.0}
    got := Perimeter(rectangle)
    want := 40.0

    if got != want {
        t.Errorf("got %.2f want %.2f", got, want)
    }
}

func TestArea(t *testing.T) {
    rectangle := Rectangle{12.0, 6.0}
    got := Area(rectangle)
    want := 72.0

    if got != want {
        t.Errorf("got %.2f want %.2f", got, want)
    }
}
```

<!-- Remember to run your tests before attempting to fix, you should get a helpful error like -->
고치기 전에 테스트를 실행하는 걸 잊지마세요. 아래와 같은 유용한 오류를 얻을 수 있습니다.

```text
./shapes_test.go:7:18: not enough arguments in call to Perimeter
    have (Rectangle)
    want (float64, float64)
```

<!-- You can access the fields of a struct with the syntax of `myStruct.field`. -->
`myStruct.field`와 같은 방식으로 각각의 필드에 접근할 수 있습니다.

<!-- Change the two functions to fix the test. -->
테스트를 고치기 위해 두 함수를 바꾸세요.

```go
func Perimeter(rectangle Rectangle) float64 {
    return 2 * (rectangle.Width + rectangle.Height)
}

func Area(rectangle Rectangle) float64 {
    return rectangle.Width * rectangle.Height
}
```

<!-- I hope you'll agree that passing a `Rectangle` to a function conveys our intent more clearly but there are more benefits of using structs that we will get on to. -->
`Rectangle`을 보내는 게 우리 의도를 더 선명하게 만든다는 데에 동감하길 바랍니다. 더구나 구조체를 쓰는 데에는 더 큰 이점이 있습니다.

<!-- Our next requirement is to write an `Area` function for circles. -->
우리의 다음 요구사항은 원에 대한 `Area` 함수를 작성하는 것입니다.

<!-- ## Write the test first -->
## 테스트 먼저 작성하기

```go
func TestArea(t *testing.T) {

    t.Run("rectangles", func(t *testing.T) {
        rectangle := Rectangle{12, 6}
        got := Area(rectangle)
        want := 72.0

        if got != want {
            t.Errorf("got %g want %g", got, want)
        }
    })

    t.Run("circles", func(t *testing.T) {
        circle := Circle{10}
        got := Area(circle)
        want := 314.1592653589793

        if got != want {
            t.Errorf("got %g want %g", got, want)
        }
    })

}
```

<!-- As you can see, the 'f' has been replaced by 'g', using 'f' it could be difficult to know the exact decimal number, with 'g' we get a complete decimal number in the error message \([fmt options](https://golang.org/pkg/fmt/)\). -->
보시는 것처럼, 'f'는 'g'로 바뀌었습니다. 'f'를 사용하면 정확한 십진수를 알기 어려울 수 있지만, 'g'를 사용하면, 오류 메시지에서 정확한 십진수를 볼 수 있습니다. \([fmt options](https://golang.org/pkg/fmt/)\)

<!-- ## Try to run the test -->
## 테스트 구동 시도하기

`./shapes_test.go:28:13: undefined: Circle`

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 테스트를 구동하는 최소한의 코드 작성하고 실패한 출력 확인하기

<!-- We need to define our `Circle` type. -->
`Circle` 타입을 정의하는 게 필요합니다.

```go
type Circle struct {
    Radius float64
}
```

<!-- Now try to run the tests again -->
이제 다시 테스트를 구동해보십시오.

`./shapes_test.go:29:14: cannot use circle (type Circle) as type Rectangle in argument to Area`

<!-- Some programming languages allow you to do something like this: -->
몇몇 프로그래밍 언어는 이런 걸 지원합니다.

```go
func Area(circle Circle) float64 { ... }
func Area(rectangle Rectangle) float64 { ... }
```

<!-- But you cannot in Go -->
Go는 그렇지 않습니다.

`./shapes.go:20:32: Area redeclared in this block`

<!-- We have two choices: -->
두 가지 방법이 있습니다:

<!-- * You can have functions with the same name declared in different _packages_. So we could create our `Area(Circle)` in a new package, but that feels overkill here. -->
다른 _packages_에서 같은 이름을 가진 함수를 만들 수 있습니다. `Area(Circle)`을 새로운 패키지에 만들면 되지만, 이 경우엔 지나친 것 같습니다.
<!-- * We can define [_methods_](https://golang.org/ref/spec#Method_declarations) on our newly defined types instead. -->
대신에 새로 정의된 타입에 대해 [_methods_](https://golang.org/ref/spec#Method_declarations)를 정의할 수 있습니다.

<!-- ### What are methods? -->
### 메소드가 무엇인가요?

<!-- So far we have only been writing _functions_ but we have been using some methods. When we call `t.Errorf` we are calling the method `ErrorF` on the instance of our `t` \(`testing.T`\). -->
우리가 _함수들_만 다루어왔지만, 몇몇 메소드도 사용했습니다. `t.Errorf`를 호출할 때, 우리는 인스턴스 `t` \(`testing.T`\)의 메소드 `Errorf`를 사용하는 것입니다.

<!-- A method is a function with a receiver. A method declaration binds an identifier, the method name, to a method, and associates the method with the receiver's base type. -->
메소드는 리시버를 가진 함수입니다. 메소드 선언은 식별자와 메소드 이름을 메소드에 엮고, 메소드를 리시버의 기본 타입에 연결합니다.

<!-- Methods are very similar to functions but they are called by invoking them on an instance of a particular type. Where you can just call functions wherever you like, such as `Area(rectangle)` you can only call methods on "things". -->
메소드는 함수와 매우 유사하지만, 특정 타입의 인스턴스에서만 호출된다는 점에서 다릅니다. 예를 들어 함수는 어디서든 부를 수 있지만, `Area(rectangle)` 같은 메소드는 "특정 상황"에서만 부를 수 있습니다.

<!-- An example will help so let's change our tests first to call methods instead and then fix the code. -->
예제가 도움이 될테니 먼저 테스트가 메소드를 호출하도록 바꾼 뒤 코드를 고쳐봅시다.

```go
func TestArea(t *testing.T) {

    t.Run("rectangles", func(t *testing.T) {
        rectangle := Rectangle{12, 6}
        got := rectangle.Area()
        want := 72.0

        if got != want {
            t.Errorf("got %g want %g", got, want)
        }
    })

    t.Run("circles", func(t *testing.T) {
        circle := Circle{10}
        got := circle.Area()
        want := 314.1592653589793

        if got != want {
            t.Errorf("got %g want %g", got, want)
        }
    })

}
```

<!-- If we try to run the tests, we get -->
테스트를 돌리려하면, 아래와 같이 표시됩니다.

```text
./shapes_test.go:19:19: rectangle.Area undefined (type Rectangle has no field or method Area)
./shapes_test.go:29:16: circle.Area undefined (type Circle has no field or method Area)
```

> type Circle has no field or method Area

<!-- I would like to reiterate how great the compiler is here. It is so important to take the time to slowly read the error messages you get, it will help you in the long run. -->
컴파일러가 얼마나 훌륭한지 다시 얘기하고 싶습니다. 에러 메시지를 찬찬히 읽어보는 건 장기적으로 도움이 되기 때문에 아주 중요합니다.

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 최소한의 코드로 작동하는 테스트 작성하고 실패한 결과 확인하기

<!-- Let's add some methods to our types -->
우리 타입에 메소드를 추가해봅시다.

```go
type Rectangle struct {
    Width  float64
    Height float64
}

func (r Rectangle) Area() float64  {
    return 0
}

type Circle struct {
    Radius float64
}

func (c Circle) Area() float64  {
    return 0
}
```

<!-- The syntax for declaring methods is almost the same as functions and that's because they're so similar. The only difference is the syntax of the method receiver `func (receiverName RecieverType) MethodName(args)`. -->
메소드를 선언하는 문법은 함수와 거의 동일하기 때문에, 둘은 매우 유사합니다. 유일한 차이점은 메소드 리시버의 문법입니다. `func (receiverName ReceiverType) MethodName(args)`

<!-- When your method is called on a variable of that type, you get your reference to its data via the `receiverName` variable. In many other programming languages this is done implicitly and you access the receiver via `this`. -->
저 타입의 변수에서 메소드가 호출될 때, `receiverName` 변수를 통해 레퍼런스를 데이터로 받게 됩니다. 많은 다른 프로그래밍 언어에서 이를 함축적으로 표현하고, `this`를 통해 리시버에 접근합니다.

<!-- It is a convention in Go to have the receiver variable be the first letter of the type. -->
리시버 변수는 타입의 첫 글자로 하는 게 Go의 컨벤션입니다.

```go
r Rectangle
```

<!-- If you try to re-run the tests they should now compile and give you some failing output. -->
테스트를 다시 동작하면 이제 실패한 결과를 보여줄 것입니다.

<!-- ## Write enough code to make it pass -->
## 테스트를 통과할 코드 작성하기

<!-- Now let's make our rectangle tests pass by fixing our new method -->
이제 직사각형이 테스트를 통과하도록 메소드를 고쳐봅시다.

```go
func (r Rectangle) Area() float64  {
    return r.Width * r.Height
}
```

<!-- If you re-run the tests the rectangle tests should be passing but circle should still be failing. -->
테스트를 다시 동작하면 직사각형은 테스트를 통과하지만 원은 계속해서 실패할 것입니다.

<!-- To make circle's `Area` function pass we will borrow the `Pi` constant from the `math` package \(remember to import it\). -->
원의 `Area` 함수도 통과하게 만드려면, `math` 패키지의 `Pi` 상수를 사용하여야 합니다. \(패키지를 임포트하는 걸 잊지마십시오.\)

```go
func (c Circle) Area() float64  {
    return math.Pi * c.Radius * c.Radius
}
```

<!-- ## Refactor -->
## 리팩토링

<!-- There is some duplication in our tests. -->
테스트에 중복이 있습니다.

<!-- All we want to do is take a collection of _shapes_, call the `Area()` method on them and then check the result. -->
우리는 _도형들_에 대해 `Area()` 메소드를 동작하고 결과를 확인하길 원합니다.

<!-- We want to be able to write some kind of `checkArea` function that we can pass both `Rectangle`s and `Circle`s to, but fail to compile if we try to pass in something that isn't a shape. -->
우리는 `Rectangle`과 `Circle` 모두 통과하지만, 도형이 아닌 것에 대해선 실패하는, `checkArea` 같은 함수를 작성하길 원합니다.

<!-- With Go, we can codify this intent with **interfaces**. -->
Go에서는 **interfaces**를 사용하여 이런 코드를 만들 수 있습니다.

<!-- [Interfaces](https://golang.org/ref/spec#Interface_types) are a very powerful concept in statically typed languages like Go because they allow you to make functions that can be used with different types and create highly-decoupled code whilst still maintaining type-safety. -->
[Interfaces](https://golang.org/ref/spec#Interface_types)는 Go와 같은 정적 타입 언어에서 매우 강력한 개념입니다. 인터페이스를 사용하면 함수를 다양한 타입에 사용할 수 있고, 타입 안정성을 유지하면서도 독립적인 코드를 작성할 수 있습니다.

<!-- Let's introduce this by refactoring our tests. -->
이 개념을 사용하여 테스트를 리팩토링 해봅시다.

```go
func TestArea(t *testing.T) {

    checkArea := func(t *testing.T, shape Shape, want float64) {
        t.Helper()
        got := shape.Area()
        if got != want {
            t.Errorf("got %g want %g", got, want)
        }
    }

    t.Run("rectangles", func(t *testing.T) {
        rectangle := Rectangle{12, 6}
        checkArea(t, rectangle, 72.0)
    })

    t.Run("circles", func(t *testing.T) {
        circle := Circle{10}
        checkArea(t, circle, 314.1592653589793)
    })

}
```

<!-- We are creating a helper function like we have in other exercises but this time we are asking for a `Shape` to be passed in. If we try to call this with something that isn't a shape, then it will not compile. -->
우리가 다른 예제에서 만들었던 것처럼 보조 함수를 만들었습니다. 다른 점이 있다면, 이번엔 `Shape`가 전달되어야 합니다. 우리가 도형이 아닌 무언가를 호출하려 한다면, 테스트는 컴파일되지 않을 것입니다.

<!-- How does something become a shape? We just tell Go what a `Shape` is using an interface declaration -->
어떻게 도형을 만들 수 있을까요? 인터페이스 선언을 통해 `Shape`가 무엇인지 Go에 알려줘야 합니다.

```go
type Shape interface {
    Area() float64
}
```

<!-- We're creating a new `type` just like we did with `Rectangle` and `Circle` but this time it is an `interface` rather than a `struct`. -->
우리는 `Rectangle`과 `Circle`에서 했던 것처럼 새로운 `type`을 만들었습니다. 대신 이번엔 `struct` 말고 `interface`입니다.

<!-- Once you add this to the code, the tests will pass. -->
이 코드를 추가하면 테스트는 통과할 것입니다.

<!-- ### Wait, what? -->
### 잠깐, 뭐라구요?

<!-- This is quite different to interfaces in most other programming languages. Normally you have to write code to say `My type Foo implements interface Bar`. -->
이는 다른 대다수 프로그래밍 언어의 인터페이스와 사뭇 다릅니다. 보통은 `My type Foo implements interface Bar`와 같은 코드를 작성하여야 합니다.

<!-- But in our case -->
그러나 우리의 경우엔

<!-- * `Rectangle` has a method called `Area` that returns a `float64` so it satisfies the `Shape` interface -->
* `Rectangle`은 `float64`를 반환하는 `Area` 메소드를 가지며 `Shape` 인터페이스 조건을 만족합니다.
<!-- * `Circle` has a method called `Area` that returns a `float64` so it satisfies the `Shape` interface -->
* `Circle`은 `float64`를 반환하는 `Area` 메소드를 가지며 `Shape` 인터페이스 조건을 만족합니다.
<!-- * `string` does not have such a method, so it doesn't satisfy the interface -->
* `string`은 이러한 메소드를 가지지 않으며, 인터페이스 조건을 만족하지 않습니다.
<!-- * etc. -->
* 등등등.

<!-- In Go **interface resolution is implicit**. If the type you pass in matches what the interface is asking for, it will compile. -->
Go에서 **인터페이스 해결책은 함축적이다**. 만약 전달된 타입이 인터페이스 요구사항과 맞다면, 컴파일 될 것입니다.

<!-- ### Decoupling -->
### 분리하기

<!-- Notice how our helper does not need to concern itself with whether the shape is a `Rectangle` or a `Circle` or a `Triangle`. By declaring an interface the helper is _decoupled_ from the concrete types and just has the method it needs to do its job. -->
우리 보조 함수가 도형이 `Rectangle`이던 `Circle`이던 `Triangle`이던 고려할 필요 없다는 점에 주의하세요. 인터페이스를 선언하는 것만으로 보조 함수는 구체적인 타입으로부터 _분리되고_ 작업을 수행하는데 필요한 메소드만 가집니다.

<!-- This kind of approach of using interfaces to declare **only what you need** is very important in software design and will be covered in more detail in later sections. -->
 **딱 필요한 것만** 선언한 인터페이스를 사용하는, 이러한 접근법은 소프트웨어 디자인에서 매우 중요하여 나중에 더 자세히 다루게 될 것입니다.

<!-- ## Further refactoring -->
## 더나은 리팩토링

<!-- Now that you have some understanding of structs we can introduce "table driven tests". -->
이제 구조체에 대해 이해했으니 "table driven tests"를 소개할 수 있습니다.

<!-- [Table driven tests](https://github.com/golang/go/wiki/TableDrivenTests) are useful when you want to build a list of test cases that can be tested in the same manner. -->
[Table driven tests](https://github.com/golang/go/wiki/TableDrivenTests)는 같은 방식으로 테스트되는 테스트 케이스 목록을 작성할 때 유용합니다.

```go
func TestArea(t *testing.T) {

    areaTests := []struct {
        shape Shape
        want  float64
    }{
        {Rectangle{12, 6}, 72.0},
        {Circle{10}, 314.1592653589793},
    }

    for _, tt := range areaTests {
        got := tt.shape.Area()
        if got != tt.want {
            t.Errorf("got %g want %g", got, tt.want)
        }
    }

}
```

<!-- The only new syntax here is creating an "anonymous struct", areaTests. We are declaring a slice of structs by using `[]struct` with two fields, the `shape` and the `want`. Then we fill the slice with cases. -->
새로 추가된 문법은 "이름없는 구조체", areaTests를 만드는 것입니다. `[]struct`를 사용하여 두 개의 필드, `shape`와 `want`를 가지는 구조체 배열을 선언합니다. 그리고 이 배열을 케이스로 채웁니다.

<!-- We then iterate over them just like we do any other slice, using the struct fields to run our tests. -->
우리는 다른 배열에서 그랬던 것처럼, 구조체 배열을 반복하여 테스트를 진행할 것입니다.

<!-- You can see how it would be very easy for a developer to introduce a new shape, implement `Area` and then add it to the test cases. In addition, if a bug is found with `Area` it is very easy to add a new test case to exercise it before fixing it. -->
개발자가 새로운 도형과 `Area` 함수, 테스트 케이스를 더하는 게 얼마나 쉬운지 느껴질 겁니다. 게다가, `Area`에서 버그가 발견되면, 새로운 테스트 케이스를 추가하기도 매우 쉽습니다.

<!-- Table based tests can be a great item in your toolbox but be sure that you have a need for the extra noise in the tests. If you wish to test various implementations of an interface, or if the data being passed in to a function has lots of different requirements that need testing then they are a great fit. -->
테이블 기반 테스트는 좋은 도구이지만 추가 노이즈가 필요한지 확인하여야 합니다. 한 인터페이스의 다양한 구현체를 테스트하길 원한다면, 아니면 함수로 넘겨진 데이터가 테스트가 필요한 많은 요구사항을 가진다면 인터페이스가 가장 알맞습니다.

<!-- Let's demonstrate all this by adding another shape and testing it; a triangle. -->
이제 삼각형을 추가하고 테스트하여 이를 증명해봅시다.

<!-- ## Write the test first -->
## 테스트 먼저 작성

<!-- Adding a new test for our new shape is very easy. Just add `{Triangle{12, 6}, 36.0},` to our list. -->
새로운 도형을 위해 새로운 테스트를 추가하는 건 매우 간단합니다. `{Triangle{12, 6}, 36.0}`을 목록에 추가하십시오.

```go
func TestArea(t *testing.T) {

    areaTests := []struct {
        shape Shape
        want  float64
    }{
        {Rectangle{12, 6}, 72.0},
        {Circle{10}, 314.1592653589793},
        {Triangle{12, 6}, 36.0},
    }

    for _, tt := range areaTests {
        got := tt.shape.Area()
        if got != tt.want {
            t.Errorf("got %g want %g", got, tt.want)
        }
    }

}
```

<!-- ## Try to run the test -->
## 테스트 실행

<!-- Remember, keep trying to run the test and let the compiler guide you toward a solution. -->
계속 테스트를 실행시키고 컴파일러가 해결책을 제시하게 하세요.

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 테스트를 통과시키기 위한 최소한의 코드를 작성한뒤 출력을 확인

`./shapes_test.go:25:4: undefined: Triangle`

<!-- We have not defined Triangle yet -->
우리는 아직 삼각형을 정의하지 않았습니다.

```go
type Triangle struct {
    Base   float64
    Height float64
}
```

<!-- Try again -->
다시 시도하세요.

```text
./shapes_test.go:25:8: cannot use Triangle literal (type Triangle) as type Shape in field value:
    Triangle does not implement Shape (missing Area method)
```

<!-- It's telling us we cannot use a Triangle as a shape because it does not have an `Area()` method, so add an empty implementation to get the test working -->
삼각형에 `Area()` 메소드가 없기 때문에, 삼각형을 사용할 수 없다고 안내하고 있습니다. 그러니 빈 구현체를 추가하여 테스트가 동작하도록 합니다.

```go
func (t Triangle) Area() float64 {
    return 0
}
```

<!-- Finally the code compiles and we get our error -->
마침내 코드가 컴파일되고 에러를 얻습니다.

`shapes_test.go:31: got 0.00 want 36.00`

<!-- ## Write enough code to make it pass -->
## 테스트를 통과할 충분한 코드 작성

```go
func (t Triangle) Area() float64 {
    return (t.Base * t.Height) * 0.5
}
```

<!-- And our tests pass! -->
우리 테스트가 통과됐습니다!

<!-- ## Refactor -->
## 리팩토링

<!-- Again, the implementation is fine but our tests could do with some improvement. -->
다시 한 번, 구현체는 괜찮지만 우리 테스트는 더 발전할 수 있습니다.

<!-- When you scan this -->
이걸 본다면

```go
{Rectangle{12, 6}, 72.0},
{Circle{10}, 314.1592653589793},
{Triangle{12, 6}, 36.0},
```

<!-- It's not immediately clear what all the numbers represent and you should be aiming for your tests to be easily understood. -->
저 숫자들은 무엇을 나타내는지 명확하지 않습니다. 이해하기 쉬운 테스트를 만드는 걸 목표로 해야 합니다.

<!-- So far you've only been shown syntax for creating instances of structs `MyStruct{val1, val2}` but you can optionally name the fields. -->
여태까지 구조체 `MyStruct{val1, val2}`의 인스턴스를 만드는 문법을 보였다. 추가적으로 필드에도 이름을 붙일 수 있다.

<!-- Let's see what it looks like -->
어떻게 되는지 보자

```go
        {shape: Rectangle{Width: 12, Height: 6}, want: 72.0},
        {shape: Circle{Radius: 10}, want: 314.1592653589793},
        {shape: Triangle{Base: 12, Height: 6}, want: 36.0},
```

<!-- In [Test-Driven Development by Example](https://g.co/kgs/yCzDLF) Kent Beck refactors some tests to a point and asserts: -->
[Test-Driven Development by Example](https://g.co/kgs/yCzDLF)에서, Kent Beck은 몇몇 테스트를 고쳤고 이렇게 주장한다:

<!-- > The test speaks to us more clearly, as if it were an assertion of truth, **not a sequence of operations** -->
> 테스트가 더 분명하게 말한다, 진리의 주장인 것처럼, **일련의 명령어가 아니라**

<!-- \(emphasis mine\) -->
\(내꺼 강조\)

<!-- Now our tests \(at least the list of cases\) make assertions of truth about shapes and their areas. -->
이제 우리 테스트는 \(적어도 케이스 목록에 대해서는\) 형태와 넓이에 대한 사실을 주장한다.

<!-- ## Make sure your test output is helpful -->
## 테스트 출력이 도움이 되게 만들자

<!-- Remember earlier when we were implementing `Triangle` and we had the failing test? It printed `shapes_test.go:31: got 0.00 want 36.00`. -->
이전에 우리가 `Triangle`을 시험하고 실패했던 걸 기억하는가? `shapes_test.go:31: got 0.00 want 36.00`가 출력되었다.

<!-- We knew this was in relation to `Triangle` because we were just working with it, but what if a bug slipped in to the system in one of 20 cases in the table? How would a developer know which case failed? This is not a great experience for the developer, they will have to manually look through the cases to find out which case actually failed. -->
이것이 `Triangle`과 관련이 있음을 안다. 왜냐하면 우리가 방금 다루었기 때문에, 하지만 버그가 20개의 테이블 중 하나에서 발생한다면? 개발자는 어떤 테스트가 실패했는지 어떻게 알까? 실제 실패한 케이스를 찾기 위해 케이스를 일일이 살펴봐야하기 때문에 별로 좋은 경험이 아니다.

<!-- We can change our error message into `%#v got %.2f want %.2f`. The `%#v` format string will print out our struct with the values in its field, so the developer can see at a glance the properties that are being tested. -->
`%#v got %.2f want %.2f`와 같이 에러 메시지를 변경할 수 있다. `%#v` 포맷 스트링은 우리의 구조체와 필드의 값을 출력하고, 개발자는 테스트하는 속성을 한눈에 볼 수 있다.

<!-- To increase the readability of our test cases further we can rename the `want` field into something more descriptive like `hasArea`. -->
우리 테스트 케이스의 가독성을 높이기 위해 `want` 필드를 `hasArea`와 같이 더 명확한 명칭으로 바꿀 수 있다.

<!-- One final tip with table driven tests is to use `t.Run` and to name the test cases. -->
테이블 주도 테스트에 대한 마지막 팁은 `t.Run`을 사용하라는 것과 테스트 케이스에 이름을 짓는 것이다.

<!-- By wrapping each case in a `t.Run` you will have clearer test output on failures as it will print the name of the case -->
각각의 케이스를 `t.Run`으로 감싸면, 케이스가 실패했을 때 해당 케이스의 이름을 출력하기 때문에, 더 명확한 출력을 얻을 것이다.

```text
--- FAIL: TestArea (0.00s)
    --- FAIL: TestArea/Rectangle (0.00s)
        shapes_test.go:33: main.Rectangle{Width:12, Height:6} got 72.00 want 72.10
```

<!-- And you can run specific tests within your table with `go test -run TestArea/Rectangle`. -->
그리고 테이블 안 특정한 테스트는 `go test -run TestArea/Rectangle`과 같은 방식으로 작동할 수 있다.

<!-- Here is our final test code which captures this -->
이런 방식을 포함한 우리의 마지막 테스트 코드이다.

```go
func TestArea(t *testing.T) {

    areaTests := []struct {
        name    string
        shape   Shape
        hasArea float64
    }{
        {name: "Rectangle", shape: Rectangle{Width: 12, Height: 6}, hasArea: 72.0},
        {name: "Circle", shape: Circle{Radius: 10}, hasArea: 314.1592653589793},
        {name: "Triangle", shape: Triangle{Base: 12, Height: 6}, hasArea: 36.0},
    }

    for _, tt := range areaTests {
        // using tt.name from the case to use it as the `t.Run` test name
        t.Run(tt.name, func(t *testing.T) {
            got := tt.shape.Area()
            if got != tt.hasArea {
                t.Errorf("%#v got %g want %g", tt.shape, got, tt.hasArea)
            }
        })

    }

}
```

<!-- ## Wrapping up -->
## 정리

<!-- This was more TDD practice, iterating over our solutions to basic mathematic problems and learning new language features motivated by our tests. -->
이번 장을 통해 TDD에 대해 더많이 연습할 수 있었다. 기본적인 수학 문제에 대해 우리 솔루션을 반복했고, 테스트를 통해 새로운 언어의 특징을 배웠다.

<!-- * Declaring structs to create your own data types which lets you bundle related data together and make the intent of your code clearer -->
* 서로 연관된 데이터를 다루고, 코드의 의도를 명확하게 만들기 위한 사용자 정의 타입인 구조체 선언
<!-- * Declaring interfaces so you can define functions that can be used by different types \([parametric polymorphism](https://en.wikipedia.org/wiki/Parametric_polymorphism)\) -->
* 서로 다른 타입에서 사용될 수 있는 함수를 정의할 수 있도록 인터페이스 선언 \([parametric polymorphism](https://en.wikipedia.org/wiki/Parametric_polymorphism)\)
<!-- * Adding methods so you can add functionality to your data types and so you can implement interfaces -->
* 사용자 정의 타입에 기능을 추가하고 인터페이스를 시행할 수 있도록 메소드 추가
<!-- * Table based tests to make your assertions clearer and your suites easier to extend & maintain -->
* assertion을 더 명확하게 만들고 케이스를 더 쉽게 확장, 유지할 수 있게 하는 테이블 주도 테스트

<!-- This was an important chapter because we are now starting to define our own types. In statically typed languages like Go, being able to design your own types is essential for building software that is easy to understand, to piece together and to test. -->
우리는 이제부터 우리만의 타입을 정의할 예정이므로, 이 챕터는 중요한 챕터이다. Go 언어와 같이 정적 타입 언어에서, 자신만의 타입을 정의하는 능력은 이해하기 쉬운 소프트웨어를 만드는데 필수적이다.

<!-- Interfaces are a great tool for hiding complexity away from other parts of the system. In our case our test helper _code_ did not need to know the exact shape it was asserting on, only how to "ask" for it's area. -->
인터페이스는 시스템의 다른 부분으로부터 복잡성을 숨기는 훌륭한 도구이다.우리의 경우, 우리 테스트 도우미 _code_는 정확한 모양을 알 필요 없고 오직 어떻게 넓이를 "요청"할 지만 안다.

<!-- As you become more familiar with Go you start to see the real strength of interfaces and the standard library. You'll learn about interfaces defined in the standard library that are used _everywhere_ and by implementing them against your own types you can very quickly re-use a lot of great functionality. -->
Go에 더 익숙해질수록 인터페이스와 표준 라이브러리의 진면모를 보게 될 것이다. 앞으로 _어디에나_ 쓰이는 표준 라이브러리에 정의된 인터페이스에 대해 배울 것이고 이를 사용자 정의 타입에 확장하여 매우 빠르게 많은 기능을 재사용할 것이다.
