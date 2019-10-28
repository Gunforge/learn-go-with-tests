<!-- # Arrays and slices -->
# 배열과 슬라이스

<!-- **[You can find all the code for this chapter here](https://github.com/quii/learn-go-with-tests/tree/master/arrays)** -->
**[이 챕터에서 쓰인 코드는 여기에서 확인 할 수 있습니다.](https://github.com/quii/learn-go-with-tests/tree/master/arrays)**

<!-- Arrays allow you to store multiple elements of the same type in a variable in
a particular order. -->
배열은 같은 타입의 변수들에 순서를 매겨 저장 할 수 있게 해줍니다.

<!-- When you have an array, it is very common to have to iterate over them. So let's
use [our new-found knowledge of `for`](iteration.md) to make a `Sum` function. `Sum` will
take an array of numbers and return the total. -->
일반적으로 배열의 요소는 iterate를 통해서 접근합니다. `Sum` 함수를 만들기 위해 [전에 배웠던 `for`](itermation.md) 를 사용합시다. `Sum`은 숫자가 들어있는 배열을 입력으로 받아 총합을 반환합니다.

<!-- Let's use our TDD skills -->
TDD스킬을 사용합시다.

<!-- ## Write the test first -->
## 테스트 먼저 작성

<!-- In `sum_test.go` -->
`sum_test.go`에 아래의 코드를 작성합니다.

```go
package main

import "testing"

func TestSum(t *testing.T) {

    numbers := [5]int{1, 2, 3, 4, 5}

    got := Sum(numbers)
    want := 15

    if got != want {
        t.Errorf("got %d want %d given, %v", got, want, numbers)
    }
}
```

<!-- Arrays have a _fixed capacity_ which you define when you declare the variable. -->
배열은 선언 할 떄 지정된 _고정된 크기_ 를 가집니다.
<!-- We can initialize an array in two ways: -->
배열은 두가지 방식으로 선언 가능합니다:

<!-- * \[N\]type{value1, value2, ..., valueN} e.g. `numbers := [5]int{1, 2, 3, 4, 5}` -->
* \[N\]type{value1, value2, ..., valueN} e.g. `numbers := [5]int{1, 2, 3, 4, 5}`
<!-- * \[...\]type{value1, value2, ..., valueN} e.g. `numbers := [...]int{1, 2, 3, 4, 5}` -->
* \[...\]type{value1, value2, ..., valueN} e.g. `numbers := [...]int{1, 2, 3, 4, 5}`

<!-- It is sometimes useful to also print the inputs to the function in the error
message and we are using the `%v` placeholder which is the "default" format,
which works well for arrays. -->
에러가 발생했을때 함수가 어떤 파라미터로 호출되었는지 출력하는 것이 유용할 떄가 있습니다. 기본값으로 포멧을 지정하는 `%v`가 배열을 출력하는데 알맞습니다.

<!-- [Read more about the format strings](https://golang.org/pkg/fmt/) -->
[포멧 스트링에 대해서 확인](http://golang.org/pkg/fmt)

<!-- ## Try to run the test -->
## 테스트 실행해 보기

<!-- By running `go test` the compiler will fail with `./sum_test.go:10:15: undefined: Sum` -->
`go test`로 테스트를 실행하면 `./sum_test.go:10:15: undefined: Sum`이라는 컴파일 에러가 출력됩니다.

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 최소한의 코드를 작성하고 출력 확인하기

<!-- In `sum.go` -->
`sum.go`에 아래의 코드를 추가합니다.

```go
package main

func Sum(numbers [5]int) int {
    return 0
}
```

<!-- Your test should now fail with _a clear error message_ -->
다음의 _에러 메세지_ 와 함께 테스트가 실패 할 겁니다.

<!-- `sum_test.go:13: got 0 want 15 given, [1 2 3 4 5]` -->
`sum_test.go:13: got 0 want 15 given, [1 2 3 4 5]`

<!-- ## Write enough code to make it pass -->
## 테스트를 통과시키기 위한 코드 작성

```go
func Sum(numbers [5]int) int {
    sum := 0
    for i := 0; i < 5; i++ {
        sum += numbers[i]
    }
    return sum
}
```

<!-- To get the value out of an array at a particular index, just use `array[index]`
syntax. In this case, we are using `for` to iterate 5 times to work through the
array and add each item onto `sum`. -->
배열의 특정 위치에 있는 값을 인덱스로 얻기 위해서는 `array[index]`를 사용하면 됩니다. 위의 코드에서는 `for`를 사용해서 5번 반복하며 값을 `sum`에 더하고 있습니다. 

<!-- ## Refactor -->
## 리팩토링

<!-- Let's introduce [`range`](https://gobyexample.com/range) to help clean up our code -->
[`range`](https://gobyexample.com/range) 를 사용해서 코드를 정리 합시다.

```go
func Sum(numbers [5]int) int {
    sum := 0
    for _, number := range numbers {
        sum += number
    }
    return sum
}
```

<!-- `range` lets you iterate over an array. Every time it is called it returns two
values, the index and the value. We are choosing to ignore the index value by
using `_` [blank identifier](https://golang.org/doc/effective_go.html#blank). -->
`range`는 배열을 iterate 합니다. 호출 될때마다 값과, 그 값에 대한 인덱스 두 값을 반환 합니다. 여기서 우리는 `_`[blank identifier](https://golang.org/doc/effective_go.html#blank) 를 사용해서 인덱스를 무시하도록 하고 있습니다. 

<!-- ### Arrays and their type -->
### 배열과 타입

<!-- An interesting property of arrays is that the size is encoded in its type. If you try
to pass an `[4]int` into a function that expects `[5]int`, it won't compile.
They are different types so it's just the same as trying to pass a `string` into
a function that wants an `int`. -->
배열의 특이한 점은 크기가 타입안에 고정되어 있다는 점 입니다. `[5]int`를 받는 함수에 `[4]int`를 건네면 컴파일 에러가 발생합니다. 이 둘은 완전히 다름 타입이어서 `int`를 받는 함수에 `string`을 건네는 것과 같은 의미를 가집니다. 

<!-- You may be thinking it's quite cumbersome that arrays have a fixed length, and most
of the time you probably won't be using them! -->
배열이 고정된 길이를 가져야 한다는 사실은 불편하다고 느꼈을 수 있습니다. 하지만 다행히도 대부분의 경우에는 배열을 사용하지 않아도 됩니다.

<!-- Go has _slices_ which do not encode the size of the collection and instead can
have any size. -->
Go는 임의의 길이를 가질 수 있는 컬렉션 _slices_ 를 가지고 있습니다.

<!-- The next requirement will be to sum collections of varying sizes. -->
다음 요구사항은 임의의 크기의 컬렉션에 대한 덧셈을 구현하는 것입니다.

<!-- ## Write the test first -->
## 테스트 먼저 작성

<!-- We will now use the [slice type][slice] which allows us to have collections of
any size. The syntax is very similar to arrays, you just omit the size when
declaring them -->
이제 부터는 가변길이를 가지는 [슬라이스 타입][slice] 를 사용할 겁니다. 배열을 선언하는 부분에서 크기를 지정하는 부분을 빼면 됩니다.

<!-- `mySlice := []int{1,2,3}` rather than `myArray := [3]int{1,2,3}` -->
이를테면 `myArray := [3]int{1,2,3}` 대신 `mySlice := []int{1,2,3}`

```go
func TestSum(t *testing.T) {

    t.Run("collection of 5 numbers", func(t *testing.T) {
        numbers := [5]int{1, 2, 3, 4, 5}

        got := Sum(numbers)
        want := 15

        if got != want {
            t.Errorf("got %d want %d given, %v", got, want, numbers)
        }
    })

    t.Run("collection of any size", func(t *testing.T) {
        numbers := []int{1, 2, 3}

        got := Sum(numbers)
        want := 6

        if got != want {
            t.Errorf("got %d want %d given, %v", got, want, numbers)
        }
    })

}
```

<!-- ## Try and run the test -->
## 테스트 실행하기

<!-- This does not compile -->
컴파일이 실패 할 겁니다.

`./sum_test.go:22:13: cannot use numbers (type []int) as type [5]int in argument to Sum`

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 최소한의 코드를 작성하고 어떻게 실패하는지 확인하기

<!-- The problem here is we can either -->
여기서 문제는 다음과 같습니다

<!-- * Break the existing API by changing the argument to `Sum` to be a slice rather
  than an array. When we do this we will know we have potentially ruined
  someone's day because our _other_ test will not compile! -->
* `Sum`의 인수를 배열에서 슬라이스로 변경하는 것은 이미 존재하는 코드가 사용하는 API는 위반하기 때문에 다른 사람이 이 함수를 사용하고 있었을때 그 사람의 코드가 망가질 가능성이 있습니다.  
<!-- * Create a new function -->
* 새 함수 선언

<!-- In our case, no-one else is using our function so rather than having two functions to maintain let's just have one. -->
우리의 경우에는, 다른 사람이 우리 코드를 쓸 일이 없기 때문에 두개로 나누기 보다는 하나로 합칩시다.

```go
func Sum(numbers []int) int {
    sum := 0
    for _, number := range numbers {
        sum += number
    }
    return sum
}
```

<!-- If you try to run the tests they will still not compile, you will have to change the first test to pass in a slice rather than an array. -->
이렇게 하고 테스트를 실행해보면 컴파일에 실패할 것입니다. 여기서는 테스트를 수정해서 통과하게 만듭시다.

<!-- ## Write enough code to make it pass -->
## 테스트 통과시키기

<!-- It turns out that fixing the compiler problems were all we need to do here and the tests pass! -->
이 경우에는 컴파일 에러만 고치면 테스트가 통과합니다.

<!-- ## Refactor -->
## 리팩토링

<!-- We had already refactored `Sum` and all we've done is changing from arrays to slices, so there's not a lot to do here. Remember that we must not neglect our test code in the refactoring stage and we have some to do here. -->
이미 모든 배열을 슬라이스로 바꾸면서 `Sum`을 리팩토링 했습니다. 그렇기 때문에 조금만 더 하면 됩니다. 리팩토링 할때 리팩토링의 대상에 테스트 코드도 포함된다는 사실을 잊지 마세요. 

```go
func TestSum(t *testing.T) {

    t.Run("collection of 5 numbers", func(t *testing.T) {
        numbers := []int{1, 2, 3, 4, 5}

        got := Sum(numbers)
        want := 15

        if got != want {
            t.Errorf("got %d want %d given, %v", got, want, numbers)
        }
    })

    t.Run("collection of any size", func(t *testing.T) {
        numbers := []int{1, 2, 3}

        got := Sum(numbers)
        want := 6

        if got != want {
            t.Errorf("got %d want %d given, %v", got, want, numbers)
        }
    })

}
```

<!-- It is important to question the value of your tests. It should not be a goal to
have as many tests as possible, but rather to have as much _confidence_ as
possible in your code base. Having too many tests can turn in to a real problem
and it just adds more overhead in maintenance. **Every test has a cost**. -->
테스트가 얼마나 유의미한지 항상 체크해야 합니다. 중요한 것은 테스트 케이스의 수가 아니라 테스트를 실행함으로서 얻을 수 있는 _자신감_ 이기 때문입니다.
__모든 테스트는 비용이 있기 때문에__ 테스트를 너무 많이 작성하는 것은 유지 비용을 늘려 또 다른 문제를 낳습니다.

<!-- In our case, you can see that having two tests for this function is redundant.
If it works for a slice of one size it's very likely it'll work for a slice of
any size \(within reason\). -->
슬라이스를 사용하면, 한 사이즈에 대해서 작동하는 코드는
다른 가변길이의 슬라이스에 대해서도 동작 할 것이기 때문에
이 하나의 함수에 대해서 두 개의 테스트를 정의 하는 것은 불필요 합니다. 

<!-- Go's built-in testing toolkit features a [coverage
tool](https://blog.golang.org/cover), which can help identify areas of your code
you have not covered. I do want to stress that having 100% coverage should not
be your goal, it's just a tool to give you an idea of your coverage. If you have
been strict with TDD, it's quite likely you'll have close to 100% coverage
anyway. -->
Go는 테스트 프레임워크 안에 [커버리지 툴](https://blog.golang.org/cover) 을 포함하고 있습니다.
테스트 커버리지는 그저 도구일 뿐입니다. 테스트 커버리지를 100%로 만드는 것이 목표가 되어서는 안됩니다. 
하지만 TDD를 따른다면 100% 에 가까운 커버리지를 달성하게 될 겁니다.

<!-- Try running -->
아래 명령어를 실행합니다.

`go test -cover`

<!-- You should see -->
아래와 같이 출력 될 것입니다.

```bash
PASS
coverage: 100.0% of statements
```

<!-- Now delete one of the tests and check the coverage again. -->
테스트 중에 하나를 주석 처리하고 커버리지를 다시 확인해 봅니다.

<!-- Now that we are happy we have a well-tested function you should commit your
great work before taking on the next challenge. -->
잘 테스트된 함수가 있기 때문에 현시점에서 다음으로 진행하기 전에 커밋을 합시다.

<!-- We need a new function called `SumAll` which will take a varying number of
slices, returning a new slice containing the totals for each slice passed in. -->
이제 복수의 슬라이스를 받아서 각각의 총합을 슬라이스로 반환하는 `SumAll`함수를 정의 합시다.

<!-- For example -->
예를 들면

<!-- `SumAll([]int{1,2}, []int{0,9})` would return `[]int{3, 9}` -->
`SumAll([]int{1,2}, []int{0,9})` 는 `[]int{3, 9}` 를 리턴 할 겁니다.

<!-- or -->
또는

<!-- `SumAll([]int{1,1,1})` would return `[]int{3}` -->
`SumAll([]int{1,1,1})` 는 `[]int{3}` 를 리턴 할 겁니다.

<!-- ## Write the test first -->
## 테스트 먼저 작성

```go
func TestSumAll(t *testing.T) {

    got := SumAll([]int{1,2}, []int{0,9})
    want := []int{3, 9}

    if got != want {
        t.Errorf("got %v want %v", got, want)
    }
}
```

<!-- ## Try and run the test -->
## 테스트 실행

`./sum_test.go:23:9: undefined: SumAll`

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 최소한의 코드를 작성하고 테스트가 어떻게 실패하는지 확인

<!-- We need to define SumAll according to what our test wants. -->
SumAll에서 정의된 대로 코드를 작성해야 합니다.

<!-- Go can let you write [_variadic functions_](https://gobyexample.com/variadic-functions) that can take a variable number of arguments. -->
[_variadic functions_](https://gobyexample.com/variadic-functions)을 사용하면 가변 갯수의 인수를 받는 함수를 정의 할 수 있습니다.

```go
func SumAll(numbersToSum ...[]int) (sums []int) {
    return
}
```

<!-- Try to compile but our tests still don't compile! -->
컴파일을 해봐도 되지 않습니다.

`./sum_test.go:26:9: invalid operation: got != want (slice can only be compared to nil)`

<!-- Go does not let you use equality operators with slices. You _could_ write
a function to iterate over each `got` and `want` slice and check their values
but for convenience sake, we can use [`reflect.DeepEqual`][deepEqual] which is
useful for seeing if _any_ two variables are the same. -->
슬라이스에는 비교 연산자를 사용 할 수 없습니다. 대신에 `got`과 `want`의 값들을 하나씩
반복하면서 비교하는 함수를 작성 할 수 있습니다. 하지만 [`reflect.DeepEqual`][deepEqual]
를 사용하면 좀 더 간단하게 임의의 두 값이 같은지 비교 할 수 있습니다.

```go
func TestSumAll(t *testing.T) {

    got := SumAll([]int{1,2}, []int{0,9})
    want := []int{3, 9}

    if !reflect.DeepEqual(got, want) {
        t.Errorf("got %v want %v", got, want)
    }
}
```

<!-- \(make sure you `import reflect` in the top of your file to have access to `DeepEqual`\) -->
\(`DeepEqual`을 사용하기 위해서 파일의 상단에 `import reflect` 를 정의 했는지 확인합니다.\)

<!-- It's important to note that `reflect.DeepEqual` is not "type safe", the code
will compile even if you did something a bit silly. To see this in action,
temporarily change the test to: -->
`reflect.DeepEqual`은 타입을 검사해 주지 않는 다는 점을 주의해야 합니다.
확인해 보기 위해서 테스트를 조금 수정해 봅니다.

```go
func TestSumAll(t *testing.T) {

    got := SumAll([]int{1,2}, []int{0,9})
    want := "bob"

    if !reflect.DeepEqual(got, want) {
        t.Errorf("got %v want %v", got, want)
    }
}
```

<!-- What we have done here is try to compare a `slice` with a `string`. Which makes
no sense, but the test compiles! So while using `reflect.DeepEqual` is
a convenient way of comparing slices \(and other things\) you must be careful
when using it. -->
이 코드는 `slice`를 `string`과 비교합니다. 아무 의미도 없는 코드이지만 컴파일에는 성공합니다.
이런 특징을 가지고 있기 때문에 `reflect.DeepEqual`을 사용 할 때는 주의 해야합니다.

<!-- Change the test back again and run it, you should have test output looking like this -->
테스트 코드를 원래대로 되돌려서 실행시켜 봅니다. 아래와 같은 문장이 출력 될 겁니다.

`sum_test.go:30: got [] want [3 9]`

<!-- ## Write enough code to make it pass -->
## 테스트를 통과시키기 위한 코드 작성

<!-- What we need to do is iterate over the varargs, calculate the sum using our
`Sum` function from before and then add it to the slice we will return -->
varargs(가변 길이 인수)에 이터레이트 하면서 `Sum`으로 각각의 슬라이스의 합을 구하고 그 결과를
리턴 할 슬라이스에 넣어야 합니다.

```go
func SumAll(numbersToSum ...[]int) []int {
    lengthOfNumbers := len(numbersToSum)
    sums := make([]int, lengthOfNumbers)

    for i, numbers := range numbersToSum {
        sums[i] = Sum(numbers)
    }

    return sums
}
```

<!-- Lots of new things to learn! -->
새로 배워야 할 점이 많습니다.

<!-- There's a new way to create a slice. `make` allows you to create a slice with
a starting capacity of the `len` of the `numbersToSum` we need to work through. -->
먼저, 슬라이스를 생성하는 새로운 방법입니다. `make`를 사용하면 일정 크기를 확보한 슬라이스를 생성
할 수 있습니다. 여기서는 `numberToSum`의 `len`을 초기 크기로 설정합니다.

<!-- You can index slices like arrays with `mySlice[N]` to get the value out or
assign it a new value with `=` -->
또, 슬라이스는 배열처럼 인덱스로 각각의 요소에 접근 할 수 있습니다. `mySlice[N]`로 값을 얻거나
`=`로 값을 대입 할 수 있습니다.

<!-- The tests should now pass -->
이제 테스트가 통과 할 겁니다.

<!-- ## Refactor -->
## 리팩토링

<!-- As mentioned, slices have a capacity. If you have a slice with a capacity of
2 and try to do `mySlice[10] = 1` you will get a _runtime_ error. -->
슬라이스는 고정된 크기를 가지고 있기 때문에 크기 2로 선언된 슬라이스에 `mySlice[10] = 1`과 같이
사용하면 _런타임_ 에러가 발생합니다.

<!-- However, you can use the `append` function which takes a slice and a new value,
returning a new slice with all the items in it. -->
하지만 `append`를 사용하면 인수로 넘긴 슬라이스에 해당 요소를 추가한 새 슬라이스를 얻을 수 있습니다.

```go
func SumAll(numbersToSum ...[]int) []int {
    var sums []int
    for _, numbers := range numbersToSum {
        sums = append(sums, Sum(numbers))
    }

    return sums
}
```

<!-- In this implementation, we are worrying less about capacity. We start with an
empty slice `sums` and append to it the result of `Sum` as we work through the varargs. -->
이렇게 구현하면, 슬라이스의 크기에 대해서는 크게 신경쓰지 않아도 됩니다.
`sums`라는 비어있는 슬라이스에서 시작해서 `Sum`의 결과를 하나씩 더해가면 됩니다.

<!-- Our next requirement is to change `SumAll` to `SumAllTails`, where it now
calculates the totals of the "tails" of each slice. The tail of a collection is
all the items apart from the first one \(the "head"\) -->
다음에 해야할 일은 `SumAll`을 `SumAllTails`로 바꾸는 겁니다. 이 함수는 슬라이스의 "tails"에 있는
각각의 슬라이스의 합을 반환합니다. "tails"은 슬라이스의 인덱스 0 에있는 "head"이외의 모든 요소를 포함합니다.

## Write the test first

```go
func TestSumAllTails(t *testing.T) {
    got := SumAllTails([]int{1,2}, []int{0,9})
    want := []int{2, 9}

    if !reflect.DeepEqual(got, want) {
        t.Errorf("got %v want %v", got, want)
    }
}
```

## Try and run the test

`./sum_test.go:26:9: undefined: SumAllTails`

## Write the minimal amount of code for the test to run and check the failing test output

Rename the function to `SumAllTails` and re-run the test

`sum_test.go:30: got [3 9] want [2 9]`

## Write enough code to make it pass

```go
func SumAllTails(numbersToSum ...[]int) []int {
    var sums []int
    for _, numbers := range numbersToSum {
        tail := numbers[1:]
        sums = append(sums, Sum(tail))
    }

    return sums
}
```

Slices can be sliced! The syntax is `slice[low:high]` If you omit the value on
one of the sides of the `:` it captures everything to the side of it. In our
case, we are saying "take from 1 to the end" with `numbers[1:]`. You might want to
invest some time in writing other tests around slices and experimenting with the
slice operator so you can be familiar with it.

## Refactor

Not a lot to refactor this time.

What do you think would happen if you passed in an empty slice into our
function? What is the "tail" of an empty slice? What happens when you tell Go to
capture all elements from `myEmptySlice[1:]`?

## Write the test first

```go
func TestSumAllTails(t *testing.T) {

    t.Run("make the sums of some slices", func(t *testing.T) {
        got := SumAllTails([]int{1,2}, []int{0,9})
        want := []int{2, 9}

        if !reflect.DeepEqual(got, want) {
            t.Errorf("got %v want %v", got, want)
        }
    })

    t.Run("safely sum empty slices", func(t *testing.T) {
        got := SumAllTails([]int{}, []int{3, 4, 5})
        want := []int{0, 9}

        if !reflect.DeepEqual(got, want) {
            t.Errorf("got %v want %v", got, want)
        }
    })

}
```

## Try and run the test

```text
panic: runtime error: slice bounds out of range [recovered]
    panic: runtime error: slice bounds out of range
```

Oh no! It's important to note the test _has compiled_, it is a runtime error.
Compile time errors are our friend because they help us write software that
works, runtime errors are our enemies because they affect our users.

## Write enough code to make it pass

```go
func SumAllTails(numbersToSum ...[]int) []int {
    var sums []int
    for _, numbers := range numbersToSum {
        if len(numbers) == 0 {
            sums = append(sums, 0)
        } else {
            tail := numbers[1:]
            sums = append(sums, Sum(tail))
        }
    }

    return sums
}
```

## Refactor

Our tests have some repeated code around assertion again, let's extract that into a function

```go
func TestSumAllTails(t *testing.T) {

    checkSums := func(t *testing.T, got, want []int) {
        t.Helper()
        if !reflect.DeepEqual(got, want) {
            t.Errorf("got %v want %v", got, want)
        }
    }

    t.Run("make the sums of tails of", func(t *testing.T) {
        got := SumAllTails([]int{1, 2}, []int{0, 9})
        want := []int{2, 9}
        checkSums(t, got, want)
    })

    t.Run("safely sum empty slices", func(t *testing.T) {
        got := SumAllTails([]int{}, []int{3, 4, 5})
        want := []int{0, 9}
        checkSums(t, got, want)
    })

}
```

A handy side-effect of this is this adds a little type-safety to our code. If
a silly developer adds a new test with `checkSums(t, got, "dave")` the compiler
will stop them in their tracks.

```bash
$ go test
./sum_test.go:52:21: cannot use "dave" (type string) as type []int in argument to checkSums
```

## Wrapping up

We have covered

* Arrays
* Slices
  * The various ways to make them
  * How they have a _fixed_ capacity but you can create new slices from old ones
    using `append`
  * How to slice, slices!
* `len` to get the length of an array or slice
* Test coverage tool
* `reflect.DeepEqual` and why it's useful but can reduce the type-safety of your code

We've used slices and arrays with integers but they work with any other type
too, including arrays/slices themselves. So you can declare a variable of
`[][]string` if you need to.

[Check out the Go blog post on slices][blog-slice] for an in-depth look into
slices. Try writing more tests to demonstrate what you learn from reading it.

Another handy way to experiment with Go other than writing tests is the Go
playground. You can try most things out and you can easily share your code if
you need to ask questions. [I have made a go playground with a slice in it for you to experiment with.](https://play.golang.org/p/ICCWcRGIO68)

[Here is an example](https://play.golang.org/p/bTrRmYfNYCp) of slicing an array 
and how changing the slice affects the original array; but a "copy" of the slice 
will not affect the original array.
[Another example](https://play.golang.org/p/Poth8JS28sc) of why it's a good idea 
to make a copy of a slice after slicing a very large slice.

[for]: ../iteration.md#
[blog-slice]: https://blog.golang.org/go-slices-usage-and-internals
[deepEqual]: https://golang.org/pkg/reflect/#DeepEqual
[slice]: https://golang.org/doc/effective_go.html#slices
