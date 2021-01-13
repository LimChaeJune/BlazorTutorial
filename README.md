블레이저 듀토리얼
================
## 1. Blazor란?
Blazor is a free and open-source web framework that enables developers to create web apps using C# and HTML. It is being developed by Microsoft.
- ### 개요
  Blazor는 HTML, CSS를 이용해 웹 UI를 구성하고, Javascript대신 C#을 이용한 동적 웹 UI를 빌드할 수 있는 .NET Framework에서 제공하는 ASP.NET Core을 사용하는 도구와 라이브러리가 포함된 웹 프레임워크입니다. 그리고 HTML과 C#의 결합으로 사용자 입력을 바인딩하고, UI업데이터를 더욱 효율적으로 렌더링할 수 있습니다.   

  *블레이저는 오픈 소스이며 [여기](https://github.com/dotnet/aspnetcore/tree/master/src/Components)에서 사용할 수 있습니다.*
## 2. Blazor의 HostingModel
Blazor에는 두 가지의 호스팅 모델이 있습니다. 
- ### 2.1 Blazor Web Assembly - WASM
  * WASM는 클라이언트의 브라우저에서 실행되어 서버에 대한 네트워크 연결이 끊어져도 클라이언트 앱은 오프라인에서도 계속 작동할 수 있습니다.  클라이언트의 컴퓨터에서 실행되는 코드를 사용하면서 서버로드가 크게 줄어 듭니다.
  * WASM은 데스크톱 및 모바일의 모든 최신 웹 브라우저에서 작동합니다.
  * WASM은 보안 샌드박스 내 사용자 장치에서 실행됩니다.
  * WASM은 서버 구성 요소없이 정적 사이트로 배포가 가능합니다. (API 제외)
  * blazor.webassembly.js 필요한 모든 .NET DLL 어셈블리를 다운로드하므로 응용 프로그램 시작 시간이 서버 쪽보다 느려집니다.
- ### 2.2 Blazor Server
  * Server측은 HTML소스가 클라이언트의 브라우저로 전송되기 전에 미리 렌더링되기 때문에 WASM보다 빠른 로딩이 가능합니다.
  * C# 코드가 브라우저로 전송되지 않고, 서버측에서 실행되기 때문에 Visual Studio를 통한 .NET 코드 디버깅이 제한이 적습니다.
  * WASM은 최신 브라우저에서만 작동하기 때문에 IE 11과 같은 브라우저를 지원하지 않지만, Server측은 지원합니다.
  * Server측은 현재 클라이언트에 대한 메모리 내 세션을 설정하고 SignalR을 사용하여 서버에서 실행되는 .NET과 클라이언트의 브라우저 간 통신하기 때문에 모든 메모리 및 CPU 사용량은 모든 사용자에 대한 서버 비용으로 발생합니다.   
- ### 2.3 Signal R
  Blazor을 알아가면서 앞으로 SignalR이란 이름을 자주 마주하게 될 것 입니다.   
그래서 먼저 설명을 하자면 SignalR은 응용 프로그램에 실시간 웹 기능을 추가하는 프로세스를 단순화하는 ASP.NET 개발자 용 라이브러리입니다.   
실시간 웹 기능이란 서버 측의 코드가 실시간으로 콘테츠를 사용할 수 있게되는 즉시 연결된 클라이언트의 푸시되도록 하는 기능입니다.   
이 오픈소스는 Blazor를 알아보는 것이 목적이니 이정도로 간단히 설명하고 넘어가겠습니다.   
SignalR 또한 오픈소스이며 [MSDN](https://docs.microsoft.com/en-us/aspnet/signalr/overview/getting-started/introduction-to-signalr)과 [GitHub](https://github.com/dotnet/aspnetcore/tree/master/src/SignalR)를 통해 더욱 자세한 정보를 알아볼 수 있습니다.
- - -
> **Blazor Server와 Blazor WASM은 모두 장단점이 있기 때문에 개발 목적에 맞게 선택하면 됩니다.**   
**많은 수의 사용자가 사용하며, 민감한 코드가 없어 숨기지 않는 경우 - WASM   
성능이 중요하며, 브라우저에서 실행하지않고 싶은 민감한 코드가 있는 경우 - Server**
## 3. Blazor 시작하기
* Blazor 필수사항
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
TBD...   
> 위의 단계는 Blazor Server 프로젝트로 진행됩니다. (추후에 WASM 추가할 예정입니다.)
- ### 3.1 Blazor 필수사항
  이 글은 Windows10 Visual Studio 2019를 기반으로 작성되는 글입니다.
  * .NET Core 2.1 SDK 또는 이후 버전 [확인](https://dotnet.microsoft.com/download/dotnet-core)
  * Visual Studio 15.7 또는 이후 버전 [확인](https://visualstudio.microsoft.com/)
- ### 3.2. Blazor 프로젝트 구조
  Blazor Server은 기본적으로 ASP.NET CORE의 구조와 동일합니다.
   
  - #### 3.2.1 Startup.cs
    앱의 시작 논리를 포함하는 클래스입니다. StartUp 클래스는 생성 시에 두 가지 메서드를 정의합니다.    
    * ConfigureServices: 앱의 DI(Depedency Inject) 서비스를 구성합니다. (ex: DBContenxt, Authorize, API, Service 등) Blazor Server에서는 AddServerSideBlazor 메소드를 호출한 뒤(AddSigleton , AddScoped, AddTransient)등의 방법으로 Service가 추가하면 앱의 Service 컨테이너에 추가되면서 Component에서 Service를 Inject하여 사용할 수 있습니다.   
   
    * Configure: 앱의 요청 처리 파이프 라인을 구성합니다.   
   
      초기 StartUp.cs의 소스는 아래와 같습니다.          
      `Startup.cs`   
      ```csharp
      public class Startup
      {
          public Startup(IConfiguration configuration)
          {
              Configuration = configuration;
          }

          public IConfiguration Configuration { get; }

          // This method gets called by the runtime. Use this method to add services to the container.
          // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
          public void ConfigureServices(IServiceCollection services)
          {
              services.AddRazorPages();
              services.AddServerSideBlazor();
              // 앱의 Dependency Inject 종속성 주입 -- 이 개념은 아래에 설명됩니다.
              services.AddSingleton<WeatherForecastService>();
          }

          // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
          public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
          {
              if (env.IsDevelopment())
              {
                  app.UseDeveloperExceptionPage();
              }
              else
              {
                  // 앱에서 오류가 발생됐을 때 화면
                  app.UseExceptionHandler("/Error");
                  // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                  app.UseHsts();
              }

              app.UseHttpsRedirection();
              app.UseStaticFiles();

              app.UseRouting();

              app.UseEndpoints(endpoints =>
              {
                  // 브라우저와의 실시간 연결을 위한 엔드 포인트를 설정하기 위해 호출됩니다. 연결은 SignalR로 생성됩니다.
                  endpoints.MapBlazorHub();
                  // 앱의 루트 페이지를 설정하고 탐색을 활성화합니다. 
                  endpoints.MapFallbackToPage("/_Host");
              });
          }
      }
      ```
  - #### 3.2.2 Program.cs
  
    프로젝트에 대한 진입점인 메소드를 포함한 클래스입니다. ASP.NET Core를 호스트합니다.
    
  - #### 3.2.3 Pages폴더   
  
    Blazor 앱을 구성하는 라우팅 가능한 Razor 페이지와 Blazor 서버 앱 루트 Razor 페이지가 포함됩니다.
    
    각 페이지의 경로는 @page 지시문을 이용해 지정할 수 있습니다. 사용법은 아래와 같습니다. 
    ```
    @page "/경로이름"
    ```
    Blazor Server 프로젝트를 생성했을 때 Pages의 폴더의 템플릿은 다음과 같습니다.
    
    * _Host.cshtml: Razor 페이지로 구현된 앱의 루트 페이지 입니다
      + 앱의 페이지가 처음 요청되면 이 페이지가 렌더링되며 응답합니다.
      + 루트 App의 구성 요소가 렌더링 되는 위치를 지정합니다.
      + 기본적으론 `_framework/blazor.server.js` Javascript 파일이 로드되며 브라우저와 서버 간의 실시가 SignalR 연결을 설정하는 JS입니다.
      + 새로 생성한 js혹은 css, Api 용도의 js 등을 추가할 수 있습니다.
    * Counter.razor: Blazor 구성요소 Razor 페이지를 통해 카운트 증가가 구현된 페이지입니다.
    * Error.cshtml: App에서 처리되지 않은 예외가 발생할 때 렌더링 됩니다.
    * FetchData.razor: StartUp.cs에서 DI로 등록된 Service를 통해 데이터를 가져오는 것이 구현된 페이지입니다.
    * Index.razor: 홈 페이지를 구현한 페이지입니다. 여느 다른 웹 개발 Index 페이지와 같습니다.
    
  - #### 3.2.4 Properties/launchSettings.json
    
    개발 환경 구성을 가진 파일입니다. 
    
    * [ASP.NET Core의 launchSettings.json](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/environments?view=aspnetcore-5.0#development-and-launchsettingsjson) 파일과 같으며 시스템 환경에 설정된 값을 재정의할 수 있습니다.
    * 로컬 개발 머신에서만 사용됩니다.
    * 배포되지 않습니다.
    * 여러가지 프로필 설정이 포함되어 있습니다.
    
  - #### 3.2.5 Shared폴더
    
    razor페이지에서 사용하는 공통 UI 구성 요소를 포함합니다.
    
    * MainLayout.razor: 앱의 레이아웃 구성 요소 입니다. 아래에 자세한 설명이있습니다.
    * NavMenu.razor: 사이드 바 Navigation을 구현합니다. 다른 Razor 쉉요소에 대한 탐색 링크를 렌더링 하는 NavLink를 포함합니다. [ASP.NET Core의 NavLink](https://docs.microsoft.com/en-us/aspnet/core/blazor/fundamentals/routing?view=aspnetcore-5.0#navlink-component)와 같으며 사용자가 표시된 탐색 링크 중 어떤 페이지가 활성 페이지인지알 수 있으며, NavLink.ActiveClass에 CSS 클래스의 이름을 할당하여 데이터와 비교해 동적으로 변경할 수 있습니다.
    
  - #### 3.2.6 __Import.razor
  
    .razor와 같은 앱의 구성요소에 포함할 공통 네임스페이스에 대한 `@Using` 지시문을 포함합니다.
    
  - #### 3.2.7 Data 폴더
  
    일반적으로는 Database의 관리를 위한 폴더입니다.
    Blazor Server의 기본 구성요소는 아래와 같습니다.
    
    * WeatherForecast.cs: Property 구성요소를 담은 클래스
    * WeatherForecastService.cs: Data를 가져오는 Task 메소드를 담은 클래스
    
  - #### 3.2.8 wwwroot    
    앱의 공개 정적 요소들이 포함된 [Web root](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/?view=aspnetcore-5.0&tabs=windows#web-root)입니다.
    
  - #### 3.2.9 appsetting.json
    앱의 구성 설정을 저장한 파일입니다.
    
    * IConfiguration 인스턴스를 Razor 구성 요소에 삽입해 구성 데이터를 액세스할 수 있습니다.
    * API, Authorize, DataBase ConnectionString 등의 인증을 구성합니다.    
- ### 3.3. Blazor Layout
    블레이저로 프로젝트를 처음 생성하면 Shared파일의 MainLayout.razor에서 소스코드를 변경함으로서 Page의 Layout Template을 작성할 수 있습니다.   
    Blazor Layout의 모든 콘텐츠는 LayoutComponent Class의 하위 항목 쓰입니다.  
    Blazor Layout은 Index.razor 페이지 내에서 정의 된 부분만 작동 됩니다. (Blazor Server만 포함)
    
    `MainLayout.razor`   
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
- ### 3.4. Blazor Component
    Component는 C# HTML 태그 조합으로 Blazor Razor 구성 요소 파일로 구현하는 것 입니다.
    - #### 3.4.1 개요
        - ##### Razor 구문
          Blazor의 Razor 구성 요소는 Razor 구문을 사용합니다.
          Razor 구문에 익숙하지 않다면 [MSDN Razor 구문 참조](https://docs.microsoft.com/ko-kr/aspnet/core/mvc/views/razor?view=aspnetcore-5.0)을 참고하는게 좋습니다.

        - ##### 작명 
          Component의 이름은 대문자로 시작 해야됩니다. ex) BlazorComponent.razor

        - ##### Routing
          Blazor의 라우팅은 앱에서 액세스 할 수 있는 각 구성 요소에 경로 템플릿을 제공하여 수행됩니다.  
          [@page](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/razor?view=aspnetcore-5.0#page) 지시문이 있는 Razor 파일이 컴파일되면 생성 된 클래스에 경로 템플릿을 지정하는 [RouteAttribute](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.routeattribute?view=aspnetcore-5.0) 가 제공됩니다. 
          런타임시 라우터는 [RouteAttribute](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.routeattribute?view=aspnetcore-5.0)가 있는 Component의 클래스를 찾고 요청 된 URL과 일치하는 경로의 Component를 렌더링합니다.              
          `BlazorComponent.razor`
          ```razor
          @page "/BlazorComponent"        
          ```

        - ##### Markup(HTML)
          Component의 UI는 HTML을 사용하여 정의됩니다.
          Razor구문을 사용하기 때문에 C# 구문을 이용한 동적 렌더링 논리를 구성할 수 있습니다.
          앱이 컴파일되면 HTML 태그와 C# 렌더링 논리가 Component의 클래스로 변환됩니다.

          Component 클래스의 구성은 [@code](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/razor?view=aspnetcore-5.0#code) 블록 내에서 정의됩니다. 
          [@code](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/razor?view=aspnetcore-5.0#code) 블록에서는 Property, Fields 등과 그것들을 처리하는 Event와 Method들을 포함합니다.
          
          다음은 동적 렌더링 예 입니다.
          `DynamicRender`
          ```html
          <div class="@(visibllity ? "visible" : "collapse")">
            @text1
          </div>

          @code{
            private bool visibllity { get; set;} = false;
            private string text1 { get; set; } = "my sample text"
          }
          ```


      

      
    
    
 
