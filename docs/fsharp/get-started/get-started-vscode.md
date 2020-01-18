---
title: Visual Studio Code에서 F# 시작
description: Visual Studio Code와 Ionide 플러그인 도구 모음으로 F#을 사용하는 방법을 알아봅니다.
ms.date: 12/23/2018
ms.openlocfilehash: 91265303c2954387df0f500940c9af68b3c97dac
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559666"
---
# <a name="get-started-with-f-in-visual-studio-code"></a>Visual Studio Code에서 F# 시작

리팩터링 F# [ide 플러그](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-fsharp) 인을 사용하여 [Visual Studio Code](https://code.visualstudio.com) 을 작성하여 IntelliSense 및 코드를 사용 하는 뛰어난 플랫폼 간 경량 ide (통합 개발 환경) 환경을 가져올 수 있습니다. 이 플러그인을 더 자세히 알고 싶다면 [Ionide.io](http://ionide.io)를 방문합니다.

시작하려면 [F#과 Ionide 플러그인의 설치](install-fsharp.md#install-f-with-visual-studio-code)가 올바르게 되어있는지 확인합니다.

## <a name="create-your-first-project-with-ionide"></a>Ionide를 사용하여 첫 번째 프로젝트 만들기

새 F# 프로젝트를 만들려면 명령줄을 열고 .NET Core CLI를 사용하여 새 프로젝트를 만듭니다.

```dotnetcli
dotnet new console -lang "F#" -o FirstIonideProject
```

완료되면 디렉터리를 프로젝트로 변경하고 Visual Studio Code를 엽니다.

```console
cd FirstIonideProject
code .
```

Visual Studio Code에서 프로젝트가 로드되면 창의 왼쪽에 F# 솔루션 탐색기 창이 표시됩니다. 즉, 사용자가 방금 만든 프로젝트가 성공적으로 로드된 것입니다. 이 시점 이전에 편집기에서 코드를 작성할 수 있지만이 경우 모든 것이 로드가 완료 된 것입니다.

## <a name="configure-f-interactive"></a>대화형 F# 구성

먼저 .NET Core 스크립팅이 기본 스크립팅 환경인지 확인합니다.

1. Visual Studio Code 설정 (**코드** > **기본** 설정 > **설정**)을 엽니다.
1. 용어 **F# 스크립트**를 검색합니다.
1. **Fsharp.core: USE SDK scripts**확인란을 클릭 합니다.

이는 현재 .NET Core 스크립팅을 사용하지 않는 .NET Framework 기반 스크립팅에 사용되는 일부 레거시 동작으로 인해 필요하며, Ionide는 이전 버전과의 호환성을 위해 현재 노력하고 있습니다. 나중에 .NET Core 스크립팅이 기본값이 됩니다.

### <a name="write-your-first-script"></a>첫 번째 스크립트 작성

.NET Core 스크립팅을 사용하도록 Visual Studio Code를 구성했으면 Visual Studio Code에서 탐색기 보기로 이동하여 새 파일을 만듭니다. 이름을 *Myfirstscript*로 합니다.

이제 다음 코드를 추가 합니다.

[!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/to-pig-latin.fsx)]

