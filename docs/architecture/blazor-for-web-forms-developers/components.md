---
title: Blazor를 사용 하 여 재사용 가능한 UI 구성 요소 빌드
description: Blazor를 사용 하 여 재사용 가능한 UI 구성 요소를 빌드하고 ASP.NET Web Forms 컨트롤과 비교 하는 방법을 알아봅니다.
author: danroth27
ms.author: daroth
ms.date: 09/18/2019
ms.openlocfilehash: b34bdf61a425807030cf7648df245cc7a01c95de
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75705732"
---
# <a name="build-reusable-ui-components-with-blazor"></a>Blazor를 사용 하 여 재사용 가능한 UI 구성 요소 빌드

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

ASP.NET Web Forms에 대 한 멋진 사항 중 하나는 다시 사용할 수 있는 ui (사용자 인터페이스) 코드를 다시 사용할 수 있는 UI 컨트롤에 캡슐화 하는 방법입니다. *.Ascx* 파일을 사용 하 여 태그에서 사용자 지정 사용자 정의 컨트롤을 정의할 수 있습니다. 전체 디자이너 지원으로 코드에서 정교한 서버 컨트롤을 빌드할 수도 있습니다.

Blazor는 *구성 요소*를 통한 UI 캡슐화도 지원 합니다. 구성 요소:

- 는 자체 포함 UI의 청크입니다.
- 자체 상태 및 렌더링 논리를 유지 관리 합니다.
- 는 UI 이벤트 처리기를 정의 하 고, 입력 데이터에 바인딩하고, 자체 수명 주기를 관리할 수 있습니다.
- 는 일반적으로 Razor 구문를 사용 하 여 *razor* 파일에 정의 됩니다.

## <a name="an-introduction-to-razor"></a>Razor 소개

Razor는 HTML 및를 기반으로 하 C#는 간단한 태그 템플릿 언어입니다. Razor를 사용 하면 태그와 C# 코드 간을 원활 하 게 전환 하 여 구성 요소 렌더링 논리를 정의할 수 있습니다. *Razor* 파일이 컴파일되면 렌더링 논리가 .net 클래스에서 구조화 된 방식으로 캡처됩니다. 컴파일된 클래스의 이름은 *razor* 파일 이름에서 가져옵니다. 네임 스페이스는 프로젝트 및 폴더 경로에 대 한 기본 네임 스페이스에서 가져온 것 이며, `@namespace` 지시어를 사용 하 여 네임 스페이스를 명시적으로 지정할 수 있습니다 (아래 Razor 지시문에 추가).

구성 요소의 렌더링 논리는를 사용 하 여 C#동적 논리가 추가 된 일반 HTML 태그를 사용 하 여 작성 됩니다. `@` 문자는로 C#전환 하는 데 사용 됩니다. Razor는 일반적으로 HTML로 다시 전환 하는 경우를 파악 하는 데 유용 합니다. 예를 들어 다음 구성 요소는 현재 시간을 사용 하 여 `<p>` 태그를 렌더링 합니다.

```razor
<p>@DateTime.Now</p>
```

C# 식의 시작과 끝을 명시적으로 지정 하려면 괄호를 사용 합니다.

```razor
<p>@(DateTime.Now)</p>
```

Razor를 사용 하면 렌더링 논리에서 C# 제어 흐름을 쉽게 사용할 수 있습니다. 예를 들어 다음과 같이 HTML을 조건부로 렌더링할 수 있습니다.

