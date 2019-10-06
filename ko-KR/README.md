<!-- # Learn Go with Tests -->
# 테스트로 배우는 Go

<p align="center">
  <img src="red-green-blue-gophers-smaller.png" />
</p>

[Art by Denise](https://twitter.com/deniseyu21)

![Build Status](https://travis-ci.org/quii/learn-go-with-tests.svg?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/quii/learn-go-with-tests)](https://goreportcard.com/report/github.com/quii/learn-go-with-tests)

<!-- - Formats: [Gitbook](https://quii.gitbook.io/learn-go-with-tests), [EPUB or PDF](https://github.com/quii/learn-go-with-tests/releases) -->
<!-- - Translations: [中文](https://studygolang.gitbook.io/learn-go-with-tests), [Português](https://larien.gitbook.io/aprenda-go-com-testes/) -->
- 포맷: [Gitbook](https://quii.gitbook.io/learn-go-with-tests), [EPUB or PDF](https://github.com/quii/learn-go-with-tests/releases)
- 원문: [English](https://quii.gitbook.io/learn-go-with-tests/)
- 다른 언어: [中文](https://studygolang.gitbook.io/learn-go-with-tests), [Português](https://larien.gitbook.io/aprenda-go-com-testes/)

<!-- ## Why

* Explore the Go language by writing tests
* **Get a grounding with TDD**. Go is a good language for learning TDD because it is a simple language to learn and testing is built-in
* Be confident that you'll be able to start writing robust, well-tested systems in Go
* [Watch a video, or read about why unit testing and TDD is important](why.md) -->

## Why

* 테스트를 작성하며 Go 언어를 탐구해 보세요.
* **TDD로 기반을 다지세요**. Go는 TDD를 배우기 좋은 언어입니다. 언어가 간단해 배우기 쉽고 테스팅 기능이 내장되어 있기 때문입니다.
* 자신감을 갖고 시작하세요. Go 언어로 탄탄하고 잘 테스트된 시스템을 만들 수 있습니다.
* [유닛 테스트와 TDD가 왜 중요한지 읽어보거나 동영상을 보세요.](why.md)


<!-- ## Table of contents -->
## 목차

<!-- ### Go fundamentals -->
### Go 핵심


<!-- 1. [Install Go](install-go.md) - Set up environment for productivity. -->
1. [Go 설치하기](install-go.md) - 생산성을 위한 환경 설정
<!-- 2. [Hello, world](hello-world.md) - Declaring variables, constants, if/else statements, switch, write your first go program and write your first test. Sub-test syntax and closures. -->
2. [Hello, world](hello-world.md) - 변수와 상수 선언, if/else 문, switch, 첫 Go 프로그램과 테스트 작성하기. 서브 테스트 문법과 클로저
<!-- 3. [Integers](integers.md) - Further Explore function declaration syntax and learn new ways to improve the documentation of your code. -->
3. [정수](integers.md) - 더 나아가 함수 선언 문법과 코드 문서화를 향상시키는 새로운 방법을 배웁니다.
<!-- 4. [Iteration](iteration.md) - Learn about `for` and benchmarking. -->
4. [반복](iteration.md) - `for`와 벤치마킹에 대해 배웁니다.
<!-- 5. [Arrays and slices](arrays-and-slices.md) - Learn about arrays, slices, `len`, varargs, `range` and test coverage. -->
5. [배열과 slices](arrays-and-slices.md) - 배열, slices, `len`, varargs, `range` 와 테스트 커버리지에 대해 배웁니다.
<!-- 6. [Structs, methods & interfaces](structs-methods-and-interfaces.md) - Learn about `struct`, methods, `interface` and table driven tests. -->
6. [구조체, 메소드, 인터페이스](structs-methods-and-interfaces.md) - `struct`, methods, `interface`와 테이블 주도 테스트에 대해 배웁니다.
<!-- 7. [Pointers & errors](pointers-and-errors.md) - Learn about pointers and errors. -->
7. [포인터와 에러](pointers-and-errors.md) - 포인터와 에러에 대해 배웁니다.
<!-- 8. [Maps](maps.md) - Learn about storing values in the map data structure. -->
8. [Map](maps.md) - map 데이터 구조에 값을 저장하는 법에 대해 배웁니다.
<!-- 9. [Dependency Injection](dependency-injection.md) - Learn about dependency injection, how it relates to using interfaces and a primer on io. -->
9. [의존성 주입](dependency-injection.md) - 의존성 주입(Dependency Injection)에 대해 배우고 DI가 인터페이스와는 어떤 관련이 있는지, 그리고 I/O(입출력)의 기본에 대해 배웁니다.
<!-- 10. [Mocking](mocking.md) - Take some existing untested code and use DI with mocking to test it. -->
10. [목킹](mocking.md) - 테스트되지 않은 코드를 가지고 목킹과 DI를 이용해서 테스트해보세요.
<!-- 11. [Concurrency](concurrency.md) - Learn how to write concurrent code to make your software faster. -->
11. [동시성](concurrency.md) - 소프트웨어를 더 빠르게 만드는 동시성 코드를 어떻게 작성하는지 배웁니다.
<!-- 12. [Select](select.md) - Learn how to synchronise asynchronous processes elegantly. -->
12. [Select](select.md) - 비동기 프로세스를 동기적으로 우아하게 바꾸는 법을 배웁니다.
<!-- 13. [Reflection](reflection.md) - Learn about reflection -->
13. [Reflection](reflection.md) - reflection에 대해 배웁니다.
<!-- 13. [Sync](sync.md) - Learn some functionality from the sync package including `WaitGroup` and `Mutex` -->
13. [Sync](sync.md) - `WaitGroup`과 `Mutex`를 포함하는 sync 패키지의 기능을 배웁니다.
<!-- 13. [Context](context.md) - Use the context package to manage and cancel long-running processes -->
13. [Context](context.md) - context 패키지로 오래 돌아가는 프로세스를 취소하거나 관리할 수 있습니다.
<!-- 14. [Intro to property based tests](roman-numerals.md) - Practice some TDD with the Roman Numerals kata and get a brief intro to property based tests -->
14. [프로퍼티 기반 테스트 소개](roman-numerals.md) - Roman Numerals 실습으로 TDD를 연습해 보고 프로퍼티 기반 테스트에 대해 간단히 배웁니다.
<!-- 15. [Maths](math.md) - Use the `math` package to draw an SVG clock -->
15. [Math](math.md) - `math` 패키지를 이용해 SVG로 시계를 그릴 수도 있습니다.

<!-- ### Build an application -->
### 어플리케이션 빌드하기

<!-- Now that you have hopefully digested the _Go Fundamentals_ section you have a solid grounding of a majority of Go's language features and how to do TDD. -->
이제 _Go 핵심_ 섹션을 완전히 이해하셨을 거라 믿습니다. 여러분은 Go 언어의 기능 그리고 TDD를 어떻게 하는지에 대해 탄탄한 기초를 쌓았습니다.

<!-- This next section will involve building an application. -->
이번 섹션은 어플리케이션 빌드를 다룹니다.

<!-- Each chapter will iterate on the previous one, expanding the application's functionality as our product owner dictates. -->
각 챕터에서는 그 이전 챕터를 복습하고, 우리 사장님이 이래라 저래라 시키는 대로 어플리케이션의 기능을 확장할 겁니다.

<!-- New concepts will be introduced to help facilitate writing great code but most of the new material will be learning what can be accomplished from Go's standard library. -->
훌륭한 코드를 작성하는데 도움을 주는 새로운 개념에 대해 소개합니다. 하지만 대부분은 Go의 표준 라이브러리로 어떤 것들을 해낼 수 있는지를 다룹니다.

<!-- By the end of this, you should have a strong grasp as to how to iteratively write an application in Go, backed by tests. -->
이 섹션의 마지막에서는, 테스트로 뒷받침되는 Go 어플리케이션을 여러분이 어떻게 반복해서 작성할 수 있는지에 대해 깊은 이해를 갖게 될 것입니다.

<!-- * [HTTP server](http-server.md) - We will create an application which listens to HTTP requests and responds to them.
* [JSON, routing and embedding](json.md) - We will make our endpoints return JSON and explore how to do routing.
* [IO and sorting](io.md) - We will persist and read our data from disk and we'll cover sorting data.
* [Command line & project structure](command-line.md) - Support multiple applications from one code base and read input from command line.
* [Time](time.md) - using the `time` package to schedule activities.
* [WebSockets](websockets.md) - learn how to write and test a server that uses WebSockets. -->
* [HTTP 서버](http-server.md) - HTTP 요청을 받고 반환하는 어플리케이션을 만듭니다.
* [JSON, 라우팅, 임베딩](json.md) - JSON을 반환하는 엔드포인트를 만들고 어떻게 라우팅을 하는지 알아봅니다.
* [IO와 정렬](io.md) - 디스크에 데이터를 저장하고 읽어 보고, 데이터 정렬에 대해 다룹니다.
* [커맨드 라인 & 프로젝트 구조](command-line.md) - 하나의 코드 베이스로 여러 개의 어플리케이션을 지원해보고, 커맨드 라인으로부터 입력을 읽어 봅니다.
* [Time](time.md) - 작업 스케줄링을 위해 `time` 패키지를 사용하기
* [WebSockets](websockets.md) - 웹소켓을 이용하는 서버를 어떻게 작성하고 테스트하는지에 대해 배웁니다.


<!-- ### Questions and answers -->
### 질의응답

<!-- I often run in to questions on the internets like -->
저는 가끔 인터넷에서 이런 질문을 보게 됩니다.

<!-- > How do I test my amazing function that does x, y and z -->
> x랑 y 그리고 z를 하는 내 쩌는 함수를 어떻게 테스트해야 하죠?

<!-- If you have such a question raise it as an issue on github and I'll try and find time to write a short chapter to tackle the issue. I feel like content like this is valuable as it is tackling people's _real_ questions around testing. -->
만약 여러분이 이런 질문을 깃허브에 이슈로 올리면, 저는 그런 이슈와 씨름하는 짧은 글을 쓰려고 시간을 내서 노력합니다.

<!-- * [OS exec](os-exec.md) - An example of how we can reach out to the OS to execute commands to fetch data and keep our business logic testable/
* [Error types](error-types.md) - Example of creating your own error types to improve your tests and make your code easier to work with. -->
* [OS exec](os-exec.md) - 운영체제에 접근해서 명령을 실행하고, 데이터를 받아오고, 비지니스 로직을 테스트 가능하게 유지하는 방법에 대한 예제
* [Error types](error-types.md) - 커스텀 에러 타입을 만들어서 테스트를 개선하고 테스트하기 쉬운 코드를 만드는 방법에 대한 예제

<!-- ## Contributing -->
## 기여 (원문)

* _This project is work in progress_ If you would like to contribute, please do get in touch.
* Read [contributing.md](https://github.com/quii/learn-go-with-tests/tree/842f4f24d1f1c20ba3bb23cbc376c7ca6f7ca79a/contributing.md) for guidelines
* Any ideas? Create an issue

<!-- ## Background -->
## 배경

<!-- I have some experience introducing Go to development teams and have tried different approaches as to how to grow a team from some people curious about Go into highly effective writers of Go systems. -->
저는 개발팀에 Go를 도입한 경험이 좀 있고, 어떻게 하면 Go에 관심있는 팀원들을 아주 효과적인 Go 시스템 개발자들로 성장시킬 수 있을지 다양한 접근을 시도해왔습니다.

<!-- ### What didn't work -->
### 잘 안 먹힌 것

<!-- #### Read _the_ book -->
#### 책 읽기

<!-- An approach we tried was to take [the blue book](https://www.amazon.co.uk/Programming-Language-Addison-Wesley-Professional-Computing/dp/0134190440) and every week discuss the next chapter along with the exercises. -->
우리가 시도해본 접근법 중 하나는 [파란 책 (교보문고)](http://kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788960778320&orderClick=JAj)을 구해다가 매주 한 챕터씩 실습하고 토론하는 것이었습니다.

<!-- I love this book but it requires a high level of commitment. The book is very detailed in explaining concepts, which is obviously great but it means that the progress is slow and steady - this is not for everyone. -->
저는 이 책이 좋았지만, 이 책은 너무 많은 시간과 비용을 요구했습니다. 개념 설명이 아주 자세하고, 확실히 대단한 책입니다. 하지만 이건 이 책의 진도가 느리다는 걸 뜻하죠. (모두에게 그렇지는 않습니다.)

<!-- I found that whilst a small number of people would read chapter X and do the exercises, many people didn't. -->
저는 소수의 사람들만 챕터 X를 읽고 실습을 하고 대부분은 안 한다는 걸 알게 되었습니다.

<!-- #### Solve some problems -->
#### 문제 풀기

<!-- Katas are fun but they are usually limited in their scope for learning a language; you're unlikely to use goroutines to solve a kata. -->
코딩 도장의 문제(_Kata_)를 푸는 건 재밌지만, 언어를 배우는 데 있어서 보통은 범위가 한정적입니다. 여러분이 고루틴을 쓰는 이유가 그저 문제를 풀고 통과하기 위해서인 것만은 아니겠죠.

<!-- Another problem is when you have varying levels of enthusiasm. Some people just learn way more of the language than others and when demonstrating what they have done end up confusing people with features the others are not familiar with. -->
다른 문제는 사람들의 열정 수준이 천차만별이라는 것입니다. 어떤 사람들은 언어에 대해 다른 사람들보다 훨씬 많이 익혀서 자신이 만든 것을 보여주지만 결국 다른 사람들은 기능이 익숙하지 않아 혼란에 빠지기도 합니다.

<!-- This ends up making the learning feel quite _unstructured_ and _ad hoc_. -->
이런 상황은 결국 학습이 체계적이지 않고(_unstructed_) 즉흥적(_ad hoc_)이라고 느껴지게 합니다.

<!-- ### What did work -->
### 잘 먹힌 것

<!-- By far the most effective way was by slowly introducing the fundamentals of the language by reading through [go by example](https://gobyexample.com/), exploring them with examples and discussing them as a group. This was a more interactive approach than "read chapter x for homework". -->
가장 효과적이었던 방법은, [go by example (한국어)](https://mingrammer.com/gobyexample/)을 읽으면서 언어의 기초를 천천히 소개하는 것이었습니다. 예제를 둘러보면서 그룹지어 토론합니다. 이것은 "챕터 x 읽고 과제 해오기" 보다 더 소통적인 접근법입니다.

<!-- Over time the team gained a solid foundation of the _grammar_ of the language so we could then start to build systems. -->
팀원들이 언어의 _문법_에 대해 탄탄한 기초를 쌓으면, 그제서 우린 시스템 빌드를 시작할 수 있습니다.

<!-- This to me seems analogous to practicing scales when trying to learn guitar. -->
저는 이게 기타를 배울 때 음계를 연습하는 것과 비슷하게 보입니다.

<!-- It doesn't matter how artistic you think you are, you are unlikely to write good music without understanding the fundamentals and practicing the mechanics. -->
여러분이 자신이 얼마나 예술적이라고 생각하는지는 아무 상관이 없이, 기초에 대한 이해나 기술 연습 없이 좋은 음악을 작곡할 수는 없을 것입니다.

<!-- ### What works for me -->
### 제게 잘 먹힌 것

<!-- When _I_ learn a new programming language I usually start by messing around in a REPL but eventually, I need more structure. -->
_제가_ 새 프로그래밍 언어를 배울 때는, 보통 REPL을 켜놓고 갖고 놀기 시작합니다. 하지만 결국 저는 구조(structure)를 필요로 하게 됩니다.

<!-- What I like to do is explore concepts and then solidify the ideas with tests. Tests verify the code I write is correct and documents the feature I have learned. -->
저는 개념을 익힌 다음 테스트를 통해서 아이디어를 공고히 하는 것을 좋아합니다. 테스트는 제가 쓴 코드가 맞는지 검증하고, 제가 배운 기능들을 문서화해줍니다.

<!-- Taking my experience of learning with a group and my own personal way I am going to try and create something that hopefully proves useful to other teams. Learning the fundamentals by writing small tests so that you can then take your existing software design skills and ship some great systems. -->
제가 뭔가 시도하고 만들거나 그룹과 함께 배웠던 경험은 다른 팀들에게도 유용할 거라 빋습니다. 작은 테스트를 작성하면서 기초를 배우면 여러분들은 여러분의 기존 소프트웨어 설계 기술을 갖고 훌륭한 시스템을 도입할 수 있을 겁니다.

<!-- ## Who this is for -->
## 누구에게 유용한가

<!-- * People who are interested in picking up Go. -->
* Go에 관심이 있고 익혀보고 싶은 사람
<!-- * People who already know some Go, but want to explore testing with TDD. -->
* Go를 조금 알지만 TDD를 통해 테스팅해보고 싶은 사람

<!-- ## What you'll need -->
## 여러분이 필요한 것

<!-- * A computer! -->
* 컴퓨터!
<!-- * [Installed Go](https://golang.org/) -->
* [Go 설치](https://golang.org/)
<!-- * A text editor -->
* 아무 텍스트 에디터
<!-- * Some experience with programming. Understanding of concepts like `if`, variables, functions etc. -->
* 약간의 프로그래밍 경험. `if`, 변수, 함수 등의 개념에 대한 이해.
<!-- * Comfortable with using the terminal -->
* 터미널을 사용하는 데 익숙함

<!-- ## Feedback -->
## Feedback (원문)

* Add issues/submit PRs [here](https://github.com/quii/learn-go-with-tests) or [tweet me @quii](https://twitter.com/quii)

[MIT license](LICENSE.md)

[Logo is by egonelbre](https://github.com/egonelbre) What a star!
