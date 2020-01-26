---
title: Mac용 Visual Studio F# 에서 시작
description: Mac용 Visual Studio와 함께를 F# 사용 하는 방법을 알아봅니다.
ms.date: 07/03/2018
ms.openlocfilehash: cd45ab9c59cef76e4bf85a93f39d8e2ee063d200
ms.sourcegitcommit: 93762e1a0dae1b5f64d82eebb7b705a6d566d839
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74552365"
---
# <a name="get-started-with-f-in-visual-studio-for-mac"></a>Mac용 Visual Studio F# 에서 시작

F#및 비주얼 F# 도구는 Mac용 Visual Studio IDE에서 지원 됩니다. [Mac용 Visual Studio 설치](install-fsharp.md#install-f-with-visual-studio-for-mac)되어 있는지 확인 합니다.

## <a name="creating-a-console-application"></a>콘솔 응용 프로그램 만들기

Mac용 Visual Studio에서 가장 기본적인 프로젝트 중 하나는 콘솔 응용 프로그램입니다.  방법은 다음과 같습니다.  Mac용 Visual Studio 열리면 다음을 수행 합니다.

1. **파일** 메뉴에서 **새 솔루션**을 가리킵니다.

2. 새 프로젝트 대화 상자에는 콘솔 응용 프로그램에 대한 두 가지 템플릿이 있습니다.  .NET Framework를 대상으로 하는 다른 > .NET 아래에 있습니다.  다른 템플릿은 .net core를 대상으로 하는 .net Core-> 앱 아래에 있습니다.  이 문서의 목적에 맞게 두 템플릿이 작동 해야 합니다.

3. 콘솔 앱에서 필요한 경우 C# 를 F# 로 변경 합니다.  **다음** 단추를 선택하여 앞으로 이동 합니다.  

4. 프로젝트에 이름을 지정하고 앱에 대해 원하는 옵션을 선택 합니다.  화면 옆의 미리 보기 창에는 선택한 옵션에 따라 생성 되는 디렉터리 구조가 표시 됩니다.  

5. **만들기**를 클릭합니다.  이제 솔루션 탐색기에 F# 프로젝트가 표시 됩니다.

## <a name="writing-your-code"></a>코드 작성

먼저 코드를 작성하여 시작 해 보겠습니다.  `Program.fs` 파일이 열려 있는지 확인하고 해당 내용을 다음으로 바꿉니다.

[!code-fsharp[HelloSquare](~/samples/snippets/fsharp/getting-started/hello-square.fs)]

위의 코드 샘플에서는 `x` 라는 입력을 사용 하는 `square` 함수를 정의하고 그 자체로 곱합니다.  에서는 F# [형식 유추](../language-reference/type-inference.md)를 사용 하므로 `x` 형식을 지정할 필요가 없습니다.  컴파일러 F# 는 곱하기가 유효한 형식을 이해 하 고 `square` 호출 되는 방식에 따라 `x`에 형식을 할당 합니다.  `square`를 마우스로 가리키면 다음과 같이 표시 됩니다.

```console
val square: x:int -> int
```

이를 함수의 형식 서명 이라고 합니다.  다음과 같이 읽을 수 있습니다. "Square는 x 라는 정수를 사용하고 정수를 생성 하는 함수입니다."  컴파일러는 현재 `int` 형식 `square`를 제공 했습니다 .이는 곱하기가 *모든* 형식에서 제네릭이 아니라 폐쇄형 형식 집합에 대해 제네릭 이기 때문입니다.  이 F# 시점에서 컴파일러 `int` 선택 되었지만 `float`와 같은 다른 입력 형식으로 `square`를 호출 하는 경우 형식 시그니처를 조정 합니다.

`EntryPoint` 특성으로 데코 레이트 된 다른 함수인 `main`를 정의 하 여 프로그램 실행을 시작 해야 F# 함을 컴파일러에 알립니다.  이 함수는 명령줄 인수를이 함수에 전달할 수 있는 다른 [C 스타일 프로그래밍 언어](https://en.wikipedia.org/wiki/Entry_point#C_and_C.2B.2B)와 동일한 규칙을 따르며 정수 코드가 반환 됩니다 (일반적으로 `0`).

이 함수에서는 `12`의 인수를 사용하여 `square` 함수를 호출 합니다.  그런 F# 다음 컴파일러는 `int -> int` 되는 `square` 형식 (즉, `int`를 사용하고 `int`를 생성 하는 함수)을 할당 합니다.  `printfn`에 대한 호출은 형식 문자열을 사용 하는 서식 있는 인쇄 함수 이며, 형식 문자열에 지정 된 매개 변수에 해당 하는 매개 변수는 C 스타일 프로그래밍 언어와 비슷하며, 결과와 새 줄을 출력 합니다.