```razor
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

또는 다음과 같이 일반적인 C# `foreach` 루프를 사용 하 여 항목 목록을 생성할 수 있습니다.

```razor
<ul>
@foreach (var item in items)
{
    <li>item.Text</li>
}
</ul>
```

ASP.NET Web Forms 지시문과 같은 razor 지시문은 Razor 구성 요소가 컴파일되는 방법의 다양 한 측면을 제어 합니다. 구성 요소의 예는 다음과 같습니다.

- 네임스페이스
- 기본 클래스
- 구현 된 인터페이스
- 제네릭 매개 변수
- 가져온 네임 스페이스
- 경로

Razor 지시문은 `@` 문자로 시작 하 고 일반적으로 파일의 시작 부분에서 새 줄의 시작 부분에 사용 됩니다. 예를 들어 `@namespace` 지시문은 구성 요소의 네임 스페이스를 정의 합니다.

```razor
@namespace MyComponentNamespace
```

다음 표에서는 Blazor에서 사용 되는 다양 한 Razor 지시문과 해당 ASP.NET Web Forms 해당 하는 경우에 대해 요약 합니다.

|Directive    |설명|예|Web Forms 동일|
|-------------|-----------|-------|--------------------|
|`@attribute` |구성 요소에 클래스 수준 특성을 추가 합니다.|`@attribute [Authorize]`|None|
|`@code`      |구성 요소에 클래스 멤버를 추가 합니다.|`@code { ... }`|`<script runat="server">...</script>`|
|`@implements`|지정 된 인터페이스를 구현 합니다.|`@implements IDisposable`|코드 숨김 사용|
|`@inherits`  |지정 된 기본 클래스에서 상속 됩니다.|`@inherits MyComponentBase`|`<%@ Control Inherits="MyUserControlBase" %>`|
|`@inject`    |구성 요소에 서비스를 삽입 합니다.|`@inject IJSRuntime JS`|None|
|`@layout`    |구성 요소에 대 한 레이아웃 구성 요소를 지정 합니다.|`@layout MainLayout`|`<%@ Page MasterPageFile="~/Site.Master" %>`|
|`@namespace` |구성 요소에 대 한 네임 스페이스를 설정 합니다.|`@namespace MyNamespace`|None|
|`@page`      |구성 요소의 경로를 지정 합니다.|`@page "/product/{id}"`|`<%@ Page %>`|
|`@typeparam` |구성 요소에 대 한 제네릭 형식 매개 변수를 지정 합니다.|`@typeparam TItem`|코드 숨김 사용|
|`@using`     |범위에 가져올 네임 스페이스를 지정 합니다.|`@using MyComponentNamespace`|*Web.config* 에 네임 스페이스 추가|

또한 Razor 구성 요소는 요소에 대 한 *지시문 특성* 을 광범위 하 게 사용 하 여 구성 요소가 컴파일되는 방법 (이벤트 처리, 데이터 바인딩, 구성 요소 & 요소 참조 등)의 다양 한 측면을 제어 합니다. 지시문 특성은 모두 괄호 안의 값이 선택 사항인 일반적인 제네릭 구문을 따릅니다.

```razor
@directive(-suffix(:name))(="value")
```

다음 표에서는 Blazor에 사용 되는 Razor 지시문에 대 한 다양 한 특성을 요약 합니다.

|특성    |설명|예|
|-------------|-----------|-------|
|`@attributes`|특성 사전을 렌더링 합니다.|`<input @attributes="ExtraAttributes" />`|
|`@bind`      |양방향 데이터 바인딩을 만듭니다.    |`<input @bind="username" @bind:event="oninput" />`|
|`@on{event}` |지정 된 이벤트에 대 한 이벤트 처리기를 추가 합니다.|`<button @onclick="IncrementCount">Click me!</button>`|
|`@key`       |컬렉션의 요소를 유지 하기 위해 diff 알고리즘에서 사용할 키를 지정 합니다.|`<DetailsEditor @key="person" Details="person.Details" />`|
|`@ref`       |구성 요소 또는 HTML 요소에 대 한 참조를 캡처합니다.|`<MyDialog @ref="myDialog" />`|

Blazor (`@onclick`, `@bind`, `@ref`등)에서 사용 하는 다양 한 지시문 특성은 아래 단원 및 이후 장에서 설명 합니다.

*.Aspx* 및 *.ascx* 파일에 사용 되는 대부분의 구문에는 Razor의 병렬 구문이 있습니다. 다음은 ASP.NET Web Forms 및 Razor의 구문에 대 한 간단한 비교입니다.

|기능                      |Web Forms           |구문               |Razor         |구문 |
|-----------------------------|--------------------|---------------------|--------------|-------|
|지시문                   |`<%@ [directive] %>`|`<%@ Page %>`        |`@[directive]`|`@page`|
|코드 블록                  |`<% %>`             |`<% int x = 123; %>` |`@{ }`        |`@{ int x = 123; }`|
|표현식<br>(HTML 인코딩)|`<%: %>`            |`<%:DateTime.Now %>` |암시적: `@`<br>명시적: `@()`|`@DateTime.Now`<br>`@(DateTime.Now)`|
|설명                     |`<%-- --%>`         |`<%-- Commented --%>`|`@* *@`       |`@* Commented *@`|
|데이터 바인딩                 |`<%# %>`            |`<%# Bind("Name") %>`|`@bind`       |`<input @bind="username" />`|

