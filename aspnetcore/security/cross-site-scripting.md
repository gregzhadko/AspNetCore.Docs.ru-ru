---
title: Предотвращение межсайтовых сценариев (XSS) в ASP.NET Core
author: rick-anderson
description: Сведения о межузловых сценариях (XSS) и методах решения этой уязвимости в ASP.NET Core приложении.
ms.author: riande
ms.date: 10/02/2018
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/cross-site-scripting
ms.openlocfilehash: a94fe1612c023468238f09a91ddb0346b65d52ba
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408023"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Предотвращение межсайтовых сценариев (XSS) в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Межсайтовые сценарии (XSS) — это уязвимость системы безопасности, которая позволяет злоумышленнику размещать сценарии на стороне клиента (обычно JavaScript) на веб-страницах. Когда другие пользователи загружают затронутые страницы, запускаются сценарии злоумышленника, позволяющие злоумышленнику украсть файлы cookie и токены сеансов, изменить содержимое веб-страницы с помощью манипуляций DOM или перенаправить браузер на другую страницу. Уязвимости XSS обычно возникают, когда приложение принимает введенные пользователем данные и выводит его на страницу без проверки, кодирования или экранирования.

## <a name="protecting-your-application-against-xss"></a>Защита приложения с помощью XSS

На уровне XSS уровня "базовый" работает путем того, как приложение вставляет `<script>` тег на отрисованную страницу или вставляет `On*` событие в элемент. Разработчики должны использовать следующие шаги по предотвращению, чтобы избежать введения XSS в приложение.

1. Никогда не размещайте недоверенные данные в входных данных HTML, если не выполнить остальные действия, описанные ниже. Недоверенные данные — это любые данные, которыми может управлять злоумышленник, входные данные HTML-форм, строки запросов, заголовки HTTP, даже данные, источником которых является база данных, которые могут стать злоумышленниками, даже если они не могут повредить ваше приложение.

2. Перед размещением недоверенных данных внутри элемента HTML убедитесь, что они закодированы в формате HTML. Кодировка HTML принимает такие символы, как &lt; и, и изменяет их в сейфовую форму, например &amp; lt;

3. Перед помещением непроверенных данных в атрибут HTML убедитесь, что он закодирован в формате HTML. Кодирование атрибута HTML — это надмножество кодировки HTML и кодирует дополнительные символы, например "и".

4. Перед размещением недоверенных данных в JavaScript Поместите данные в элемент HTML, содержимое которого извлекается во время выполнения. Если это невозможно, убедитесь, что данные кодируются в JavaScript. Кодировка JavaScript принимает небезопасные символы для JavaScript и заменяет их шестнадцатеричным значением, например в &lt; кодировке `\u003C` .

5. Перед размещением недоверенных данных в строке запроса URL-адреса убедитесь, что он кодируется в формате URL.

## <a name="html-encoding-using-razor"></a>Кодирование HTML с помощьюRazor

RazorЯдро, используемое в MVC, автоматически кодирует все выходные данные, источником которых являются переменные, если только вы не уверены, что это делает. При использовании директивы используются правила кодирования атрибутов HTML *@* . Поскольку кодировка атрибута HTML является надмножеством кодировки HTML, это означает, что вам не нужно беспокоиться о том, следует ли использовать кодировку HTML или кодировку атрибута HTML. Необходимо убедиться, что используется только @ в контексте HTML, а не при попытке вставить недоверенные входные данные непосредственно в JavaScript. Вспомогательные функции тегов также будут кодировать входные данные, используемые в параметрах тегов.

Примите следующее Razor представление:

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

В этом представлении выводится содержимое переменной *унтрустединпут* . Эта переменная содержит некоторые символы, которые используются в атаках XSS, &lt; а именно и &gt; . При проверке исходного кода отображаются выходные данные, закодированные как:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC предоставляет `HtmlString` класс, который не кодируется автоматически при выводе. Этот параметр никогда не должен использоваться в сочетании с ненадежными входными данными, так как в этом случае будет предоставлена уязвимость XSS.

## <a name="javascript-encoding-using-razor"></a>Кодирование JavaScript с помощьюRazor

Иногда требуется вставить значение в код JavaScript для обработки в представлении. Это можно сделать двумя способами. Самым надежным способом вставки значений является размещение значения в атрибуте данных тега и его получение в JavaScript. Пример.

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

Это приведет к созданию следующего HTML-кода

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

При запуске будет отображаться следующее:

```
<"123">
   <"123">
```

Вы также можете вызвать кодировщик JavaScript напрямую:

```cshtml
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
```

Это будет отображаться в браузере следующим образом:

```html
<script>
    document.write("\u003C\u0022123\u0022\u003E");
</script>
```

