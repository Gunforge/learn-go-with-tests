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

<!-- Editor preference is very individualistic, you may already have a preference that supports Go. If you don't you should consider an Editor such as [Visual Studio Code](https://code.visualstudio.com), which has exceptional Go support. -->
에디터는 취향을 많이 타죠. 좋아하시는 에디터가 있으시면 그대로 쓰시면 돼요. 혹시 아직 좋아하는 에디터가 없으시면 [Visual Studio Code](https://code.visualstudio.com)를 고려해 보세요. 끝내줍니다!

<!-- You can install it using the following command: -->
OSX환경에서는 아래 커맨드를 입력해면 바로 설치할 수 있어요.

```sh
brew cask install visual-studio-code
```

<!-- You can confirm VS Code installed correctly you can run the following in your shell. -->
끝났다면 아래에 있는 커맨드를 입력해서 VS Code가 잘 설치 됐는지 확인해요. 

```sh
code .
```

<!-- VS Code is shipped with very little software enabled, you can enable new software by installing extensions. To add Go support you must install an extension, there are a variety available for VS Code, an exceptional one is [Luke Hoban's package](https://github.com/Microsoft/vscode-go). This can be installed as follows: -->
VS Code는 최소한의 기능들만 가진 상태로 설치되기 때문에, 확장 기능(extension)을 설치해서 사용하는게 좋아요. 우리는 Go를 쓸거니까 [Luke Hoban's package](https://github.com/Microsoft/vscode-go)를 설치하도록 해요. 다음 커맨드를 입력하면 설치할 수 있어요.

```sh
code --install-extension ms-vscode.go
```

<!-- When you open a Go file for the first time in VS Code, it will indicate that the Analysis tools are missing, you should click the button to install these. The list of tools that gets installed (and used) by VS Code are available [here](https://github.com/Microsoft/vscode-go/wiki/Go-tools-that-the-Go-extension-depends-on). -->
VS Code 에서 Go 파일을 열면 이런 저런 툴들을 추천해 줄 거에요. 다 좋은 것들이니까 설치합시다. 설치되는 리스트는 [여기](https://github.com/Microsoft/vscode-go/wiki/Go-tools-that-the-Go-extension-depends-on) 에서 확인 가능해요.

<!-- ## Go Debugger -->
## Go 디버거

<!-- A good option for debugging Go (that's integrated with VS Code) is Delve. This can be installed as follows using go get: -->
Go를 디버깅 하기 위한 툴은 여러 가지가 있지만 VS Code에서 사용하기 좋은건 Delve이에요. go get 커맨드를 이용해서 아래처럼 설치할 수 있어요.

```sh
go get -u github.com/go-delve/delve/cmd/dlv
```

<!-- ## Go Linting -->
## Go 린트 (Lint)

<!-- An improvement over the default linter can be configured using [GolangCI-Lint](https://github.com/golangci/golangci-lint). -->
Go 에서 제공하는 린터도 있지만, 좀 더 많은 기능을 제공하는 [GolangCI-Lint](https://github.com/golangci/golangci-lint)을 설치해 봐요.

<!-- This can be installed as follows: -->
아래의 커맨드로 설치할 수 있어요.

```sh
go get -u github.com/golangci/golangci-lint/cmd/golangci-lint
```

<!-- ## Refactoring and your tooling -->
## 리팩토링을 위한 툴

<!-- A big emphasis of this book is around the importance of refactoring. -->
이 책은 리팩토링을 매우 중요하게 생각하고 계속 강조할 거에요.

<!-- Your tools can help you do bigger refactoring with confidence. -->
리팩토링을 도와주는 툴들이 있으면, 프로그램이 망가질 걱정없이 리팩토링 할 수 있어요.

<!-- You should be familiar enough with your editor to perform the following with a simple key combination: -->
아래의 작업들을 단축키를 사용해서 빠르게 사용하는데에 익숙해지는 걸 추천해요:

<!-- - **Extract/Inline variable**. Being able to take magic values and give them a name lets you simplify your code quickly
- **Extract method/function**. It is vital to be able to take a section of code and extract functions/methods
- **Rename**. You should be able to confidently rename symbols across files.
- **go fmt**. Go has an opinioned formatter called `go fmt`. Your editor should be running this on every file save.
- **Run tests**. It goes without saying that you should be able to do any of the above and then quickly re-run your tests to ensure your refactoring hasn't broken anything -->
- **Extract/Inline variable**. 의미를 알 수 없이 떡하니 적혀있는 숫자들을 변수로서 이름을 부여해서 코드를 깔끔하게 해요.
- **Extract method/function**. 코드를 추출해서 function이나 method로 빼낼 수 있어야 해요.
- **Rename**. 여러 파일의 변수이름을 같이, 빠르게 수정할 수 있어야 해요.
- **go fmt**. Go 에는 `go fmt` 라는 포매터가 있어요. 저장할 떄 마다 에디터에서 이걸 자동으로 실행시킬 수 있어야 해요.
- **Run tests**. 리팩토링을 빠르게 진행하면서, 프로그램의 다른 부분이 망가지지 않았는지 계속 확인 할 수 있어야 해요.


<!-- In addition, to help you work with your code you should be able to: -->
더해서, 코드를 효율적으로 짜기 위해서는 이것들을 할 수 있어야 해요:

<!-- - **View function signature** - You should never be unsure how to call a function in Go. Your IDE should describe a function in terms of its documentation, its parameters and what it returns.
- **View function definition** - If it's still not clear what a function does, you should be able to jump to the source code and try and figure it out yourself.
- **Find usages of a symbol** - Being able to see the context of a function being called can help your decision process when refactoring. -->
- **View function signature** - 함수의 시그니처를 확인해서, 그 함수의 파라미터와 리턴 값들을 확인 할 수 있어야 해요.
- **View function definition** - 시그니처를 봐도 그 함수가 뭘하는지 모르겠을 떄는 빠르게 정의로 이동해서 함수를 확인 할 수 있어야 해요.
- **Find usages of a symbol** - 함수가 다른 곳에서 어떻게 쓰이는지 확인해서 리팩토링을 바르게 진행할 수 있어야 해요.

<!-- Mastering your tools will help you concentrate on the code and reduce context switching. -->
툴을 사용하는데 익숙해지면 코드를 짜는 것 이외의 일들이 단순해져서 더 집중하기 쉬울거에요.

<!-- ## Wrapping up -->
## 뭘했죠?

<!-- At this point you should have Go installed, an editor available and some basic tooling in place. Go has a very large ecosystem of third party products. We have identified a few useful components here, for a more complete list see https://awesome-go.com. -->
여러분은 Go를 설치하고 에디터를 설정했어요. Go는 서드파티를 포함한 매우 큰 에코 시스템을 가지고 있는 언어에요. 이 책에서 그 중 몇몇을 언급할 것이지만 좀 더 많은 컴포넌트들을 확인하고 싶다면 이 링크로 들어가 보세요 https://awesome-go.com.