Razor 구성 요소 클래스에 멤버를 추가 하려면 `@code` 지시문을 사용 합니다. 이 기법은 ASP.NET Web Forms 사용자 정의 컨트롤 또는 페이지에서 `<script runat="server">...</script>` 블록을 사용 하는 것과 비슷합니다.

```razor
@code {
    int count = 0;

    void IncrementCount()
    {
        count++;
    }
}
```

Razor는 C#을 기반으로 하기 때문에 C# 프로젝트 ( *.csproj*) 내에서 컴파일해야 합니다. Visual Basic 프로젝트 ( *.vbproj*)에서 *razor* 파일을 컴파일할 수 없습니다. Blazor 프로젝트에서 Visual Basic 프로젝트를 계속 참조할 수 있습니다. 반대의 경우도 마찬가지입니다.

전체 Razor 구문 참조는 [ASP.NET Core에 대 한 Razor 구문 참조](/aspnet/core/mvc/views/razor)를 참조 하세요.

## <a name="use-components"></a>구성 요소 사용

일반 HTML 외에도 구성 요소는 렌더링 논리의 일부로 다른 구성 요소를 사용할 수 있습니다. Razor에서 구성 요소를 사용 하는 구문은 ASP.NET Web Forms 앱에서 사용자 정의 컨트롤을 사용 하는 것과 비슷합니다. 구성 요소는 구성 요소의 형식 이름과 일치 하는 요소 태그를 사용 하 여 지정 됩니다. 예를 들어 다음과 같이 `Counter` 구성 요소를 추가할 수 있습니다.

```razor
<Counter />
```

ASP.NET Web Forms와 달리 Blazor의 구성 요소는 다음과 같습니다.

- 요소 접두사를 사용 하지 않습니다 (예: `asp:`).
- 페이지 또는 *web.config*에서 등록이 필요 하지 않습니다.

.NET 형식 처럼 Razor 구성 요소는 정확히 일치 해야 합니다. 구성 요소가 포함 된 어셈블리를 참조 하는 경우에는 구성 요소를 사용할 수 있습니다. 구성 요소의 네임 스페이스를 범위에 가져오려면 `@using` 지시어를 적용 합니다.

```razor
@using MyComponentLib

<Counter />
```

기본 Blazor 프로젝트에 표시 된 것 처럼, 동일한 디렉터리 및 하위 디렉터리에 있는 모든 *. razor* 파일에 `@using` 지시문을 *_Imports razor* 파일에 배치 하는 것이 일반적입니다.

구성 요소에 대 한 네임 스페이스가 범위에 없는 경우에서 C#가능 하면 전체 형식 이름을 사용 하 여 구성 요소를 지정할 수 있습니다.

```razor
<MyComponentLib.Counter />
```

## <a name="component-parameters"></a>구성 요소 매개 변수

ASP.NET Web Forms에서 공용 속성을 사용 하 여 매개 변수 및 데이터를 컨트롤에 전달할 수 있습니다. 이러한 속성은 특성을 사용 하 여 태그에서 설정 하거나 코드에서 직접 설정할 수 있습니다. Blazor 구성 요소는 구성 요소 매개 변수로 간주 되는 `[Parameter]` 특성으로도 표시 되어야 하지만 비슷한 방식으로 작동 합니다.

다음 `Counter` 구성 요소는 단추를 클릭할 때마다 `Counter` 증가 해야 하는 양을 지정 하는 데 사용할 수 있는 `IncrementAmount` 라는 구성 요소 매개 변수를 정의 합니다.

