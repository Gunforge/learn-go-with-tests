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
