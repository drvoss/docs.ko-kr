---
title: F# 설치
description: 여러 가지 방법으로 F# 를 설치 하는 방법을 알아봅니다.
ms.date: 12/20/2019
ms.openlocfilehash: 302e04f7cf3271516dff88d9d5f18f620b6ede80
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75346559"
---
# <a name="install-f"></a>F# 설치

F#을 환경에 따라 여러 가지 방법으로 설치할 수 있습니다.

## <a name="install-f-with-visual-studio"></a>Visual Studio를 사용하여 F# 설치

1. 처음으로 [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)를 다운로드 하는 경우 Visual Studio 설치 관리자를 실행 합니다. 설치 관리자에서 적절한 버전의 Visual Studio를 설치 합니다.

   Visual Studio가 이미 설치되어 있는 경우 F#을 추가하기 위하여 버전 옆에 있는 **수정**을 선택 합니다.

2. 워크로드 페이지에서 **ASP.NET 및 웹 개발** 워크로드를 선택 합니다. 여기에 .NET Core 지원을 위한 F# 및 ASP.NET Core 프로젝트가 포함 됩니다.

3. 오른쪽 아래 모서리에서 **수정**을 눌러 선택한 모든 항목을 설치 합니다.

   그런 다음 Visual Studio 설치 관리자에서 **시작**을 선택하여 F#을 사용할 수 있는 Visual Studio를 열 수 있습니다.

## <a name="install-f-with-visual-studio-code"></a>Visual Studio Code에서 F# 설치

1. [git](https://git-scm.com/download)를 설치하고 경로에 사용할 수 있는지 확인 합니다. 명령 프롬프트에서 `git --version`를 입력하고 **엔터**키를 눌러 제대로 설치 되었는지 확인할 수 있습니다.

2. [.NET Core SDK](https://dotnet.microsoft.com/download)와 [Visual Studio Code](https://code.visualstudio.com)를 설치합니다.

3. 확장 아이콘을 선택하고 "Ionide"를 검색 합니다.

   Visual Studio Code에서 F#을 지원하기 위해 필요한 유일한 플러그인은 [Ionide fsharp](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-fsharp)입니다. 하지만 [FAKE](https://fake.build/) 지원을 위해 [Ionide FAKE](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-FAKE)를 설치하고 [Paket](https://fsprojects.github.io/Paket/) 지원을 위해 [Ionide Paket](https://marketplace.visualstudio.com/items?itemName=Ionide.Ionide-Paket)을 설치할 수 있습니다. FAKE와 Paket은 각각 프로젝트를 빌드하고 종속성을 관리하기 위한 부가적인  F# 커뮤니티 도구입니다.

## <a name="install-f-with-visual-studio-for-mac"></a>Mac 용 Visual Studio에서 F# 설치

[Mac 용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)에서는 구성에 관계 없이 F#이 기본적으로 설치됩니다.

설치가 완료되면 **Visual Studio 시작**을 선택 합니다. MacOS에서 Finder를 통해 Visual Studio를 열 수도 있습니다.

## <a name="install-f-on-a-build-server"></a>빌드 F# 서버에 설치

.NET Core 또는 .NET SDK를 통해 .NET Framework를 사용하는 경우 빌드 서버에 .NET SDK를 설치 하기만 하면 됩니다. 필요한 모든 항목이 있습니다.

.NET Framework를 사용하는 경우 .NET SDK를 **사용하지 않는** 경우 [Visual Studio Build Tools SKU](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=BuildTools&rel=16)를 Windows Server에 설치해야 합니다. 설치 관리자에서 **.NET 데스크톱 빌드 도구**를 선택하고 설치 관리자 메뉴의 오른쪽에 있는  **F# 컴파일러** 구성 요소를 선택 합니다.
