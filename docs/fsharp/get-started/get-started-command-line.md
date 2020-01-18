---
title: 명령줄 도구를 사용하여 F# 시작
description: 운영 체제 (Windows, macOS 또는 Linux)에서 F# .NET Core CLI 사용하여 간단한 다중 프로젝트 솔루션을 빌드하는 방법에 대해 알아봅니다.
ms.date: 03/26/2018
ms.openlocfilehash: aa3ed84660a951eeafc11a00ea3831f587b6d876
ms.sourcegitcommit: f8c36054eab877de4d40a705aacafa2552ce70e9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75559489"
---
# <a name="get-started-with-f-with-the-net-core-cli"></a>.NET Core CLI를 사용하여 F# 시작

이 문서에서는 .NET Core CLI를 사용하여 운영 F# 체제 (Windows, Macos 또는 Linux)에서를 시작 하는 방법을 설명 합니다. 콘솔 응용 프로그램에서 호출 하는 클래스 라이브러리를 사용하여 다중 프로젝트 솔루션을 빌드하는 과정을 거칩니다.

## <a name="prerequisites"></a>전제 조건

시작하려면 최신 [.NET Core SDK](https://dotnet.microsoft.com/download)를 설치해야 합니다.

이 문서에서는 명령줄과 원하는 텍스트 편집기의 사용 방법을 알고 있다고 가정합니다. 아직 사용하고 있지 않은 경우라면, [Visual Studio Code](get-started-vscode.md)는 F#용 텍스트 편집기로 선택할 수 있는 훌륭한 옵션입니다.

## <a name="build-a-simple-multi-project-solution"></a>간단한 다중 프로젝트 솔루션 구축

명령 프롬프트나 터미널을 열고 `FSNetCore`라는 새 솔루션 파일을 만들기 위해 [dotnet new](../../core/tools/dotnet-new.md) 명령을 다음과 같이 사용합니다.

```dotnetcli
dotnet new sln -o FSNetCore
```

위의 명령을 실행하면 다음과 같은 디렉터리 구조가 생성됩니다.

```console
FSNetCore
    ├── FSNetCore.sln
```

### <a name="write-a-class-library"></a>클래스 라이브러리 작성

*FSNetCore*로 디렉터리를 변경합니다.

`dotnet new` 명령어를 사용하여 **src** 폴더에 Library라는 클래스 라이브러리 프로젝트를 만듭니다.

```dotnetcli
dotnet new classlib -lang "F#" -o src/Library
```

위의 명령을 실행하면 다음과 같은 디렉터리 구조가 생성됩니다.

```console
└── FSNetCore
    ├── FSNetCore.sln
    └── src
        └── Library
            ├── Library.fs
            └── Library.fsproj
```

`Library.fs`의 내용을 다음 코드로 변경합니다.

```fsharp
module Library

open Newtonsoft.Json

let getJsonNetJson value =
    sprintf "I used to be %s but now I'm %s thanks to JSON.NET!" value (JsonConvert.SerializeObject(value))
```

라이브러리 프로젝트에 Newtonsoft.json NuGet 패키지를 추가 합니다.

```dotnetcli
dotnet add src/Library/Library.fsproj package Newtonsoft.Json
```

다음과 같이 [dotnet sln add](../../core/tools/dotnet-sln.md)명령을 사용하여 `FSNetCore` 솔루션에 `Library` 프로젝트를 추가합니다.

```dotnetcli
dotnet sln add src/Library/Library.fsproj
```

`dotnet build`를 실행하여 프로젝트를 빌드합니다. 확인되지 않은 종속성은 빌드할 때 복원됩니다.

### <a name="write-a-console-application-that-consumes-the-class-library"></a>클래스 라이브러리를 사용하는 콘솔 응용 프로그램 작성

다음과 같이 `dotnet new` 명령을 사용하여 **src**의 App 폴더에 콘솔 응용 프로그램을 만듭니다.

```dotnetcli
dotnet new console -lang "F#" -o src/App
```

위의 명령을 실행하면 다음과 같은 디렉터리 구조가 생성됩니다.

```console
└── FSNetCore
    ├── FSNetCore.sln
    └── src
        ├── App
        │   ├── App.fsproj
        │   ├── Program.fs
        └── Library
            ├── Library.fs
            └── Library.fsproj
```

`Program.fs`파일의 내용을 다음 코드로 대체합니다.

```fsharp
open System
open Library

[<EntryPoint>]
let main argv =
    printfn "Nice command-line arguments! Here's what JSON.NET has to say about them:"

    argv
    |> Array.map getJsonNetJson
    |> Array.iter (printfn "%s")

    0 // return an integer exit code
```

다음과 같이 [dotnet add reference](../../core/tools/dotnet-add-reference.md)를 사용하여 `Library`에 참조를 추가합니다.

```dotnetcli
dotnet add src/App/App.fsproj reference src/Library/Library.fsproj
```

다음과 같이 `dotnet sln add` 명령을 사용하여 `FSNetCore`솔루션에 `App` 프로젝트를 추가합니다.

```dotnetcli
dotnet sln add src/App/App.fsproj
```

`dotnet restore`로 NuGet 종속성을 복원하고 `dotnet build`을 실해하여 프로젝트를 빌드합니다.

다음과 같이 `src/App` 콘솔 프로젝트로 디렉터리를 변경하고, `Hello World` 인수를 전달하여 프로젝트를 실행합니다.

```console
cd src/App
dotnet run Hello World
```

다음 결과가 보일 것입니다.

```console
Nice command-line arguments! Here's what JSON.NET has to say about them:

I used to be Hello but now I'm ""Hello"" thanks to JSON.NET!
I used to be World but now I'm ""World"" thanks to JSON.NET!
```

## <a name="next-steps"></a>다음 단계

다음으로 다양한 F#기능을 알아보기 위해 [F# 둘러보기](../tour.md)를 확인합니다.