```razor
<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    [Parameter]
    public int IncrementAmount { get; set; } = 1;

    void IncrementCount()
    {
        currentCount+=IncrementAmount;
    }
}
```

Blazor에서 구성 요소 매개 변수를 지정 하려면 ASP.NET Web Forms에서와 같이 특성을 사용 합니다.

```razor
<Counter IncrementAmount="10" />
```

## <a name="event-handlers"></a>이벤트 처리기

ASP.NET Web Forms 및 Blazor 모두 UI 이벤트를 처리 하기 위한 이벤트 기반 프로그래밍 모델을 제공 합니다. 이러한 이벤트의 예로는 단추 클릭 및 텍스트 입력이 있습니다. ASP.NET Web Forms에서는 HTML 서버 컨트롤을 사용 하 여 DOM에서 노출 하는 UI 이벤트를 처리 하거나 웹 서버 컨트롤에서 노출 하는 이벤트를 처리할 수 있습니다. 이벤트는 폼 다시 게시 요청을 통해 서버에 표시 됩니다. 다음 Web Forms 단추 클릭 예를 살펴보겠습니다.

*Counter. .ascx*

```aspx-csharp
<asp:Button ID="ClickMeButton" runat="server" Text="Click me!" OnClick="ClickMeButton_Click" />
```

*Counter.ascx.cs*

```csharp
public partial class Counter : System.Web.UI.UserControl
{
    protected void ClickMeButton_Click(object sender, EventArgs e)
    {
        Console.WriteLine("The button was clicked!");
    }
}
```

Blazor에서는 양식 `@on{event}`지시문 특성을 사용 하 여 직접 DOM UI 이벤트 처리기를 등록할 수 있습니다. `{event}` 자리 표시자는 이벤트의 이름을 나타냅니다. 예를 들어 다음과 같이 단추 클릭을 수신할 수 있습니다.

```razor
<button @onclick="OnClick">Click me!</button>

@code {
    void OnClick()
    {
        Console.WriteLine("The button was clicked!);
    }
}
```

이벤트 처리기는 선택적 이벤트 관련 인수를 수락 하 여 이벤트에 대 한 자세한 정보를 제공할 수 있습니다. 예를 들어 마우스 이벤트는 `MouseEventArgs` 인수를 사용할 수 있지만 반드시 필요한 것은 아닙니다.

```razor
<button @onclick="OnClick">Click me!</button>

@code {
    void OnClick(MouseEventArgs e)
    {
        Console.WriteLine($"Mouse clicked at {e.ScreenX}, {e.ScreenY}.");
    }
}
```

이벤트 처리기에 대 한 메서드 그룹을 참조 하는 대신 람다 식을 사용할 수 있습니다. 람다 식을 사용 하면 다른 범위 내 값을 닫을 수 있습니다.

```razor
@foreach (var buttonLabel in buttonLabels)
{
    <button @onclick="() => Console.WriteLine($"The {buttonLabel} button was clicked!")">@buttonLabel</button>
}
```

이벤트 처리기는 동기적 또는 비동기적으로 실행 될 수 있습니다. 예를 들어 다음 `OnClick` 이벤트 처리기는 비동기적으로 실행 됩니다.

```razor
<button @onclick="OnClick">Click me!</button>

@code {
    async Task OnClick()
    {
        var result = await Http.GetAsync("api/values");
    }
}
```

이벤트가 처리 된 후 구성 요소 상태 변경을 고려 하 여 구성 요소가 렌더링 됩니다. 비동기 이벤트 처리기를 사용 하면 처리기 실행이 완료 된 직후에 구성 요소가 렌더링 됩니다. 비동기 `Task` 완료 되 면 구성 요소가 *다시* 렌더링 됩니다. 비동기 실행 모드는 비동기 `Task` 진행 중인 동안 적절 한 UI를 렌더링할 수 있는 기회를 제공 합니다.

```razor
<button @onclick="ShowMessage">Get message</button>

@if (showMessage)
{
    @if (message == null)
    {
        <p><em>Loading...</em></p>
    }
    else
    {
        <p>The message is: @message</p>
    }
}

@code
{
    bool showMessage = false;
    string message;

    public async Task ShowMessage()
    {
        showMessage = true;
        message = await MessageService.GetMessageAsync();
    }
}
```