이 함수는 단어를 [Pig Latin](https://en.wikipedia.org/wiki/Pig_Latin) 형태로 변환합니다. 다음으로 F# Interactive (FSI)를 사용하여 평가합니다.

전체 함수를 강조 표시합니다(11줄 길이여야 함). 강조 표시되면 **Alt** 키를 누르고 **Enter** 키를 누릅니다. 화면 아래쪽에 터미널 창이 표시되고 다음과 같이 표시됩니다.

![Ionide를 사용한 F# Interactive 출력 예시](./media/getting-started-vscode/vscode-fsi.png)

여기서 다음 세 가지 작업이 수행되었습니다.

1. FSI 프로세스를 시작했습니다.
2. FSI 프로세스로 강조 표시된 코드를 전송했습니다.
3. FSI 프로세스에서 전송한 코드를 평가합니다.

[함수](../language-reference/functions/index.md)를 전송했으므로 이제 FSI를 사용하여 해당 함수를 호출할 수 있습니다. 대화형 창에서 다음을 입력합니다.

```fsharp
toPigLatin "banana";;
```

다음과 같은 결과가 나타납니다.

```fsharp
val it : string = "ananabay"
```

이제 모음을 첫 글자로 사용해 보겠습니다. 다음을 입력하세요.

```fsharp
toPigLatin "apple";;
```

다음과 같은 결과가 나타납니다.

```fsharp
val it : string = "appleyay"
```

예상대로 함수가 작동합니다. 축하합니다. 첫 번째 F#함수를 Visual Studio Code에서 작성하고 FSI를 사용하여 평가했습니다!

> [!NOTE]
> FSI에서는 `;;`가 라인의 끝입니다. FSI에서는 여러 줄의 입력을 허용하기 때문입니다. 마지막에 있는 `;;`는 FSI가 코드의 마지막이라는 것을 인지할 수 있게 해줍니다.

## <a name="explaining-the-code"></a>코드 설명

코드가 실제로 수행 하는 작업을 잘 모르는 경우에는 다음 단계를 수행 합니다.

여기에서 볼 수 있듯이 `toPigLatin`은 단어를 입력으로 사용하고 해당 단어의 Pig-Latin 표현으로 변환하는 함수입니다. 이에 대한 규칙은 다음과 같습니다.

단어의 첫 문자가 모음으로 시작 하는 경우 단어의 끝에 "yay"를 추가 합니다. 모음으로 시작 하지 않으면 첫 번째 문자를 단어의 끝으로 이동하여 "ay"를 추가 합니다.

FSI.EXE에서 다음을 발견할 수 있습니다.

```fsharp
val toPigLatin : word:string -> string
```

이는 `toPigLatin`이 `string`을 `word`입력으로 사용하고 다른 `string`를 반환하는 함수임을 나타냅니다. 이를 [함수의 형식 서명](https://fsharpforfunandprofit.com/posts/function-signatures/)이라고 하며, F# 코드를 이해하는 핵심이 되는 F# 기본 부분입니다. 또한 Visual Studio Code에서 함수를 가리키면 이를 확인할 수 있습니다.

함수의 본문에는 다음과 같은 두 가지 요소가 있습니다.

1. 지정 된 문자 (`c`)가 [패턴 일치](../language-reference/pattern-matching.md)를 통해 제공 된 패턴 중 하 나와 일치 하는지 확인하여 모음 인지 여부를 확인 하는 `isVowel`이라는 내부 함수입니다.

   [!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/to-pig-latin.fsx#L2-L6)]

2. 첫 번째 문자가 모음인지 확인하고 첫 번째 문자가 모음인지 여부를 기준으로 입력 문자에서 반환 값을 생성하는 [`if..then..else`](../language-reference/conditional-expressions-if-then-else.md) 식입니다.

   [!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/to-pig-latin.fsx#L8-L11)]

따라서 `toPigLatin` 흐름은 다음과 같습니다.

입력 단어의 첫 문자가 모음 인지 확인 합니다. 이 경우 "yay"를 단어 끝에 연결 합니다. 그렇지 않으면 첫 번째 문자를 단어의 끝으로 이동하여 "ay"를 추가 합니다.

이에 대한 최종 정보는 다음과 같습니다. 다른 많은 언어와 달리 함수에서 반환할 명시적인 명령이 없습니다. F#이 식 기반이고 함수 본문의 마지막 식이 반환 값이기 때문입니다. `if..then..else` 자체가 식이므로 `then` 블록의 본문이나 `else` 블록의 본문은 입력 값에 따라 반환됩니다.

## <a name="turn-the-console-app-into-a-pig-latin-generator"></a>콘솔 앱을 Pig 라틴어 생성기로 전환 합니다.

이 문서의 이전 섹션에서는 초기 함수를 작성하고 FSI.EXE를 F# 사용하여 대화형으로 실행 하는 코드를 작성 하는 일반적인 첫 번째 단계를 보여 주었습니다. 이를 repl 기반 개발 이라고 하며, 여기서 [repl](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) 은 "읽기-평가-인쇄 루프"를 나타냅니다. 작업을 수행할 때까지 기능을 시험해 볼 수 있는 좋은 방법입니다.

REPL 기반 개발의 다음 단계는 작업 코드를 F# 구현 파일로 이동 하는 것입니다. 그런 다음 F# 컴파일러에서 실행할 수 있는 어셈블리로 컴파일할 수 있습니다.

시작하려면 이전에 .NET Core CLI를 사용하여 만든 *Program fs* 파일을 엽니다. 일부 코드는 이미 여기에 있습니다.

그런 다음 `PigLatin` 이라는 새 [`module`](../language-reference/modules.md) 만들고 앞에서 만든 `toPigLatin` 함수를 다음과 같이 복사 합니다.

[!code-fsharp[ToPigLatin](~/samples/snippets/fsharp/getting-started/pig-latin.fs#L3-L14)]

이 모듈은 `main` 함수와 `open System` 선언 아래에 있어야 합니다. 의 F#선언 순서는 중요 하므로 파일에서 호출 하기 전에 함수를 정의 해야 합니다.

이제 `main` 함수에서 인수에 대해 Pig 라틴어 생성기 함수를 호출 합니다.

```fsharp
[<EntryPoint>]
let main argv =
    for name in argv do
        let newName = PigLatin.toPigLatin name
        printfn "%s in Pig Latin is: %s" name newName

    0
```

이제 명령줄에서 콘솔 앱을 실행할 수 있습니다.

```dotnetcli
dotnet run apple banana
```

스크립트 파일과 동일한 결과를 출력 하는 것을 볼 수 있지만 이번에는 실행 중인 프로그램입니다.

## <a name="troubleshooting-ionide"></a>문제가 있는 Ide 문제 해결

다음은 발생할 수 있는 특정 문제를 해결할 수 있는 몇 가지 방법입니다.

1. 코드 편집 기능을 사용하려면 F# 파일을 디스크에 저장하고 Visual Studio Code 작업 영역에 열려 있는 폴더 내에 저장해야 합니다.
1. 시스템이 열려 있는 상태에서 시스템을 변경 하거나 Visual Studio Code를 설치한 경우 Visual Studio Code를 다시 시작 합니다.
1. 프로젝트 디렉터리에 잘못 된 문자가 있는 경우에는 작동 하지 않을 수 있습니다.  이 경우 프로젝트 디렉터리의 이름을 바꿉니다.
1. 제대로 작동하는 IDE 명령이 없으면 [Visual Studio Code 키 바인딩](https://code.visualstudio.com/docs/getstarted/keybindings#_advanced-customization)을 확인하여 실수로 재정의하고 있는지 확인합니다.
1. 컴퓨터에서 IDE가 중단되고 위의 어떤 것도 문제를 해결하지 못한 경우에는 컴퓨터에서 `ionide-fsharp` 디렉터리를 제거하고 플러그인 도구 모음을 다시 설치해 보세요.
1. 프로젝트를 로드하지 못한 경우(F# 솔루션 탐색기 표시 됨) 해당 프로젝트를 마우스 오른쪽 단추로 클릭하고 자세한 **정보 보기**를 클릭하여 더 많은 진단 정보를 얻습니다.

IDE는 F# 커뮤니티의 구성원이 빌드하고 유지 관리하는 오픈 소스 프로젝트입니다. 문제를 보고하고 [ide-vscode-Fsharp.core GitHub 리포지토리](https://github.com/ionide/ionide-vscode-fsharp)에 참여하세요.

이상 ide 개발자와 F# 커뮤니티의 [Gitter channel](https://gitter.im/ionide/ionide-project)에서 추가 도움을 요청할 수도 있습니다.

## <a name="next-steps"></a>다음 단계

F# 및 해당 언어의 기능에 대해 자세히 알아보려면 [둘러보기 F# ](../tour.md)를 참조 하세요.
