---
title: Visual Studio에서 F# 시작
description: Visual Studio에서 F#을 사용하는 방법을 알아봅니다.
ms.date: 12/20/2019
ms.openlocfilehash: 9bf47fb08ea3df0aa0d5064043d94f030d45d568
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75337342"
---
# <a name="get-started-with-f-in-visual-studio"></a>Visual Studio에서 F# 시작

F#과 비주얼 F# 도구는 Visual Studio IDE(통합 개발 환경)에서 지원 됩니다.

시작 하려면 [Visual Studio가 F# 지원 센터에 설치](install-fsharp.md#install-f-with-visual-studio)되어 있는지 확인 합니다.

## <a name="create-a-console-application"></a>콘솔 애플리케이션 만들기

Visual Studio의 가장 기본적인 프로젝트 중 하나는 콘솔 앱입니다. 만드는 방법은 다음과 같습니다.

1. Visual Studio 2019를 엽니다.

2. 시작 창에서 **새 프로젝트 만들기**를 선택합니다.

3. **새 프로젝트 만들기** 페이지의 언어 목록에서를 **F#**을 선택 합니다.

4. **콘솔 앱(.Net Core)** 템플릿을 선택하고 **다음**을 선택 합니다.

5. **새 프로젝트 구성** 페이지에서 **프로젝트 이름** 상자에 이름을 입력 합니다. 그런 다음 **만들기**를 선택합니다.

   Visual Studio에서 새 F# 프로젝트를 만듭니다. 솔루션 탐색기 창에서 볼 수 있습니다.

## <a name="write-the-code"></a>코드 작성

몇 가지 코드를 작성하여 시작해 보겠습니다. `Program.fs` 파일을 열고 다음 내용으로 변경합니다.

[!code-fsharp[HelloSquare](~/samples/snippets/fsharp/getting-started/hello-square.fs)]

위의 코드 샘플은 `x` 라는 입력을 사용하고 자신에게 곱하는 `square` 이라는 함수를 정의 합니다. 에서는 F# [형식 유추](../language-reference/type-inference.md)를 사용 하므로 `x` 형식을 지정할 필요가 없습니다. 컴파일러 F# 는 곱하기가 유효한 형식을 이해 하 고 `square` 호출 되는 방법에 따라 `x`에 형식을 할당 합니다. `square` 위로 마우스를 가져가면 다음과 같이 표시됩니다.

```fsharp
val square: x:int -> int
```

이것을 함수 형식 시그니처라고 합니다. 다음과 같이 읽을 수 있습니다. "Square는 x 라는 정수를 사용하고 정수를 생성 하는 함수입니다." 컴파일러는 현재 `int` 형식 `square`를 제공 했습니다. 이는 곱하기가 *모든* 형식에서 제네릭이 아니라 폐쇄형 형식의 집합 이기 때문입니다. `float`F# 와 같이 다른 입력 형식으로 `square`를 호출 하는 경우 컴파일러는 형식 서명을 조정 합니다.

`EntryPoint` 특성을 사용 하 여 데코레이팅된 다른 함수인 `main`이 정의 됩니다. 이 특성은 프로그램 F# 실행을 시작 해야 함을 컴파일러에 알립니다. 함수에 명령줄 인수를 전달하고 정수 코드(일반적으로 `0`)를 반환하는 [C 스타일 프로그래밍 언어](https://en.wikipedia.org/wiki/Entry_point#C_and_C.2B.2B)와 동일한 규칙을 따릅니다.

`12`의 인수로 `square` 함수를 호출 하는 진입점 함수 `main`에 있습니다. 그런 F# 다음 컴파일러는 `int -> int` 되는 `square` 형식 (즉, `int`를 사용하고 `int`를 생성 하는 함수)을 할당 합니다. `printfn`에 대 한 호출은 서식 문자열을 사용하고 결과 (및 새 줄)를 인쇄 하는 형식이 지정 된 인쇄 함수입니다. C 스타일의 프로그래밍 언어와 유사한 형식 문자열에는 전달 된 인수에 해당 하는 매개 변수 (`%d`)가 있습니다 .이 경우에는 `12` 및 `(square 12)`입니다.

## <a name="run-the-code"></a>코드 실행

**Ctrl**+**F5**키를 눌러 코드를 실행 하 고 결과를 볼 수 있습니다. 또는 최상위 메뉴 모음에서 **디버그** > **디버깅 하지 않고 시작** 을 선택할 수 있습니다. 이렇게 하면 디버깅 하지 않고 프로그램이 실행 됩니다.

다음 출력은 Visual Studio에서 연 콘솔 창에 출력 됩니다.

```console
12 squared is: 144!
```

지금까지 Visual Studio에서 첫 번째 F# 프로젝트를 만든 다음 값을 계산 F# 하 고 인쇄 하는 함수를 작성 하 고 프로젝트를 실행 하 여 결과를 확인 했습니다.

## <a name="next-steps"></a>다음 단계

아직 확인하지 않았다면 F# 언어의 핵심 기능 중 일부를 다루는 [F# 둘러보기](../tour.md)를 확인해보세요. 일부 기능과 Visual Studio로 복사하여 실행할 F# 수 있는 다양한 코드 샘플에 대한 개요를 제공 합니다.

> [!div class="nextstepaction"]
> [F# 둘러보기](../tour.md)

## <a name="see-also"></a>참조

- [F# 언어 참조](../language-reference/index.md)
- [형식 유추](../language-reference/type-inference.md)
- [기호 및 연산자 참조](../language-reference/symbol-and-operator-reference/index.md)