구성 요소는 `EventCallback<TValue>`형식의 구성 요소 매개 변수를 정의 하 여 자체 이벤트를 정의할 수도 있습니다. 이벤트 콜백은 선택적 인수, 동기 또는 비동기, 메서드 그룹 또는 람다 식 등 DOM UI 이벤트 처리기의 모든 변형을 지원 합니다.

```razor
<button class="btn btn-primary" @onclick="OnClick">Click me!</button>

@code {
    [Parameter]
    public EventCallback<MouseEventArgs> OnClick { get; set; }
}
```

## <a name="data-binding"></a>데이터 바인딩

Blazor는 UI 구성 요소에서 구성 요소의 상태에 데이터를 바인딩하는 간단한 메커니즘을 제공 합니다. 이 방식은 데이터 소스에서 UI 컨트롤로 데이터를 바인딩하는 ASP.NET Web Forms의 기능과 다릅니다. 데이터 처리 섹션에서 다양 한 데이터 원본의 데이터를 처리 [하](data.md) 는 방법을 다룹니다.

UI 구성 요소에서 구성 요소의 상태로 양방향 데이터 바인딩을 만들려면 `@bind` 지시문 특성을 사용 합니다. 다음 예제에서 확인란의 값은 `isChecked` 필드에 바인딩됩니다.

```razor
<input type="checkbox" @bind="isChecked" />

@code {
    bool isChecked;
}
```

구성 요소가 렌더링 되 면 확인란의 값이 `isChecked` 필드의 값으로 설정 됩니다. 사용자가 확인란을 설정/해제 하면 `onchange` 이벤트가 발생 하 고 `isChecked` 필드가 새 값으로 설정 됩니다. 이 경우 `@bind` 구문은 다음 태그와 동일 합니다.

```razor
<input value="@isChecked" @onchange="(UIChangeEventArgs e) => isChecked = e.Value" />
```

바인딩에 사용 되는 이벤트를 변경 하려면 `@bind:event` 특성을 사용 합니다.

```razor
<input @bind="text" @bind:event="oninput" />
<p>@text</p>

@code {
    string text;
}
```

구성 요소는 해당 매개 변수에 대 한 데이터 바인딩을 지원할 수도 있습니다. 데이터 바인딩하려면 바인딩 가능 매개 변수와 동일한 이름을 사용 하 여 이벤트 콜백 매개 변수를 정의 합니다. 이름에 "Changed" 접미사가 추가 됩니다.

*PasswordBox razor*

```razor
Password: <input
    value="@Password"
    @oninput="OnPasswordChanged"
    type="@(showPassword ? "text" : "password")" />

<label><input type="checkbox" @bind="showPassword" />Show password</label>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();
        return PasswordChanged.InvokeAsync(Password);
    }
}
```

데이터 바인딩을 기본 UI 요소에 연결 하려면 `@bind` 특성을 사용 하는 대신 값을 설정 하 고 UI 요소에서 직접 이벤트를 처리 합니다.

구성 요소 매개 변수에 바인딩하려면 `@bind-{Parameter}` 특성을 사용 하 여 바인딩할 매개 변수를 지정 합니다.

```razor
<PasswordBox @bind-Password="password" />

@code {
    string password;
}
```

## <a name="state-changes"></a>상태 변경

구성 요소의 상태가 일반적인 UI 이벤트 또는 이벤트 콜백 외부에서 변경 된 경우 구성 요소는 다시 렌더링 해야 한다는 것을 수동으로 신호로 보내야 합니다. 구성 요소의 상태가 변경 되었음을 알리려면 구성 요소에서 `StateHasChanged` 메서드를 호출 합니다.

아래 예제에서는 응용 프로그램의 다른 부분에서 업데이트할 수 있는 `AppState` 서비스의 메시지를 구성 요소에 표시 합니다. 구성 요소는 메시지가 업데이트 될 때마다 구성 요소가 렌더링 되도록 `AppState.OnChange` 이벤트를 사용 하 여 `StateHasChanged` 메서드를 등록 합니다.

