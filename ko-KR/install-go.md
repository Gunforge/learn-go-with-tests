<!-- # Install Go, set up environment for productivity -->
# Go 개발환경 설정하기

<!-- The official installation instructions for Go are available [here](https://golang.org/doc/install). -->
[여기서](https://golang.org/doc/install) 공식 설치 가이드를 확인 할 수 있어요.

<!-- This guide will assume that you are using a package manager for e.g. [Homebrew](https://brew.sh), [Chocolatey](https://chocolatey.org), [Apt](https://help.ubuntu.com/community/AptGet/Howto) or [yum](https://access.redhat.com/solutions/9934). -->
이 문서에서는 여러분이 [Homebrew](https://brew.sh), [Chocolatey](https://chocolatey.org), [Apt](https://help.ubuntu.com/community/AptGet/Howto) or [yum](https://access.redhat.com/solutions/9934) 같은 패키지 매니저를 쓰고 있는걸 전제로 합니다.

<!-- For demonstration purposes we will show the installation procedure for OSX using Homebrew. -->
데모에서는 OSX에서 homebrew를 사용해서 설치 과정을 보여드릴 거에요.

<!-- ## Installation -->
## Go 설치 

<!-- The process of installation is very easy. First, what you have to do is to run this command to install homebrew (brew). Brew has a dependency on Xcode so you should ensure this is installed first. -->
Go를 설치하는건 매우 간단해요. 먼저, homebrew를 설치합니다. Brew를 설치하려면 Xcode가 필요하기 때문에 Xcode가 설치되어 있는지 확인 해주세요.

```sh
xcode-select --install
```

<!-- Then you run the following to install homebrew: -->
끝났다면 아래의 커맨드를 입력해서 homebrew를 설치합니다:

```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

<!-- At this point you can now install Go: -->
이제 Go를 설치해요:

```sh
brew install go
```

<!-- *You should follow any instructions recommended by your package manager. **Note** these may be host os specific*. -->
*패키지 매니저에 따라서 커맨드가 다르기 때문에 사용하는 패키지 매니저의 가이드를 따라주세요. **주의** 이 가이드는 호스트 os에 의존적입니다 (OSX)*

<!-- You can verify the installation with: -->
제대로 설치 되었는지는 아래의 커멘드로 확인 할 수 있어요.

```sh
$ go version
go version go1.10 darwin/amd64
```

<!-- ## Go Environment -->
## Go 개발 환경

<!-- Go is opinionated. -->
Go 가 동작하는 환경은 시스템 전체에 하나로 통합되어 있어요.

<!-- By convention, all Go code lives within a single workspace (folder). This workspace could be anywhere in your machine. If you don't specify, Go will assume $HOME/go as the default workspace. The workspace is identified (and modified) by the environment variable [GOPATH](https://golang.org/cmd/go/#hdr-GOPATH_environment_variable). -->
Go는 공식적으로 모든 Go 코드가 하나의 공간(workspace)안에 있어야 한다는 약속을 정해 뒀어요. 컴퓨터의 임의의 장소를 이 공간으로 설정할 수 있죠. 설정을 하지 않았다면 Go는 $HOME/go 디렉토리를 기본 공간으로 설정해요. 공간을 설정하고 싶다면 환경 변수 [GOPATH](https://golang.org/cmd/go/#hdr-GOPATH_environment_variable) 를 설정하면 돼요.

<!-- You should set the environment variable so that you can use it later in scripts, shells, etc. -->
여러분이 만든 스크립트를 사용하거나, 쉘 환경을 제대로 동작하게 만드려면 환경변수를 설정해야 돼요.

<!-- Update your .bash_profile to contain the following exports: -->
홈에 있는 .bash_profile 파일에 아래 내용을 추가해서 GOPATH를 설정해요.

```sh
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

<!-- *Note* you should open a new shell to pickup these environment variables. -->
*주의* 파일을 수정한 뒤에는 쉘을 새로 열어야 환경변수 설정이 반영돼요.

<!-- Go assumes that your workspace contains a specific directory structure. -->
여러분의 작업 공간은 Go에서 정한 디렉토리 구조를 따라야 정상적으로 작동해요.

<!-- Go places its files in three directories: All source code lives in src, package objects lives in pkg, and the compiled programs live in bin. You can create these directories as follows. -->
이 디렉토리 구조는 세개의 디렉토리를 포함해요. 모든 소스 코드는 src 디렉토리에, 모든 패키지 오브젝트는 pkg에, 모든 컴파일 된 프로그램은 bin에 있어야 해요. 디렉토리 구조를 생성하기 위해서는 아래의 커맨드를 실행하면 돼요.

```sh
mkdir -p $GOPATH/src $GOPATH/pkg $GOPATH/bin
```

<!-- At this point you can _go get_ and the src/package/bin will be installed correctly in the appropriate $GOPATH/xxx directory. -->
이제 _go get_ 을 실행하면 Go가 알아서 src/package/bin 안의 컨텐츠를 설정해 줄거에요.

<!-- ## Go Editor -->
## Go 코드 편집기

Editor preference is very individualistic, you may already have a preference that supports Go. If you don't you should consider an Editor such as [Visual Studio Code](https://code.visualstudio.com), which has exceptional Go support.

You can install it using the following command:

```sh
brew cask install visual-studio-code
```

You can confirm VS Code installed correctly you can run the following in your shell.

```sh
code .
```

VS Code is shipped with very little software enabled, you can enable new software by installing extensions. To add Go support you must install an extension, there are a variety available for VS Code, an exceptional one is [Luke Hoban's package](https://github.com/Microsoft/vscode-go). This can be installed as follows:

```sh
code --install-extension ms-vscode.go
```

When you open a Go file for the first time in VS Code, it will indicate that the Analysis tools are missing, you should click the button to install these. The list of tools that gets installed (and used) by VS Code are available [here](https://github.com/Microsoft/vscode-go/wiki/Go-tools-that-the-Go-extension-depends-on).

<!-- ## Go Debugger -->
## Go 디버거

A good option for debugging Go (that's integrated with VS Code) is Delve. This can be installed as follows using go get:

```sh
go get -u github.com/go-delve/delve/cmd/dlv
```

<!-- ## Go Linting -->
## Go 린트 (Lint)

An improvement over the default linter can be configured using [GolangCI-Lint](https://github.com/golangci/golangci-lint).

This can be installed as follows:

```sh
go get -u github.com/golangci/golangci-lint/cmd/golangci-lint
```

<!-- ## Refactoring and your tooling -->
## 리팩토링을 위한 툴

A big emphasis of this book is around the importance of refactoring.

Your tools can help you do bigger refactoring with confidence.

You should be familiar enough with your editor to perform the following with a simple key combination:

- **Extract/Inline variable**. Being able to take magic values and give them a name lets you simplify your code quickly
- **Extract method/function**. It is vital to be able to take a section of code and extract functions/methods
- **Rename**. You should be able to confidently rename symbols across files.
- **go fmt**. Go has an opinioned formatter called `go fmt`. Your editor should be running this on every file save.
- **Run tests**. It goes without saying that you should be able to do any of the above and then quickly re-run your tests to ensure your refactoring hasn't broken anything

In addition, to help you work with your code you should be able to:

- **View function signature** - You should never be unsure how to call a function in Go. Your IDE should describe a function in terms of its documentation, its parameters and what it returns.
- **View function definition** - If it's still not clear what a function does, you should be able to jump to the source code and try and figure it out yourself.
- **Find usages of a symbol** - Being able to see the context of a function being called can help your decision process when refactoring.

Mastering your tools will help you concentrate on the code and reduce context switching.

<!-- ## Wrapping up -->
## 뭘했죠?

At this point you should have Go installed, an editor available and some basic tooling in place. Go has a very large ecosystem of third party products. We have identified a few useful components here, for a more complete list see https://awesome-go.com.
