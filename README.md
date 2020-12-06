블레이저 듀토리얼
================
# 1. Blazor란?
Blazor는 HTML, CSS를 이용해 웹 UI를 구성하고, Javascript대신 C#을 이용한 동적 웹 UI를 빌드할 수 있는 .NET Framework에서 제공하는 ASP.NET Core을 사용하는 도구와 라이브러리가 포함된 웹 프레임워크입니다.   
HTML과 C#의 결합으로 사용자 입력을 바인딩하고, UI업데이터를 더욱 효율적으로 렌더링할 수 있습니다.   
**블레이저는 오픈 소스이며 [여기](https://github.com/dotnet/aspnetcore/tree/master/src/Components)에서 사용할 수 있습니다.**
# 2. Blazor의 HostingModel
Blazor에는 두 가지의 호스팅 모델이 있습니다. 
## 2.1 Blazor Web Assembly - WASM
* WASM는 클라이언트의 브라우저에서 실행되어 서버에 대한 네트워크 연결이 끊어져도 클라이언트 앱은 오프라인에서도 계속 작동할 수 있습니다.  클라이언트의 컴퓨터에서 실행되는 코드를 사용하면서 서버로드가 크게 줄어 듭니다.
* WASM은 데스크톱 및 모바일의 모든 최신 웹 브라우저에서 작동합니다.
* WASM은 보안 샌드박스 내 사용자 장치에서 실행됩니다.
* WASM은 서버 구성 요소없이 정적 사이트로 배포가 가능합니다. (API 제외)
* blazor.webassembly.js 필요한 모든 .NET DLL 어셈블리를 다운로드하므로 응용 프로그램 시작 시간이 서버 쪽보다 느려집니다.
## 2.2 Blazor Server
* Server측은 HTML소스가 클라이언트의 브라우저로 전송되기 전에 미리 렌더링되기 때문에 WASM보다 빠른 로딩이 가능합니다.
* C# 코드가 브라우저로 전송되지 않고, 서버측에서 실행되기 때문에 Visual Studio를 통한 .NET 코드 디버깅이 제한이 적습니다.
* WASM은 최신 브라우저에서만 작동하기 때문에 IE 11과 같은 브라우저를 지원하지 않지만, Server측은 지원합니다.
* Server측은 현재 클라이언트에 대한 메모리 내 세션을 설정하고 SignalR을 사용하여 서버에서 실행되는 .NET과 클라이언트의 브라우저 간 통신하기 때문에 모든 메모리 및 CPU 사용량은 모든 사용자에 대한 서버 비용으로 발생합니다.   
## 2.3 Signal R
Blazor을 알아가면서 앞으로 SignalR이란 이름을 자주 마주하게 될 것 입니다.   
그래서 먼저 설명을 하자면 SignalR은 응용 프로그램에 실시간 웹 기능을 추가하는 프로세스를 단순화하는 ASP.NET 개발자 용 라이브러리입니다.   
실시간 웹 기능이란 서버 측의 코드가 실시간으로 콘테츠를 사용할 수 있게되는 즉시 연결된 클라이언트의 푸시되도록 하는 기능입니다.   
이 오픈소스는 Blazor를 알아보는 것이 목적이니 이정도로 간단히 설명하고 넘어가겠습니다.   
SignalR 또한 오픈소스이며 [MSDN](https://docs.microsoft.com/en-us/aspnet/signalr/overview/getting-started/introduction-to-signalr)과 [GitHub](https://github.com/dotnet/aspnetcore/tree/master/src/SignalR)를 통해 더욱 자세한 정보를 알아볼 수 있습니다.
- - -
**Blazor Server와 Blazor WASM은 모두 장단점이 있기 때문에 개발 목적에 맞게 선택하면 됩니다.**   
**많은 수의 사용자가 사용하며, 민감한 코드가 있어 숨기지 않는 경우 - WASM   
성능이 중요하며, 브라우저에서 실행하지않고 싶은 민감한 코드가 있는 경우 - Server**
# 3. Blazor 시작하기
* Blazor 필수사항
* Aplication 생성   
* Blazor 프로젝트 구조
* Blazor Layout   
* Blazor Components     
* Blazor Binding   
* Blazor Parameter   
* Blazor Routing   
* Blazor JavaScript   
* Blazor Dependency Inject   
* Blazor Entity Framework   
* Blazor Authorize   
---
**위의 단계는 Blazor Server 프로젝트로 진행됩니다. (추후에 WASM 추가할 예정입니다.)**
## 3.1 Blazor 필수사항
이 글은 Windows10 Visual Studio 2019를 기반으로 작성되는 글입니다.
* [.NET Core 2.1 SDK 또는 이후 버전](https://dotnet.microsoft.com/download/dotnet-core)
* [Visual Studio 15.7 또는 이후 버전](https://visualstudio.microsoft.com/)
## 3.2. Application 생성
먼저 아래 사진처럼 블레이저를 선택합니다.
![start](https://user-images.githubusercontent.com/30399915/101181964-da43d080-3690-11eb-9d89-c6aa5370df5a.png)
여기서 Server로 생성할 것인지 WASM을 생성할건지 선택할 수 있습니다.
![server](https://user-images.githubusercontent.com/30399915/101181969-dadc6700-3690-11eb-9421-0b52eccea163.png)
프로젝트를 생성하고 처음 실행을 했을 때 페이지 화면입니다.
![builde](https://user-images.githubusercontent.com/30399915/101181972-db74fd80-3690-11eb-8688-c93f0ba2f6c1.png)

## 3.3. Blazor 프로젝트 구조
Blazor Server은 기본적으로 ASP.NET CORE의 구조를 따라갑니다.


## 3.4. Blazor Layout
블레이저로 프로젝트를 처음 생성하면 Shared파일의 MainLayout.razor에서 소스코드를 변경함으로서 Page의 Layout Template을 작성할 수 있습니다.
Blazor Layout의 모든 콘텐츠는 LayoutComponent Class의 하위 항목 쓰입니다.  
Blazor Layout은 Index.razor 페이지 내에서 정의 된 부분만 작동 됩니다.    
**Index.razor 페이지의 경로 Server(Pages\Index.razor), WASM(wwwroot\index.html)**   

---
MainLayout.razor   
```XML
      @inherits LayoutComponentBase
      <div class="sidebar">
          <NavMenu />
      </div>

      <div class="main">
          <div class="top-row px-4">
              <a href="https://docs.microsoft.com/aspnet/" target="_blank">About</a>
          </div>

          <div class="content px-4">
              @Body  //이곳의 Index.razor내의 정의된 HTML들이 들어갑니다.
          </div>
      </div>
```