```csharp
public class AppState
{
    public string Message { get; }

    // Lets components receive change notifications
    public event Action OnChange;

    public void UpdateMessage(string message)
    {
        shortlist.Add(itinerary);
        NotifyStateChanged();
    }

    private void NotifyStateChanged() => OnChange?.Invoke();
}
```

```razor
@inject AppState AppState

<p>App message: @AppState.Message</p>

@code {
    protected override void OnInitialized()
    {
        AppState.OnChange += StateHasChanged
    }
}
```

## <a name="component-lifecycle"></a>구성 요소 수명 주기

ASP.NET Web Forms 프레임 워크에는 모듈, 페이지 및 컨트롤에 대 한 잘 정의 된 수명 주기 방법이 있습니다. 예를 들어 다음 컨트롤은 `Init`, `Load`및 `UnLoad` 수명 주기 이벤트에 대 한 이벤트 처리기를 구현 합니다.

*Counter.ascx.cs*

```csharp
public partial class Counter : System.Web.UI.UserControl
{
    protected void Page_Init(object sender, EventArgs e) { ... }
    protected void Page_Load(object sender, EventArgs e) { ... }
    protected void Page_UnLoad(object sender, EventArgs e) { ... }
}
```

Blazor 구성 요소에는 잘 정의 된 수명 주기도 있습니다. 구성 요소의 수명 주기를 사용 하 여 구성 요소 상태를 초기화 하 고 고급 구성 요소 동작을 구현할 수 있습니다.

모든 Blazor의 구성 요소 수명 주기 메서드에는 동기 버전과 비동기 버전이 모두 있습니다. 구성 요소 렌더링이 동기식입니다. 비동기 논리는 구성 요소 렌더링의 일부로 실행할 수 없습니다. 모든 비동기 논리는 `async` 수명 주기 메서드의 일부로 실행 해야 합니다.

### <a name="oninitialized"></a>OnInitialized 됨

`OnInitialized` 및 `OnInitializedAsync` 메서드는 구성 요소를 초기화 하는 데 사용 됩니다. 구성 요소는 일반적으로 처음 렌더링 된 후에 초기화 됩니다. 구성 요소가 초기화 된 후에는 최종적으로 삭제 되기 전에 여러 번 렌더링 될 수 있습니다. `OnInitialized` 메서드는 ASP.NET Web Forms 페이지 및 컨트롤의 `Page_Load` 이벤트와 비슷합니다.

```csharp
protected override void OnInitialized() { ... }
protected override async Task OnInitializedAsync() { await ... }
```

### <a name="onparametersset"></a>OnParametersSet

`OnParametersSet` 및 `OnParametersSetAsync` 메서드는 구성 요소가 부모 로부터 매개 변수를 수신 하 고 값이 속성에 할당 될 때 호출 됩니다. 이러한 메서드는 구성 요소 초기화 후와 *구성 요소가 렌더링 될*때마다 실행 됩니다.

```csharp
protected override void OnParametersSet() { ... }
protected override async Task OnParametersSetAsync() { await ... }
```

### <a name="onafterrender"></a>OnAfterRender

`OnAfterRender` 및 `OnAfterRenderAsync` 메서드는 구성 요소가 렌더링을 완료 한 후에 호출 됩니다. 요소 및 구성 요소 참조가이 시점에 채워집니다 (아래 개념에 대 한 자세한 정보). 이 시점에서 브라우저와의 상호 작용을 사용 하도록 설정 합니다. DOM 및 JavaScript 실행과의 상호 작용은 안전 하 게 수행할 수 있습니다.

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

`OnAfterRender` 및 `OnAfterRenderAsync`는 *서버에서 사전 렌더링 시 호출 되지 않습니다*.

`firstRender` 매개 변수는 구성 요소가 처음 렌더링 될 때 `true` 됩니다. 그렇지 않으면 해당 값이 `false`됩니다.

### <a name="idisposable"></a>IDisposable

Blazor 구성 요소는 UI에서 구성 요소가 제거 될 때 리소스를 삭제 하기 위해 `IDisposable`를 구현할 수 있습니다. Razor 구성 요소는 `@implements` 지시어를 사용 하 여 `IDispose`를 구현할 수 있습니다.

