---
<span data-ttu-id="73a55-101">Title: "ASP.NET Core Blazor рекомендации по повышению производительности веб сборки" Автор: описание: "советы по повышению производительности в ASP.NET Core приложениях веб – Blazor сборки и устранении распространенных проблем с производительностью".</span><span class="sxs-lookup"><span data-stu-id="73a55-101">title: 'ASP.NET Core Blazor WebAssembly performance best practices' author: description: 'Tips for increasing performance in ASP.NET Core Blazor WebAssembly apps and avoiding common performance problems.'</span></span>
<span data-ttu-id="73a55-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span><span class="sxs-lookup"><span data-stu-id="73a55-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="73a55-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="73a55-103">'Blazor'</span></span>
- <span data-ttu-id="73a55-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="73a55-104">'Identity'</span></span>
- <span data-ttu-id="73a55-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="73a55-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="73a55-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="73a55-106">'Razor'</span></span>
- <span data-ttu-id="73a55-107">ИД пользователя "SignalR":</span><span class="sxs-lookup"><span data-stu-id="73a55-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-webassembly-performance-best-practices"></a><span data-ttu-id="73a55-108">Рекомендации по производительности ASP.NET Coreной Blazor сборки</span><span class="sxs-lookup"><span data-stu-id="73a55-108">ASP.NET Core Blazor WebAssembly performance best practices</span></span>

