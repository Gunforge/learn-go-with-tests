<!-- # Pointers & errors -->
# 포인터 & 에러

<!-- **[You can find all the code for this chapter here](https://github.com/quii/learn-go-with-tests/tree/master/pointers)** -->
**[이번 장의 코드는 여기서 찾을 수 있습니다](https://github.com/quii/learn-go-with-tests/tree/master/pointers)**

<!-- We learned about structs in the last section which let us capture a number of values related around a concept. -->
우리는 지난 장에서 구조체에 대해 배웠습니다. 구조체를 이용하여 하나의 개념과 관련된 수많은 값을 다룰 수 있습니다.

<!-- At some point you may wish to use structs to manage state, exposing methods to let users change the state in a way that you can control. -->
때때로, 구조체를 사용하여 상태를 관리하고 싶을 수 있습니다. 사용자가 메소드를 통해 상태를 바꿀 수 있게 하며, 제어할 수 있는 방식으로 말입니다.

<!-- **Fintech loves Go** and uhhh bitcoins? So let's show what an amazing banking system we can make. -->
**핀테크가 Go를 사랑합니다** 그리고 으음 비트코인도요? 그렇다면 어떤 놀라운 은행 시스템을 우리가 만들 수 있는지 살펴봅시다.

<!-- Let's make a `Wallet` struct which let's us deposit `Bitcoin`. -->
`비트코인`을 보관할 `지갑` 구조체를 만들어봅시다.

<!-- ## Write the test first -->
## 첫 번째 테스트 작성하기

```go
func TestWallet(t *testing.T) {

    wallet := Wallet{}

    wallet.Deposit(10)

    got := wallet.Balance()
    want := 10

    if got != want {
        t.Errorf("got %d want %d", got, want)
    }
}
```

<!-- In the [previous example](./structs-methods-and-interfaces.md) we accessed fields directly with the field name, however in our _very secure wallet_ we don't want to expose our inner state to the rest of the world. We want to control access via methods. -->
[이전 예제](./structs-methods-and-interfaces.md)에서 우리는 필드에 필드 이름으로 직접 접근했습니다. 그러나 우리의 _매우 안전한 지갑_ 에서 우리는 내부 상태가 노출되는 것을 원하지 않습니다. 우리는 접근 방식을 메소드로 제어하길 원합니다.

<!-- ## Try to run the test -->
## 테스트 작동 시키기

`./wallet_test.go:7:12: undefined: Wallet`

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 실패한 테스트 출력을 확인하기 위한 최소한의 코드를 작성하기

<!-- The compiler doesn't know what a `Wallet` is so let's tell it. -->
컴파일러가 `Wallet`이 무엇인지 모르니 알려주도록 합시다.

```go
type Wallet struct { }
```

<!-- Now we've made our wallet, try and run the test again -->
이제 우리의 지갑을 만들었으니, 다시 테스트 해보도록 합시다.

```go
./wallet_test.go:9:8: wallet.Deposit undefined (type Wallet has no field or method Deposit)
./wallet_test.go:11:15: wallet.Balance undefined (type Wallet has no field or method Balance)
```

<!-- We need to define these methods. -->
이러한 메소드를 정의해야 합니다.

<!-- Remember to only do enough to make the tests run. We need to make sure our test fails correctly with a clear error message. -->
기억하세요! 테스트가 돌아가게만 만들면 됩니다. 우리 테스트가 올바르게 실패하며, 명확한 에러 메시지를 출력하게 만들어야 합니다.

```go
func (w Wallet) Deposit(amount int) {

}

func (w Wallet) Balance() int {
    return 0
}
```

<!-- If this syntax is unfamiliar go back and read the structs section. -->
이런 문법이 익숙하지 않다면 다시 돌아가서 구조체 섹션을 읽으세요.

<!-- The tests should now compile and run -->
테스트는 이제 컴파일 되고 돌아갈 것입니다.

`wallet_test.go:15: got 0 want 10`

<!-- ## Write enough code to make it pass -->
## 테스트를 통과할 충분한 코드 작성하기

<!-- We will need some kind of _balance_ variable in our struct to store the state -->
이제 우리 구조체에 상태를 저장하기 위해 _balance_ 변수가 필요할 것입니다.

```go
type Wallet struct {
    balance int
}
```

<!-- In Go if a symbol (so variables, types, functions et al) starts with a lowercase symbol then it is private _outside the package it's defined in_. -->
Go에서 심볼이 소문자로 시작한다면, _정의된 패키지 외부에서_ 그 심볼은 private합니다. (변수, 자료형, 함수도 마찬가지입니다.)

<!-- In our case we want our methods to be able to manipulate this value but no one else. -->
우리의 경우에는 오직 메소드만이 이 값을 조작하길 원합니다.

<!-- Remember we can access the internal `balance` field in the struct using the "receiver" variable. -->
기억하세요! 우리는 `receiver` 변수를 사용하여 구조체 내부 `balance` 필드에 접근할 수 있습니다.

```go
func (w Wallet) Deposit(amount int) {
    w.balance += amount
}

func (w Wallet) Balance() int {
    return w.balance
}
```

<!-- With our career in fintech secured, run our tests and bask in the passing test -->
핀테크 분야에 우리 커리어를 확보하면서, 테스트를 실행하고 통과해봅시다.

`wallet_test.go:15: got 0 want 10`

### ????

<!-- Well this is confusing, our code looks like it should work, we add the new amount onto our balance and then the balance method should return the current state of it. -->
우리 코드는 동작할 것처럼 보이고, 이런 결과는 혼란스럽습니다. 우리는 잔액에 새로운 액수를 더하고, balance 메소드는 잔액의 현재 상태를 반환해야 합니다.

<!-- In Go, **when you call a function or a method the arguments are** _**copied**_. -->
Go에서, **함수나 메소드를 부를 때, 인자들은 _복사됩니다_**

<!-- When calling `func (w Wallet) Deposit(amount int)` the `w` is a copy of whatever we called the method from. -->
`func (w Wallet) Deposit(amount int)`를 부를 때 `w`는 우리가 메소드를 통해 불러오는 것의 인스턴스입니다.

<!-- Without getting too computer-sciency, when you create a value - like a wallet, it is stored somewhere in memory. You can find out what the _address_ of that bit of memory with `&myVal`. -->
너무 컴퓨터공학적으로 가지 않고, 당신이 값을 만든다면 - 지갑과 같은, 값은 메모리 어딘가 저장됩니다. `&myVal`을 통해 그 메모리 _주소_ 를 찾을 수 있습니다.

<!-- Experiment by adding some prints to your code -->
print 구문을 추가하여 실험하기

```go
func TestWallet(t *testing.T) {

    wallet := Wallet{}

    wallet.Deposit(10)

    got := wallet.Balance()

    fmt.Printf("address of balance in test is %v \n", &wallet.balance)

    want := 10

    if got != want {
        t.Errorf("got %d want %d", got, want)
    }
}
```

```go
func (w Wallet) Deposit(amount int) {
    fmt.Printf("address of balance in Deposit is %v \n", &w.balance)
    w.balance += amount
}
```

<!-- The `\n` escape character, prints new line after outputting the memory address. We get the pointer to a thing with the address of symbol; `&`. -->
`\n`는 메모리 주소를 출력한 후에 새로운 줄을 출력합니다. `&`를 사용하여 포인터를 얻을 수 있습니다.

<!-- Now re-run the test -->
이제 테스트를 다시 실행해봅시다.

```text
address of balance in Deposit is 0xc420012268
address of balance in test is 0xc420012260
```

<!-- You can see that the addresses of the two balances are different. So when we change the value of the balance inside the code, we are working on a copy of what came from the test. Therefore the balance in the test is unchanged. -->
두 잔액의 주소가 다른 것을 볼 수 있습니다. 우리가 코드 내부에서 잔액을 변경하여도 테스트의 복사본에서 변경되고 있는 것입니다. 테스트의 잔액은 바뀌지 않습니다.

<!-- We can fix this with _pointers_. [Pointers](https://gobyexample.com/pointers) let us _point_ to some values and then let us change them. So rather than taking a copy of the Wallet, we take a pointer to the wallet so we can change it. -->
_포인터_ 를 사용하여 이를 고칠 수 있습니다. [포인터](https://gobyexample.com/pointers)는 특정한 값을 _가리킬_ 수 있게 하고 우리는 그것을 바꿀 수 있습니다. Wallet의 복사본을 가지고 작업하기보다, wallet의 포인터를 가지고 작업하면 값을 바꿀 수 있습니다.

```go
func (w *Wallet) Deposit(amount int) {
    w.balance += amount
}

func (w *Wallet) Balance() int {
    return w.balance
}
```

<!-- The difference is the receiver type is `*Wallet` rather than `Wallet` which you can read as "a pointer to a wallet". -->
차이점은 receiver 자료형이 `Wallet`이 아닌 `*Wallet`이란 점입니다. Wallet의 포인터라 읽습니다.

<!-- Try and re-run the tests and they should pass. -->
다시 시도해보면 분명 통과할 것입니다.

<!-- Now you might wonder, why did they pass? We didn't dereference the pointer in the function, like so: -->
이제는 궁금할 겁니다. 왜 통과했지? 우리는 아래와 같이 포인터를 역참조하지 않았습니다.

```go
func (w *Wallet) Balance() int {
    return (*w).balance
}
```

<!-- and seemingly addressed the object directly. In fact, the code above using `(*w)` is absolutely valid. However, the makers of Go deemed this notation cumbersome, so the language permits us to write `w.balance`, without explicit dereference. -->
그리고 객체에 주소 자체를 집어넣은 걸로 보입니다. 사실, `(*w)`를 사용한 위 코드도 완전히 유효합니다. 그러나, Go의 설계자들은 이러한 표기법이 거추장스럽다고 간주했고, Go에서 역참조를 이용하지 않고 `w.balance`라 사용할 수 있습니다.
<!-- These pointers to structs even have an own name: _struct pointers_ and they are [automatically dereferenced](https://golang.org/ref/spec#Method_values). -->
구조체를 가리키는 포인터는 그자체로 이름을 가지고 있습니다: _구조체 포인터_ 그리고 구조체 포인터는 [자동으로 역참조](https://golang.org/ref/spec#Method_values)됩니다.

<!-- ## Refactor -->
## 리팩토링

<!-- We said we were making a Bitcoin wallet but we have not mentioned them so far. We've been using `int` because they're a good type for counting things! -->
우리는 비트코인 지갑을 만든다고 했지만 그에 대한 얘기는 거의 얘기하지 않았습니다. `int`는 무언가를 세기 좋은 자료형이기 때문에, 이를 사용했습니다.

<!-- It seems a bit overkill to create a `struct` for this. `int` is fine in terms of the way it works but it's not descriptive. -->
이를 위해 `구조체`를 만드는 건 과해 보입니다. `int`는 작동하는 데엔 좋지만 서술적이진 않습니다.

<!-- Go lets you create new types from existing ones. -->
Go는 기존 자료형을 이용하여 새로운 자료형을 만드는 걸 허용합니다.

<!-- The syntax is `type MyName OriginalType` -->
문법은 `type MyName OriginalType`입니다.

```go
type Bitcoin int

type Wallet struct {
    balance Bitcoin
}

func (w *Wallet) Deposit(amount Bitcoin) {
    w.balance += amount
}

func (w *Wallet) Balance() Bitcoin {
    return w.balance
}
```

```go
func TestWallet(t *testing.T) {

    wallet := Wallet{}

    wallet.Deposit(Bitcoin(10))

    got := wallet.Balance()

    want := Bitcoin(10)

    if got != want {
        t.Errorf("got %d want %d", got, want)
    }
}
```

<!-- To make `Bitcoin` you just use the syntax `Bitcoin(999)`. -->
`Bitcoin`을 만들기 위해 `Bitcoin(999)`와 같은 문법을 사용합니다.

<!-- By doing this we're making a new type and we can declare _methods_ on them. This can be very useful when you want to add some domain specific functionality on top of existing types. -->
이를 통해 새로운 자료형을 만들고 _메소드_ 를 선언할 수 있습니다. 이는 도메인 특화 함수를 만들 때 매우 유용합니다.

<!-- Let's implement [Stringer](https://golang.org/pkg/fmt/#Stringer) on Bitcoin -->
비트코인에 [Stringer](https://golang.org/pkg/fmt/#Stringer)를 구현합니다.

```go
type Stringer interface {
        String() string
}
```

<!-- This interface is defined in the `fmt` package and lets you define how your type is printed when used with the `%s` format string in prints. -->
이 인터페이스는 `fmt` 패키지 안에서 정의되고 당신의 자료형이 `%s` 포맷과 함께 사용될 때 어떻게 출력될 지 정의합니다.

```go
func (b Bitcoin) String() string {
    return fmt.Sprintf("%d BTC", b)
}
```

<!-- As you can see, the syntax for creating a method on a type alias is the same as it is on a struct. -->
위에 보이는 것처럼, 자료형 별칭에 대한 메소드를 생성하는 문법은 구조체에서 메소드를 만드는 것과 같습니다.

<!-- Next we need to update our test format strings so they will use `String()` instead. -->
다음으로 우리의 포맷 스트링이 `String()` 메소드를 사용하도록 업데이트가 필요합니다.

```go
    if got != want {
        t.Errorf("got %s want %s", got, want)
    }
```

<!-- To see this in action, deliberately break the test so we can see it -->
이 코드가 작동하는 걸 보려면, 일부러 테스트를 망쳐봅시다.

`wallet_test.go:18: got 10 BTC want 20 BTC`

<!-- This makes it clearer what's going on in our test. -->
이는 테스트에서 무엇이 진행되는지 명확하게 보여줍니다.

<!-- The next requirement is for a `Withdraw` function. -->
다음 요구사항은 `Withdraw` 함수입니다.

<!-- ## Write the test first -->
## 첫 번째 테스트 작성하기

<!-- Pretty much the opposite of `Deposit()` -->
`Deposit()`과 정반대입니다.

```go
func TestWallet(t *testing.T) {

    t.Run("Deposit", func(t *testing.T) {
        wallet := Wallet{}

        wallet.Deposit(Bitcoin(10))

        got := wallet.Balance()

        want := Bitcoin(10)

        if got != want {
            t.Errorf("got %s want %s", got, want)
        }
    })

    t.Run("Withdraw", func(t *testing.T) {
        wallet := Wallet{balance: Bitcoin(20)}

        wallet.Withdraw(Bitcoin(10))

        got := wallet.Balance()

        want := Bitcoin(10)

        if got != want {
            t.Errorf("got %s want %s", got, want)
        }
    })
}
```

<!-- ## Try to run the test -->
## 테스트 작동 시키기

`./wallet_test.go:26:9: wallet.Withdraw undefined (type Wallet has no field or method Withdraw)`

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 실패한 테스트 출력을 확인하기 위한 최소한의 코드를 작성하기

```go
func (w *Wallet) Withdraw(amount Bitcoin) {

}
```

`wallet_test.go:33: got 20 BTC want 10 BTC`

<!-- ## Write enough code to make it pass -->
## 테스트를 통과할 충분한 코드 작성하기

```go
func (w *Wallet) Withdraw(amount Bitcoin) {
    w.balance -= amount
}
```

<!-- ## Refactor -->
## 리팩토링

There's some duplication in our tests, lets refactor that out.

```go
func TestWallet(t *testing.T) {

    assertBalance := func(t *testing.T, wallet Wallet, want Bitcoin) {
        t.Helper()
        got := wallet.Balance()

        if got != want {
            t.Errorf("got %s want %s", got, want)
        }
    }

    t.Run("Deposit", func(t *testing.T) {
        wallet := Wallet{}
        wallet.Deposit(Bitcoin(10))
        assertBalance(t, wallet, Bitcoin(10))
    })

    t.Run("Withdraw", func(t *testing.T) {
        wallet := Wallet{balance: Bitcoin(20)}
        wallet.Withdraw(Bitcoin(10))
        assertBalance(t, wallet, Bitcoin(10))
    })

}
```

<!-- What should happen if you try to `Withdraw` more than is left in the account? For now, our requirement is to assume there is not an overdraft facility. -->
계좌에 남아있는 잔액보다 더 많이 `Withdraw`하려 하면 무슨 일이 일어나야 할까요? 지금은, 우리 요구사항은 초과 인출이 불가능하다고 추정하는 것입니다.

<!-- How do we signal a problem when using `Withdraw` ? -->
`Withdraw`를 사용할 때 어떻게 문제를 알릴 수 있을까요?

<!-- In Go, if you want to indicate an error it is idiomatic for your function to return an `err` for the caller to check and act on. -->
Go에서, 에러를 보여주고 싶다면 호출자가 확인하고 반응할 수 있게 `err`를 반환하는 게 자연스럽습니다.

<!-- Let's try this out in a test. -->
테스트를 통해 시도해봅시다.

<!-- ## Write the test first -->
## 첫 번째 테스트 작성하기

```go
t.Run("Withdraw insufficient funds", func(t *testing.T) {
    startingBalance := Bitcoin(20)
    wallet := Wallet{startingBalance}
    err := wallet.Withdraw(Bitcoin(100))

    assertBalance(t, wallet, startingBalance)

    if err == nil {
        t.Error("wanted an error but didn't get one")
    }
})
```

<!-- We want `Withdraw` to return an error _if_ you try to take out more than you have and the balance should stay the same. -->
_만약_ 가진 돈보다 더 큰 액수를 인출하려 할 때, `Withdraw`는 에러를 반환해야 하며 잔액은 그대로 남아있어야 한다.

<!-- We then check an error has returned by failing the test if it is `nil`. -->
그 다음, 반환값이 `nil`일 경우 테스트에 통과하지 못하며, 이를 통해 에러가 반환되었는지 확인한다.

<!-- `nil` is synonymous with `null` from other programming languages. Errors can be `nil` because the return type of `Withdraw` will be `error`, which is an interface. If you see a function that takes arguments or returns values that are interfaces, they can be nillable. -->
`nil`은 다른 프로그래밍 언어의 `null`과 같은 말이다. `Withdraw`의 반환값 자료형이 `error` 구현체이기 때문에, 에러는 `nil`이 될 수 있다. 구현체를 인자로 받거나 반환하는 함수는 nil 값이 될 수 있다.

<!-- Like `null` if you try to access a value that is `nil` it will throw a **runtime panic**. This is bad! You should make sure that you check for nils. -->
`null`과 마찬가지로 `nil` 값에 접근하려 하면 **runtime panic**이 발생한다. 이는 나쁘다! nil에 대한 검사를 확실하게 해야 한다.

<!-- ## Try and run the test -->
## 테스트 작동 시키기

`./wallet_test.go:31:25: wallet.Withdraw(Bitcoin(100)) used as value`

<!-- The wording is perhaps a little unclear, but our previous intent with `Withdraw` was just to call it, it will never return a value. To make this compile we will need to change it so it has a return type. -->
단어가 불명확할 수 있다. 그러나 우리 이전 `Withdraw`가 부르는 것이었기 때문에, 절대 값을 반환하지 않을 것이다. 이를 컴파일하기 위해서는 반환 자료형을 가지도록 변경하는 게 필요하다.

<!-- ## Write the minimal amount of code for the test to run and check the failing test output -->
## 실패한 테스트 출력을 확인하기 위한 최소한의 코드를 작성하기

```go
func (w *Wallet) Withdraw(amount Bitcoin) error {
    w.balance -= amount
    return nil
}
```

<!-- Again, it is very important to just write enough code to satisfy the compiler. We correct our `Withdraw` method to return `error` and for now we have to return _something_ so let's just return `nil`. -->
다시 한 번 강조하지만, 컴파일러가 만족할만한 충분한 코드를 작성하는 건 매우 중요하다. 우리는 `Withdraw` 메소드가 `error`를 반환하도록 수정했고, 이제부터 _무언가_를 반환해야 하니, `nil`을 반환하여 보자.

<!-- ## Write enough code to make it pass -->
## 테스트를 통과할 충분한 코드 작성하기

```go
func (w *Wallet) Withdraw(amount Bitcoin) error {

    if amount > w.balance {
        return errors.New("oh no")
    }

    w.balance -= amount
    return nil
}
```

<!-- Remember to import `errors` into your code. -->
코드에 `errors`를 임포트하는 걸 잊지 말자.

<!-- `errors.New` creates a new `error` with a message of your choosing. -->
`errors.New`는 당신이 고른 메시지와 함께 새로운 `error`를 만든다.

<!-- ## Refactor -->
## 리팩토링

<!-- Let's make a quick test helper for our error check just to help our test read clearer -->
에러 체크를 위한 도우미 함수를 만들어보자. 우리 테스트를 더 읽기 좋게 만들어준다. 

```go
assertError := func(t *testing.T, err error) {
    t.Helper()
    if err == nil {
        t.Error("wanted an error but didn't get one")
    }
}
```

<!-- And in our test -->
그리고 우리 테스트에서

```go
t.Run("Withdraw insufficient funds", func(t *testing.T) {
    wallet := Wallet{Bitcoin(20)}
    err := wallet.Withdraw(Bitcoin(100))

    assertBalance(t, wallet, Bitcoin(20))
    assertError(t, err)
})
```

<!-- Hopefully when returning an error of "oh no" you were thinking that we _might_ iterate on that because it doesn't seem that useful to return. -->
바라건대 "oh no" 에러를 반환할 때 _아마_ 반복적이라 생각했을 것이다. 왜냐면 이를 반환하는 건 유용하지 않아 보이기 때문이다.

<!-- Assuming that the error ultimately gets returned to the user, let's update our test to assert on some kind of error message rather than just the existence of an error. -->
결국 에러가 사용자에게 반환된다고 가정해보면, 우리 테스트가 에러의 존재를 잡기보다, 몇몇 에러 메시지에 대해서만 작동하도록 업데이트하자.

<!-- ## Write the test first -->
## 첫 번째 테스트 작성하기

<!-- Update our helper for a `string` to compare against. -->
우리 도우미 함수가 `string` 비교를 수행하도록 업데이트 한다.

```go
assertError := func(t *testing.T, got error, want string) {
    t.Helper()
    if got == nil {
        t.Fatal("didn't get an error but wanted one")
    }

    if got.Error() != want {
        t.Errorf("got %q, want %q", got, want)
    }
}
```

<!-- And then update the caller -->
그리고 함수를 사용하는 부분도 업데이트 한다.

```go
t.Run("Withdraw insufficient funds", func(t *testing.T) {
    startingBalance := Bitcoin(20)
    wallet := Wallet{startingBalance}
    err := wallet.Withdraw(Bitcoin(100))

    assertBalance(t, wallet, startingBalance)
    assertError(t, err, "cannot withdraw, insufficient funds")
})
```

<!-- We've introduced `t.Fatal` which will stop the test if it is called. This is because we don't want to make any more assertions on the error returned if there isn't one around. Without this the test would carry on to the next step and panic because of a nil pointer. -->
`t.Fatal`은 자신이 호출되면 테스트를 중단하는 역할을 한다. 에러가 하나도 없으면 더이상 검증할 필요가 없기 때문이다. 이 부분이 없으면 테스트는 다음 단계로 넘어갈 것이고 nil 포인터 때문에 패닉에 빠지게 된다.

<!-- ## Try to run the test -->
## 테스트 작동 시키기

`wallet_test.go:61: got err 'oh no' want 'cannot withdraw, insufficient funds'`

<!-- ## Write enough code to make it pass -->
## 테스트를 통과할 충분한 코드 작성하기

```go
func (w *Wallet) Withdraw(amount Bitcoin) error {

    if amount > w.balance {
        return errors.New("cannot withdraw, insufficient funds")
    }

    w.balance -= amount
    return nil
}
```

<!-- ## Refactor -->
## 리팩토링

<!-- We have duplication of the error message in both the test code and the `Withdraw` code. -->
테스트 코드와 `Withdraw` 코드의 에러 메시지가 중복됩니다.

<!-- It would be really annoying for the test to fail if someone wanted to re-word the error and it's just too much detail for our test. We don't _really_ care what the exact wording is, just that some kind of meaningful error around withdrawing is returned given a certain condition. -->
누군가 에러를 재작성하길 원한다면 테스트를 실패시킬 때 정말 짜증날 것입니다. 그리고 너무 디테일합니다 우리 테스트에. 우리는 정확한 단어가 뭔지는 _정말_ 신경쓰지 않습니다. 그저 인출 기능의 특정한 조건에서만 발생하는 에러이면 됩니다.

<!-- In Go, errors are values, so we can refactor it out into a variable and have a single source of truth for it. -->
Go에서, 에러는 값이고, 우리는 그걸 변수에 담아 하나의 소스로 리팩토링할 수 있습니다.

```go
var ErrInsufficientFunds = errors.New("cannot withdraw, insufficient funds")

func (w *Wallet) Withdraw(amount Bitcoin) error {

    if amount > w.balance {
        return ErrInsufficientFunds
    }

    w.balance -= amount
    return nil
}
```

<!-- The `var` keyword allows us to define values global to the package. -->
`var` 키워드는 패키지 내에서 전역 변수를 정의할 수 있게 합니다.

<!-- This is a positive change in itself because now our `Withdraw` function looks very clear. -->
이는 긍정적인 변화입니다. 이제 `Withdraw` 함수는 매우 명확해졌습니다.

<!-- Next we can refactor our test code to use this value instead of specific strings. -->
다음으로 테스트 코드가 특정 문자열 대신 이 값을 사용하도록 리팩토링할 수 있습니다.

```go
func TestWallet(t *testing.T) {

    t.Run("Deposit", func(t *testing.T) {
        wallet := Wallet{}
        wallet.Deposit(Bitcoin(10))
        assertBalance(t, wallet, Bitcoin(10))
    })

    t.Run("Withdraw with funds", func(t *testing.T) {
        wallet := Wallet{Bitcoin(20)}
        wallet.Withdraw(Bitcoin(10))
        assertBalance(t, wallet, Bitcoin(10))
    })

    t.Run("Withdraw insufficient funds", func(t *testing.T) {
        wallet := Wallet{Bitcoin(20)}
        err := wallet.Withdraw(Bitcoin(100))

        assertBalance(t, wallet, Bitcoin(20))
        assertError(t, err, ErrInsufficientFunds)
    })
}

func assertBalance(t *testing.T, wallet Wallet, want Bitcoin) {
    t.Helper()
    got := wallet.Balance()

    if got != want {
        t.Errorf("got %q want %q", got, want)
    }
}

func assertError(t *testing.T, got error, want error) {
    t.Helper()
    if got == nil {
        t.Fatal("didn't get an error but wanted one")
    }

    if got != want {
        t.Errorf("got %q, want %q", got, want)
    }
}
```

<!-- And now the test is easier to follow too. -->
그리고 이제 테스트는 흐름을 따라가기 더 쉬워졌습니다.

<!-- I have moved the helpers out of the main test function just so when someone opens up a file they can start reading our assertions first, rather than some helpers. -->
누군가 파일을 열었을 때 도우미 함수보다 assertion을 먼저 읽을 수 있도록, 도우미 함수를 메인 테스트 함수 바깥으로 옮겼습니다.

<!-- Another useful property of tests is that they help us understand the _real_ usage of our code so we can make sympathetic code. We can see here that a developer can simply call our code and do an equals check to `ErrInsufficientFunds` and act accordingly. -->
테스트는 코드의 _진짜_ 사용법을 이해하고 어울리는 코드를 작성하는 데 도움이 됩니다. 개발자가 코드를 호출하고, `ErrInsufficientFunds`와 등가비교를 하고, 제대로 동작하는 걸 볼 수 있습니다.

<!-- ### Unchecked errors -->
### 확인되지 않은 에러

<!-- Whilst the Go compiler helps you a lot, sometimes there are things you can still miss and error handling can sometimes be tricky. -->
Go 컴파일러가 당신을 엄청나게 돕는 동안, 때때로 놓치는 것이나 에러 제어가 힘든 부분이 있을 수 있다.

<!-- There is one scenario we have not tested. To find it, run the following in a terminal to install `errcheck`, one of many linters available for Go. -->
우리가 테스트 하지 않은 한 가지 시나리오가 있다. 그것을 찾기 위해, `errcheck`을 설치하기 위해 터미널에서 다음 명령을 실행하자. `errcheck`은 Go 언어에서 사용할 수 있는 린터 중 하나이다.

`go get -u github.com/kisielk/errcheck`

<!-- Then, inside the directory with your code run `errcheck .` -->
그러고나서, 당신의 코드가 있는 디렉토리 안에서 `errcheck .`를 실행하라.

<!-- You should get something like -->
아래와 같은 메시지가 출력되어야 한다.

`wallet_test.go:17:18: wallet.Withdraw(Bitcoin(10))`

<!-- What this is telling us is that we have not checked the error being returned on that line of code. That line of code on my computer corresponds to our normal withdraw scenario because we have not checked that if the `Withdraw` is successful that an error is _not_ returned. -->
이것은 우리가 해당 행에서 반환되는 에러를 확인하지 않았단 걸 의미한다. 해당 행은 우리의 일반적인 인출 시나리오에 해당한다. 우리가 `Withdraw`가 성공했는지 확인하지 않았기 때문에 에러는 반환되지 _않는다._

<!-- Here is the final test code that accounts for this. -->
여기 이를 계산하는 마지막 테스트 코드가 있다.

```go
func TestWallet(t *testing.T) {

    t.Run("Deposit", func(t *testing.T) {
        wallet := Wallet{}
        wallet.Deposit(Bitcoin(10))

        assertBalance(t, wallet, Bitcoin(10))
    })

    t.Run("Withdraw with funds", func(t *testing.T) {
        wallet := Wallet{Bitcoin(20)}
        err := wallet.Withdraw(Bitcoin(10))

        assertBalance(t, wallet, Bitcoin(10))
        assertNoError(t, err)
    })

    t.Run("Withdraw insufficient funds", func(t *testing.T) {
        wallet := Wallet{Bitcoin(20)}
        err := wallet.Withdraw(Bitcoin(100))

        assertBalance(t, wallet, Bitcoin(20))
        assertError(t, err, ErrInsufficientFunds)
    })
}

func assertBalance(t *testing.T, wallet Wallet, want Bitcoin) {
    t.Helper()
    got := wallet.Balance()

    if got != want {
        t.Errorf("got %s want %s", got, want)
    }
}

func assertNoError(t *testing.T, got error) {
    t.Helper()
    if got != nil {
        t.Fatal("got an error but didn't want one")
    }
}

func assertError(t *testing.T, got error, want error) {
    t.Helper()
    if got == nil {
        t.Fatal("didn't get an error but wanted one")
    }

    if got != want {
        t.Errorf("got %s, want %s", got, want)
    }
}
```

<!-- ## Wrapping up -->
## 요약

<!-- ### Pointers -->
### 포인터

<!-- * Go copies values when you pass them to functions/methods so if you're writing a function that needs to mutate state you'll need it to take a pointer to the thing you want to change. -->
* Go는 값을 함수/메소드에 인자로 넘겼을 때 해당 값을 복사한다. 그러므로 상태를 조작하는 함수를 작성 중이라면, 바꾸길 원하는 값에 대한 포인터를 인자로 받아야 한다.
<!-- * The fact that Go takes a copy of values is useful a lot of the time but sometimes you won't want your system to make a copy of something, in which case you need to pass a reference. Examples could be very large data or perhaps things you intend only to have one instance of \(like database connection pools\). -->
* Go가 복사된 값을 인자로 가진다는 사실은 유용하지만 때때로 시스템이 사본을 만들기 원하지 않을 때가 있다. 예를 들어 매우 큰 데이터나 하나의 인스턴스만 가지길 의도한 경우. \(데이터베이스 연결 풀 같은\)

### nil

<!-- * Pointers can be nil -->
* 포인터는 nil이 될 수 있다.
<!-- * When a function returns a pointer to something, you need to make sure you check if it's nil or you might raise a runtime exception, the compiler won't help you here. -->
* 함수가 무언가의 포인터를 반환할 때, 반환값이 nil인지 확인해야 한다. 그렇지 않으면 런타임 예외가 발생하고 이는 컴파일러가 도움을 줄 수 없는 영역이다.
<!-- * Useful for when you want to describe a value that could be missing -->
* 잃어버릴 수 있는 값에 대해 묘사할 때 유용하다

<!-- ### Errors -->
### 에러

<!-- * Errors are the way to signify failure when calling a function/method. -->
* 에러는 함수/메소드를 호출했을 때 실패를 의미하는 방법이다.
<!-- * By listening to our tests we concluded that checking for a string in an error would result in a flaky test. So we refactored to use a meaningful value instead and this resulted in easier to test code and concluded this would be easier for users of our API too. -->
* 앞선 경우에서 결론 내렸듯이, 에러를 문자열로 구분하는 건 테스트를 신뢰할 수 없게 만든다. 그래서 우리는 의미있는 값을 사용하도록 리팩토링 했다. 이는 코드를 테스트하기 더 쉽게 만들었고, API 사용자가 사용하기에도 더 편하게 만들었다.
<!-- * This is not the end of the story with error handling, you can do more sophisticated things but this is just an intro. Later sections will cover more strategies. -->
* 이것이 에러 제어의 끝은 아니다. 더 복잡한 일을 할 수 있고 이것은 인트로일 뿐이다. 나중에 더많은 기법에 대해 다룰 것이다.
<!-- * [Don’t just check errors, handle them gracefully](https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully) -->
* [에러를 확인만 하지 말고 우아하게 제어하자](https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully)

<!-- ### Create new types from existing ones -->
### 기존 자료형에서 새로운 자료형 만들기

<!-- * Useful for adding more domain specific meaning to values -->
* 값에 도메인 특화 의미를 부여하는데 유용하다
<!-- * Can let you implement interfaces -->
* 인터페이스를 구현할 수 있게 한다

<!-- Pointers and errors are a big part of writing Go that you need to get comfortable with. Thankfully the compiler will _usually_ help you out if you do something wrong, just take your time and read the error. -->
포인터와 에러는 Go를 작성하는데 있어서 익숙해져야할 중요한 부분이다. 감사하게도 컴파일러가 _보통은_ 잘못된 부분을 도와주기 때문에, 시간을 들여 에러를 읽으면 해결할 수 있다.