```razor
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="capture-component-references"></a>캡처 구성 요소 참조

ASP.NET Web Forms에서 해당 ID를 참조 하 여 코드에서 직접 컨트롤 인스턴스를 조작 하는 것이 일반적입니다. Blazor에서 구성 요소에 대 한 참조를 캡처 및 조작할 수도 있지만 일반적이 지 않습니다.

Blazor에서 구성 요소 참조를 캡처하려면 `@ref` 지시어 특성을 사용 합니다. 특성의 값은 참조 된 구성 요소와 형식이 같은 설정 가능한 필드의 이름과 일치 해야 합니다.

```razor
<MyLoginDialog @ref="loginDialog" ... />

@code {
    MyLoginDialog loginDialog;

    void OnSomething()
    {
        loginDialog.Show();
    }
}
```

부모 구성 요소가 렌더링 되 면 필드는 자식 구성 요소 인스턴스로 채워집니다. 그런 다음 구성 요소 인스턴스의 메서드를 호출 하거나 다른 방식으로 조작할 수 있습니다.

구성 요소 참조를 사용 하 여 구성 요소 상태를 직접 조작 하지 않는 것이 좋습니다. 이렇게 하면 구성 요소가 올바른 시간에 자동으로 렌더링 되지 않습니다.

## <a name="capture-element-references"></a>캡처 요소 참조

Blazor 구성 요소는 요소에 대 한 참조를 캡처할 수 있습니다. ASP.NET Web Forms의 HTML 서버 컨트롤과 달리 Blazor의 요소 참조를 사용 하 여 DOM을 직접 조작할 수 없습니다. Blazor는 DOM diff 알고리즘을 사용 하 여 대부분의 DOM 상호 작용을 처리 합니다. Blazor의 캡처된 요소 참조는 불투명 합니다. 그러나 JavaScript interop 호출에서 특정 요소 참조를 전달 하는 데 사용 됩니다. JavaScript interop에 대 한 자세한 내용은 [ASP.NET Core Blazor javascript interop](/aspnet/core/blazor/javascript-interop)를 참조 하세요.

## <a name="templated-components"></a>템플릿 기반 구성 요소

ASP.NET Web Forms에서 *템플릿 기반 컨트롤*을 만들 수 있습니다. 개발자는 템플릿 기반 컨트롤을 사용 하 여 컨테이너 컨트롤을 렌더링 하는 데 사용 되는 HTML 부분을 지정할 수 있습니다. 템플릿 기반 서버 컨트롤을 작성 하는 메커니즘은 복잡 하지만 사용자 지정 가능한 방식으로 데이터를 렌더링 하는 강력한 시나리오를 가능 하 게 합니다. 템플릿 컨트롤의 예로는 `Repeater` 및 `DataList`가 있습니다.

Blazor 구성 요소는 `RenderFragment` 또는 `RenderFragment<T>`형식의 구성 요소 매개 변수를 정의 하 여 템플릿을 만들 수도 있습니다. `RenderFragment`는 구성 요소에서 렌더링할 수 있는 Razor 태그의 청크를 나타냅니다. `RenderFragment<T>`는 렌더링 조각이 렌더링 될 때 지정할 수 있는 매개 변수를 사용 하는 Razor 태그의 청크입니다.

### <a name="child-content"></a>자식 콘텐츠

Blazor 구성 요소는 자식 콘텐츠를 `RenderFragment` 캡처하고 구성 요소 렌더링의 일부로 해당 콘텐츠를 렌더링할 수 있습니다. 자식 콘텐츠를 캡처하려면 `RenderFragment` 형식의 구성 요소 매개 변수를 정의 하 고 `ChildContent`이름을 지정 합니다.

*ChildContentComponent. razor*

```razor
<h1>Component with child content</h1>

<div>@ChildContent</div>

@code {
    [Parameter]
    public RenderFragment ChildContent { get; set; }
}
```

그러면 부모 구성 요소가 일반적인 Razor 구문를 사용 하 여 자식 콘텐츠를 제공할 수 있습니다.

```razor
<ChildContentComponent>
    <p>The time is @DateTime.Now</p>