<span data-ttu-id="73a55-109">По [получения кришнамурси](https://github.com/pranavkm)</span><span class="sxs-lookup"><span data-stu-id="73a55-109">By [Pranav Krishnamoorthy](https://github.com/pranavkm)</span></span>

<span data-ttu-id="73a55-110">В этой статье приводятся рекомендации по ASP.NET Core Blazor рекомендациям по производительности сборки.</span><span class="sxs-lookup"><span data-stu-id="73a55-110">This article provides guidelines for ASP.NET Core Blazor WebAssembly performance best practices.</span></span>

## <a name="avoid-unnecessary-component-renders"></a><span data-ttu-id="73a55-111">Избегайте визуализации ненужных компонентов</span><span class="sxs-lookup"><span data-stu-id="73a55-111">Avoid unnecessary component renders</span></span>

Blazor<span data-ttu-id="73a55-112">позволяет избежать повторного отображения компонента, когда алгоритм воспринимает, что компонент не изменился.</span><span class="sxs-lookup"><span data-stu-id="73a55-112">'s diffing algorithm avoids rerendering a component when the algorithm perceives that the component hasn't changed.</span></span> <span data-ttu-id="73a55-113">Переопределение <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A?displayProperty=nameWithType> для точного управления отрисовкой компонентов.</span><span class="sxs-lookup"><span data-stu-id="73a55-113">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A?displayProperty=nameWithType> for fine-grained control over component rendering.</span></span>

<span data-ttu-id="73a55-114">При создании компонента только пользовательского интерфейса, который никогда не изменяется после первоначального рендеринга, настройте <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> для возврата `false` :</span><span class="sxs-lookup"><span data-stu-id="73a55-114">If authoring a UI-only component that never changes after the initial render, configure <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> to return `false`:</span></span>

```razor
@code {
    protected override bool ShouldRender() => false;
}
```

<span data-ttu-id="73a55-115">Большинству приложений не требуется детализированный контроль, но их <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> также можно использовать для выборочного отображения компонента, отвечающего на событие пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="73a55-115">Most apps don't require fine-grained control, but <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> can also be used to selectively render a component responding to a UI event.</span></span>

<span data-ttu-id="73a55-116">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="73a55-116">In the following example:</span></span>

* <span data-ttu-id="73a55-117"><xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A>переопределяется и задается в качестве значения <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> поля, которое изначально `false` загружается при загрузке компонента.</span><span class="sxs-lookup"><span data-stu-id="73a55-117"><xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> is overridden and set to the value of the <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> field, which is initially `false` when the component loads.</span></span>
* <span data-ttu-id="73a55-118">Если кнопка выбрана, <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> для параметра задается значение `true` , которое заставляет компонент перерисовываться с обновленным `currentCount` .</span><span class="sxs-lookup"><span data-stu-id="73a55-118">When the button is selected, <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> is set to `true`, which forces the component to rerender with the updated `currentCount`.</span></span>
* <span data-ttu-id="73a55-119">Сразу после <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> повторной визуализации устанавливает значение <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> обратно, `false` чтобы предотвратить дальнейшее повторное отображение до тех пор, пока не будет выбрана следующая кнопка.</span><span class="sxs-lookup"><span data-stu-id="73a55-119">Immediately after rerendering, <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> sets the value of <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> back to `false` to prevent further rerendering until the next time the button is selected.</span></span>

```razor
<p>Current count: @currentCount</p>

<button @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;
    private bool shouldRender;

    protected override bool ShouldRender() => shouldRender;

    protected override void OnAfterRender(bool first)
    {
        shouldRender = false;
    }

    private void IncrementCount()
    {
        currentCount++;
        shouldRender = true;
    }
}
```

<span data-ttu-id="73a55-120">Для получения дополнительной информации см. <xref:blazor/lifecycle#after-component-render>.</span><span class="sxs-lookup"><span data-stu-id="73a55-120">For more information, see <xref:blazor/lifecycle#after-component-render>.</span></span>

## <a name="virtualize-re-usable-fragments"></a><span data-ttu-id="73a55-121">Виртуализация повторно используемых фрагментов</span><span class="sxs-lookup"><span data-stu-id="73a55-121">Virtualize re-usable fragments</span></span>

<span data-ttu-id="73a55-122">Компоненты предлагают удобный подход к созданию многократно используемых фрагментов кода и разметки.</span><span class="sxs-lookup"><span data-stu-id="73a55-122">Components offer a convenient approach to produce re-usable fragments of code and markup.</span></span> <span data-ttu-id="73a55-123">Как правило, рекомендуется создавать отдельные компоненты, которые наилучшим образом согласовываются с требованиями приложения.</span><span class="sxs-lookup"><span data-stu-id="73a55-123">In general, we recommend authoring individual components that best align with the app's requirements.</span></span> <span data-ttu-id="73a55-124">Одно из предостережений заключается в том, что каждый дополнительный дочерний компонент вносит вклад в общее время, необходимое для визуализации родительского компонента.</span><span class="sxs-lookup"><span data-stu-id="73a55-124">One caveat is that each additional child component contributes to the total time it takes to render a parent component.</span></span> <span data-ttu-id="73a55-125">Для большинства приложений дополнительные издержки незначительны.</span><span class="sxs-lookup"><span data-stu-id="73a55-125">For most apps, the additional overhead is negligible.</span></span> <span data-ttu-id="73a55-126">Приложения, которые создают большое количество компонентов, должны использовать стратегии для снижения затрат на обработку, например для ограничения количества готовых к просмотру компонентов.</span><span class="sxs-lookup"><span data-stu-id="73a55-126">Apps that produce a large number of components should consider using strategies to reduce processing overhead, such as limiting the number of rendered components.</span></span>

<span data-ttu-id="73a55-127">Например, сетка или список, которые отображают сотни строк, содержащих компоненты, являются ресурсоемкими для визуализации.</span><span class="sxs-lookup"><span data-stu-id="73a55-127">For example, a grid or list that renders hundreds of rows containing components is processor intensive to render.</span></span> <span data-ttu-id="73a55-128">Рассмотрите возможность виртуализации макета сетки или списка, чтобы подмножество компонентов отображалось в любое заданное время.</span><span class="sxs-lookup"><span data-stu-id="73a55-128">Consider virtualizing a grid or list layout so that only a subset of the components is rendered at any given time.</span></span> <span data-ttu-id="73a55-129">Пример подготовки к просмотру подмножества компонентов см. в следующих компонентах [примера приложения виртуализации (в репозитории ASPNET/Samples GitHub)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Virtualization):</span><span class="sxs-lookup"><span data-stu-id="73a55-129">For an example of component subset rendering, see the following components in the [Virtualization sample app (aspnet/samples GitHub repository)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Virtualization):</span></span>

* <span data-ttu-id="73a55-130">`Virtualize`Component ([Shared/виртуализировать. Razor](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Shared/Virtualize.cs)): компонент, написанный на языке C#, реализующий <xref:Microsoft.AspNetCore.Components.ComponentBase> для визуализации набора строк данных погоды на основе прокрутки пользователя.</span><span class="sxs-lookup"><span data-stu-id="73a55-130">`Virtualize` component ([Shared/Virtualize.razor](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Shared/Virtualize.cs)): A component written in C# that implements <xref:Microsoft.AspNetCore.Components.ComponentBase> to render a set of weather data rows based on user scrolling.</span></span>
* <span data-ttu-id="73a55-131">`FetchData`Component ([pages/FetchData. Razor](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Pages/FetchData.razor)): использует `Virtualize` компонент для вывода 25 строк данных о погоде за раз.</span><span class="sxs-lookup"><span data-stu-id="73a55-131">`FetchData` component ([Pages/FetchData.razor](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Pages/FetchData.razor)): Uses the `Virtualize` component to display 25 rows of weather data at a time.</span></span>

## <a name="avoid-javascript-interop-to-marshal-data"></a><span data-ttu-id="73a55-132">Избегайте взаимодействия JavaScript для маршалирования данных</span><span class="sxs-lookup"><span data-stu-id="73a55-132">Avoid JavaScript interop to marshal data</span></span>

<span data-ttu-id="73a55-133">В Blazor этой сборке вызов взаимодействия JavaScript (JS) должен пройти через границу Assembly-JS.</span><span class="sxs-lookup"><span data-stu-id="73a55-133">In Blazor WebAssembly, a JavaScript (JS) interop call must traverse the WebAssembly-JS boundary.</span></span> <span data-ttu-id="73a55-134">Сериализация и десериализация содержимого в двух контекстах создает дополнительную нагрузку на обработку приложения.</span><span class="sxs-lookup"><span data-stu-id="73a55-134">Serializing and deserializing content across the two contexts creates processing overhead for the app.</span></span> <span data-ttu-id="73a55-135">Частые вызовы взаимодействия JS часто негативно влияют на производительность.</span><span class="sxs-lookup"><span data-stu-id="73a55-135">Frequent JS interop calls often adversely affects performance.</span></span> <span data-ttu-id="73a55-136">Чтобы уменьшить объем маршалинга данных по границе, определите, может ли приложение консолидировать большое количество небольших полезных данных в одну большую полезную нагрузку, чтобы избежать большого объема переключения контекста между сборками и JS.</span><span class="sxs-lookup"><span data-stu-id="73a55-136">To reduce the marshalling of data across the boundary, determine if the app can consolidate many small payloads into a single large payload to avoid the high volume of context switching between WebAssembly and JS.</span></span>

## <a name="use-systemtextjson"></a><span data-ttu-id="73a55-137">Использование System. Text. JSON</span><span class="sxs-lookup"><span data-stu-id="73a55-137">Use System.Text.Json</span></span>

Blazor<span data-ttu-id="73a55-138">Реализация взаимодействия JS основывается на <xref:System.Text.Json> высокопроизводительной библиотеке СЕРИАЛИЗАЦИИ JSON с нехваткой выделения памяти.</span><span class="sxs-lookup"><span data-stu-id="73a55-138">'s JS interop implementation relies on <xref:System.Text.Json>, which is a high-performance JSON serialization library with low memory allocation.</span></span> <span data-ttu-id="73a55-139">Использование <xref:System.Text.Json> не приводит к дополнительным размерам полезных данных приложения, чем добавление одной или нескольких альтернативных библиотек JSON.</span><span class="sxs-lookup"><span data-stu-id="73a55-139">Using <xref:System.Text.Json> doesn't result in additional app payload size over adding one or more alternate JSON libraries.</span></span>

<span data-ttu-id="73a55-140">Инструкции по миграции см. в статье [Миграция из Newtonsoft. JSON в System. Text. JSON](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to).</span><span class="sxs-lookup"><span data-stu-id="73a55-140">For migration guidance, see [How to migrate from Newtonsoft.Json to System.Text.Json](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to).</span></span>

## <a name="use-synchronous-and-unmarshalled-js-interop-apis-where-appropriate"></a><span data-ttu-id="73a55-141">Используйте синхронные и неупакованные API взаимодействия JS, если это уместно</span><span class="sxs-lookup"><span data-stu-id="73a55-141">Use synchronous and unmarshalled JS interop APIs where appropriate</span></span>

Blazor<span data-ttu-id="73a55-142">Веб – сборка предлагает две дополнительные версии <xref:Microsoft.JSInterop.IJSRuntime> по сравнению с одной версией, доступной для Blazor серверных приложений:</span><span class="sxs-lookup"><span data-stu-id="73a55-142"> WebAssembly offers two additional versions of <xref:Microsoft.JSInterop.IJSRuntime> over the single version available to Blazor Server apps:</span></span>

* <span data-ttu-id="73a55-143"><xref:Microsoft.JSInterop.IJSInProcessRuntime>позволяет синхронно вызывать вызовы взаимодействия JS, что требует меньше ресурсов, чем асинхронные версии:</span><span class="sxs-lookup"><span data-stu-id="73a55-143"><xref:Microsoft.JSInterop.IJSInProcessRuntime> allows invoking JS interop calls synchronously, which has less overhead than the asynchronous versions:</span></span>

  ```razor
  @inject IJSRuntime JS

  @code {
      protected override void OnInitialized()
      {
          var jsInProcess = (IJSInProcessRuntime)JS;

          var value = jsInProcess.Invoke<string>("jsInteropCall");
      }
  }
  ```

* <span data-ttu-id="73a55-144"><xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime>разрешает неупакованные вызовы взаимодействия JS:</span><span class="sxs-lookup"><span data-stu-id="73a55-144"><xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> permits unmarshalled JS interop calls:</span></span>

  ```javascript
  function jsInteropCall() {
    return BINDING.js_to_mono_obj("Hello world");
  }
  ```

  ```razor
  @inject IJSRuntime JS

  @code {
      protected override void OnInitialized()
      {
          var jsInProcess = (WebAssemblyJSRuntime)JS;

          var value = jsInProcess.InvokeUnmarshalled<string>("jsInteropCall");
      }
  }
  ```

  > [!WARNING]
  > <span data-ttu-id="73a55-145">Хотя использование <xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> имеет минимальные издержки при подходе к взаимодействию JS, интерфейсы API JavaScript, необходимые для взаимодействия с этими API-интерфейсами, в настоящее время не документированы и подвержены критическим изменениям в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="73a55-145">While using <xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> has the least overhead of the JS interop approaches, the JavaScript APIs required to interact with these APIs are currently undocumented and subject to breaking changes in future releases.</span></span>

## <a name="reduce-app-size"></a><span data-ttu-id="73a55-146">Уменьшить размер приложения</span><span class="sxs-lookup"><span data-stu-id="73a55-146">Reduce app size</span></span>

### <a name="intermediate-language-il-linking"></a><span data-ttu-id="73a55-147">Компоновка промежуточного языка (IL)</span><span class="sxs-lookup"><span data-stu-id="73a55-147">Intermediate Language (IL) linking</span></span>

<span data-ttu-id="73a55-148">[ Blazor Связывание Приложение "сборка](xref:host-and-deploy/blazor/configure-linker) " уменьшает размер приложения, обрезая неиспользуемый код в двоичных файлах приложения.</span><span class="sxs-lookup"><span data-stu-id="73a55-148">[Linking a Blazor WebAssembly app](xref:host-and-deploy/blazor/configure-linker) reduces the app's size by trimming unused code in the app's binaries.</span></span> <span data-ttu-id="73a55-149">По умолчанию компоновщик включается только при построении в `Release` конфигурации.</span><span class="sxs-lookup"><span data-stu-id="73a55-149">By default, the linker is only enabled when building in `Release` configuration.</span></span> <span data-ttu-id="73a55-150">Чтобы воспользоваться этим преимуществом, опубликуйте приложение для развертывания с помощью команды [DotNet Publish](/dotnet/core/tools/dotnet-publish) с параметром [-c |--Configuration](/dotnet/core/tools/dotnet-publish#options) , имеющим значение `Release` :</span><span class="sxs-lookup"><span data-stu-id="73a55-150">To benefit from this, publish the app for deployment using the [dotnet publish](/dotnet/core/tools/dotnet-publish) command with the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option set to `Release`:</span></span>

```dotnetcli
dotnet publish -c Release
```

### <a name="disable-unused-features"></a><span data-ttu-id="73a55-151">Отключить неиспользуемые функции</span><span class="sxs-lookup"><span data-stu-id="73a55-151">Disable unused features</span></span>

Blazor<span data-ttu-id="73a55-152">Среда выполнения этой сборки включает следующие функции .NET, которые могут быть отключены, если приложение не требует их для меньшего размера полезных данных:</span><span class="sxs-lookup"><span data-stu-id="73a55-152"> WebAssembly's runtime includes the following .NET features that can be disabled if the app doesn't require them for a smaller payload size:</span></span>

* <span data-ttu-id="73a55-153">Файл данных включается, чтобы сделать сведения о часовом поясе правильными.</span><span class="sxs-lookup"><span data-stu-id="73a55-153">A data file is included to make timezone information correct.</span></span> <span data-ttu-id="73a55-154">Если приложению не нужна эта функция, попробуйте отключить ее, задав `BlazorEnableTimeZoneSupport` для свойства MSBuild в файле проекта приложения `false` :</span><span class="sxs-lookup"><span data-stu-id="73a55-154">If the app doesn't require this feature, consider disabling it by setting the `BlazorEnableTimeZoneSupport` MSBuild property in the app's project file to `false`:</span></span>

  ```xml
  <PropertyGroup>
    <BlazorEnableTimeZoneSupport>false</BlazorEnableTimeZoneSupport>
  </PropertyGroup>
  ```

* <span data-ttu-id="73a55-155">Сведения о параметрах сортировки включаются для того, чтобы интерфейсы API <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> работали правильно.</span><span class="sxs-lookup"><span data-stu-id="73a55-155">Collation information is included to make APIs such as <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> work correctly.</span></span> <span data-ttu-id="73a55-156">Если вы уверены, что приложению не требуются данные параметров сортировки, рассмотрите возможность отключения, задав `BlazorWebAssemblyPreserveCollationData` для свойства MSBuild в файле проекта приложения `false` :</span><span class="sxs-lookup"><span data-stu-id="73a55-156">If you're certain that the app doesn't require the collation data, consider disabling it by setting the `BlazorWebAssemblyPreserveCollationData` MSBuild property in the app's project file to `false`:</span></span>

  ```xml
  <PropertyGroup>
    <BlazorWebAssemblyPreserveCollationData>false</BlazorWebAssemblyPreserveCollationData>
  </PropertyGroup>
  ```