>[!WARNING]
> Не следует объединять ненадежные входные данные в JavaScript для создания элементов DOM. Следует использовать `createElement()` и назначать значения свойств соответствующим образом `node.TextContent=` , например, или использовать `element.SetAttribute()` / `element[attribute]=` в противном случае — для XSS на основе DOM.

## <a name="accessing-encoders-in-code"></a>Доступ к кодировщикам в коде

Кодировщики HTML, JavaScript и URL-адреса доступны в коде двумя способами: их можно внедрять с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection) или использовать кодировщики по умолчанию, содержащиеся в `System.Text.Encodings.Web` пространстве имен. Если вы используете кодировщики по умолчанию, любые значения, которые вы применили к диапазонам символов, не вступают в силу. кодировщики по умолчанию используют наиболее надежные правила кодировки.

Для использования настраиваемых кодировщиков с помощью ondi конструкторы должны использовать параметр *HtmlEncoder*, *JavaScriptEncoder* и *UrlEncoder* , если это уместно. Например:

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a>Параметры URL-адреса кодировки

Если требуется создать строку запроса URL-адреса с ненадежным входом в качестве значения, используйте `UrlEncoder` для кодирования значения. Например,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

После кодирования переменная Енкодедвалуе будет содержать `%22Quoted%20Value%20with%20spaces%20and%20%26%22` . Пробелы, кавычки, знаки препинания и другие незащищенные символы будут закодироваться в шестнадцатеричное значение, например, символ пробела станет %20.

>[!WARNING]
> Не используйте ненадежные входные данные в качестве части URL-пути. Всегда передавать недоверенные входные данные в качестве значения строки запроса.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Настройка кодировщиков

По умолчанию кодировщики используют список безопасности, ограниченный базовым диапазоном символов латиницы в Юникоде, и закодировать все символы за пределами этого диапазона в качестве эквивалентов кода символа. Это поведение также влияет на Razor отрисовку TagHelper и HtmlHelper, так как она будет использовать кодировщики для вывода строк.

Причина этого заключается в защите от неизвестных или будущих ошибок браузера (предыдущие ошибки браузера приступили к анализу на основе обработки символов, отличных от английского). Если ваш веб-узел сильно использует символы, отличные от латинских, например китайский, кириллица или другие, это, вероятно, не является нужным поведением.

Списки надежных кодировщиков можно настроить для включения диапазонов Юникода, соответствующих приложению, во время запуска в `ConfigureServices()` .

Например, используя конфигурацию по умолчанию, можно использовать HtmlHelper, например Razor .

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

При просмотре исходного кода веб-страницы он отображается следующим образом с кодировкой китайского текста.

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Чтобы расширить символы, обрабатываемые кодировщиком, вставьте следующую строку в `ConfigureServices()` метод в `startup.cs` .

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

В этом примере список безопасного списка расширяется для включения диапазона Юникода Кжкунифиедидеографс. Отображаемые выходные данные теперь станут

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Диапазоны безсписок надежных списков указываются как диаграммы с кодом Юникода, а не язык. [Стандарт Unicode](https://unicode.org/) содержит список [диаграмм кода](https://www.unicode.org/charts/index.html) , которые можно использовать для поиска диаграммы, содержащей символы. Каждый кодировщик, HTML, JavaScript и URL-адрес необходимо настроить отдельно.

> [!NOTE]
> Настройка безопасного списка влияет только на кодировщики, источником которых является ondi. Если вы напрямую обращаетесь к кодировщику `System.Text.Encodings.Web.*Encoder.Default` по умолчанию, будет использоваться только обычный Латинский доступ.

## <a name="where-should-encoding-take-place"></a>Где следует выполнять кодирование?

Общепринятой практикой является то, что кодирование происходит в момент выходных данных, а закодированные значения никогда не должны храниться в базе данных. Кодирование в точке вывода позволяет изменять использование данных, например, из HTML в значение строки запроса. Он также позволяет легко искать данные без необходимости кодирования значений перед поиском и позволяет использовать любые изменения или исправления ошибок, внесенные в кодировщики.

## <a name="validation-as-an-xss-prevention-technique"></a>Проверка как методика предотвращения XSS

Проверка может быть полезным инструментом при ограничении атак XSS. Например, числовая строка, содержащая только символы 0-9, не будет вызывать атаку XSS. Проверка выполняется более сложно при приеме HTML в пользовательском вводе. Анализ входных данных HTML является сложным, если это невозможно. Markdown, в сочетании с средством синтаксического анализа, которое является лентой внедренного HTML, является более безопасным вариантом для приема расширенных входных данных. Никогда не полагайтесь только на проверку. Всегда кодировать недоверенные входные данные перед выводом независимо от того, какая проверка или очистка выполнена.