</ChildContentComponent>
```

### <a name="template-parameters"></a>템플릿 매개 변수

또한 템플릿 기반 Blazor 구성 요소는 `RenderFragment` 또는 `RenderFragment<T>`형식의 여러 구성 요소 매개 변수를 정의할 수 있습니다. `RenderFragment<T>`에 대 한 매개 변수는 호출 될 때 지정할 수 있습니다. 구성 요소에 대 한 제네릭 형식 매개 변수를 지정 하려면 `@typeparam` Razor 지시문을 사용 합니다.

*SimpleListView*

```razor
@typeparam TItem

@Heading

<ul>
@foreach (var item in Items)
{
    <li>@ItemTemplate(item)</li>
}
</ul>

@code {
    [Parameter]
    public RenderFragment Heading { get; set; }

    [Parameter]
    public RenderFragment<TItem> ItemTemplate { get; set; }

    [Parameter]
    public IEnumerable<TItem> Items { get; set; }
}
```

템플릿 기반 구성 요소를 사용 하는 경우 매개 변수의 이름과 일치 하는 자식 요소를 사용 하 여 템플릿 매개 변수를 지정할 수 있습니다. 요소로 전달 되는 `RenderFragment<T>` 형식의 구성 요소 인수에는 `context`라는 암시적 매개 변수가 있습니다. 자식 요소의 `Context` 특성을 사용 하 여이 구현 매개 변수의 이름을 변경할 수 있습니다. 형식 매개 변수의 이름과 일치 하는 특성을 사용 하 여 제네릭 형식 매개 변수를 지정할 수 있습니다. 가능 하면 형식 매개 변수가 유추 됩니다.

```razor
<SimpleListView Items="messages" TItem="string">
    <Heading>
        <h1>My list</h1>
    </Heading>
    <ItemTemplate Content="message">
        <p>The message is: @message</p>
    </ItemTemplate>
</SimpleListView>
```

이 구성 요소의 출력은 다음과 같습니다.

```html
<h1>My list</h1>
<ul>
    <li>The message is: message1</li>
    <li>The message is: message2</li>
<ul>
```

## <a name="code-behind"></a>코드 숨김

Blazor 구성 요소는 일반적으로 단일 *razor* 파일로 작성 됩니다. 그러나 코드를 사용 하 여 코드와 태그를 분리 하는 것도 가능 합니다. 구성 요소 파일을 사용 하려면 구성 요소 C# 파일의 파일 이름과 일치 하지만 *.cs* 확장명이 추가 된 파일 (*Counter.razor.cs*)을 추가 합니다. 이 C# 파일을 사용 하 여 구성 요소에 대 한 기본 클래스를 정의 합니다. 기본 클래스의 이름을 원하는 대로 지정할 수 있지만 클래스 이름을 component 클래스와 동일 하 게 하는 것이 일반적 이지만 `Base` 확장 (`CounterBase`)이 추가 됩니다. 구성 요소 기반 클래스는 `ComponentBase`에서도 파생 되어야 합니다. 그런 다음 Razor 구성 요소 파일에서 `@inherits` 지시어를 추가 하 여 구성 요소 (`@inherits CounterBase`)에 대 한 기본 클래스를 지정 합니다.

*카운터. razor*

```razor
@inherits CounterBase

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button @onclick="IncrementCount">Click me</button>
```

*Counter.razor.cs*

```csharp
public class CounterBase : ComponentBase
{
    protected int currentCount = 0;

    protected void IncrementCount()
    {
        currentCount++;
    }
}
```

기본 클래스에서 구성 요소의 멤버에 대 한 표시 여부를 `protected` 하거나 구성 요소 클래스에 표시 되도록 `public` 해야 합니다.

## <a name="additional-resources"></a>추가 자료

위의 Blazor 구성 요소에 대 한 모든 측면을 철저 하 게 처리 하지는 않습니다. [Razor 구성 요소 ASP.NET Core 만들고 사용](/aspnet/core/blazor/components)하는 방법에 대 한 자세한 내용은 Blazor 설명서를 참조 하세요.

>[!div class="step-by-step"]
>[이전](app-startup.md)
>[다음](pages-routing-layouts.md)