## <a name="running-your-code"></a>코드 실행

최상위 메뉴에서 **실행** 을 클릭 한 다음 **디버깅 하지 않고 시작**을 클릭하여 코드를 실행하고 결과를 볼 수 있습니다.  이렇게 하면 디버깅 하지 않고 프로그램을 실행하고 결과를 볼 수 있습니다.

이제 콘솔 창에 다음으로 표시 된 Mac용 Visual Studio 표시 됩니다.

```console
12 squared is 144!
```

지금까지 Mac용 Visual Studio에서 첫 번째 F# 프로젝트를 만들고 F# 함수를 작성하여 해당 함수를 호출한 결과를 출력 하 고 프로젝트를 실행 하 여 일부 결과를 확인 했습니다.

## <a name="using-f-interactive"></a>대화형 F# 사용

Mac용 Visual Studio에서 비주얼 F# 도구의 가장 유용한 기능 중 하나는 F# 대화형 창입니다.  이를 통해 해당 코드를 호출 하 고 결과를 대화형으로 볼 수 있는 프로세스에 코드를 보낼 수 있습니다.

사용을 시작 하려면 코드에 정의 된 `square` 함수를 강조 표시 합니다.  그런 다음 최상위 메뉴에서 **편집** 을 클릭 합니다.  그런 다음 선택 **영역을 F# 대화형으로 보내기를**선택 합니다.  그러면 F# 대화형 창에서 코드가 실행 됩니다.  또는 선택 영역을 마우스 오른쪽 단추로 클릭 하 고 **대화형으로 선택 항목 F# 보내기를**선택할 수도 있습니다.  F# 대화형 창이 다음과 같이 표시 됩니다.

```console
>

val square : x:int -> int

>
```

이는 함수를 마우스로 가리킬 때 앞서 살펴본 `square` 함수에 대한 동일한 함수 시그니처를 보여 줍니다.  `square`은 이제 F# 대화형 프로세스에서 정의 되었으므로 다른 값으로 호출할 수 있습니다.

```console
> square 12;;
val it : int = 144
>square 13;;
val it : int = 169
```

그러면 함수를 실행하여 `it`새 이름에 결과를 바인딩하고 `it`의 형식과 값을 표시 합니다.  `;;`를 사용하여 각 줄을 종료 해야 합니다.  이는 함수 F# 호출이 완료 될 때 대화형으로 인식 되는 방법입니다.  대화형으로 F# 새 함수를 정의할 수도 있습니다.

```console
> let isOdd x = x % 2 <> 0;;

val isOdd : x:int -> bool

> isOdd 12;;
val it : bool = false
```

위의에서는 `int`를 사용하고 홀수 인지 확인 하는 새 함수인 `isOdd`를 정의 합니다.  이 함수를 호출 하 여 다른 입력으로 반환 되는 항목을 확인할 수 있습니다.  함수 호출 내에서 함수를 호출할 수 있습니다.

```console
> isOdd (square 15);;
val it : bool = true
```

[파이프 전달 연산자](../language-reference/symbol-and-operator-reference/index.md) 를 사용하여 값을 두 개의 함수로 파이프라인 할 수도 있습니다.

```console
> 15 |> square |> isOdd;;
val it : bool = true
```

파이프 전달 연산자는 이후 자습서에서 설명 합니다.

이는 대화형으로 F# 수행할 수 있는 작업에만 해당 됩니다.  자세히 알아보려면를 [사용 하 F#여 대화형 프로그래밍 ](../tutorials/fsharp-interactive/index.md)을 확인 하세요.

## <a name="next-steps"></a>다음 단계

아직 없는 경우 F# 언어의 핵심 기능 중 일부를 포함 하는 [둘러보기 F# ](../tour.md)를 확인 하세요. F#기능 중 일부에 대한 개요를 제공하고 Mac용 Visual Studio로 복사하여 실행할 수 있는 충분 한 코드 샘플을 제공 합니다. 또한 사용할 수 있는 몇 가지 유용한 외부 리소스도 [ F# 가이드](../index.yml)에서 안내합니다.

## <a name="see-also"></a>참조

- [F# 설명서](../index.yml)
- [F# 둘러보기](../tour.md)
- [F#언어 참조](../language-reference/index.md)
- [형식 유추](../language-reference/type-inference.md)
- [기호 및 연산자 참조](../language-reference/symbol-and-operator-reference/index.md)
