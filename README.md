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
- - -
**Blazor Server와 Blazor WASM은 모두 장단점이 있기 때문에 개발 목적에 맞게 선택하면 됩니다.**   
**많은 수의 사용자가 사용하며, 민감한 코드가 있어 숨기지 않는 경우 - WASM   
성능이 중요하며, 브라우저에서 실행하지않고 싶은 민감한 코드가 있는 경우 - Server**
# 3. Blazor 시작하기
이 글은 Windows10 Visual Studio 2019를 기반으로 작성되는 글입니다.
* [.NET Core 2.1 SDK 또는 이후 버전](https://dotnet.microsoft.com/download/dotnet-core)
* [Visual Studio 15.7 또는 이후 버전](https://visualstudio.microsoft.com/)
# 4. Application 생성
