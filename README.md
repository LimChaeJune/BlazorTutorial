블레이저 듀토리얼
================
## 1. Blazor란?
- ### 소개 
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
**많은 수의 사용자가 사용하며, 민감한 코드가 있어 숨기지 않는 경우 - WASM   
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
              // 앱의 Dependency Inject 종속성 주입 이 개념은 아래에 설명됩니다.
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

  - ### 3.3. Blazor Layout
    블레이저로 프로젝트를 처음 생성하면 Shared파일의 MainLayout.razor에서 소스코드를 변경함으로서 Page의 Layout Template을 작성할 수 있습니다.   
    Blazor Layout의 모든 콘텐츠는 LayoutComponent Class의 하위 항목 쓰입니다.  
    Blazor Layout은 Index.razor 페이지 내에서 정의 된 부분만 작동 됩니다.    

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

