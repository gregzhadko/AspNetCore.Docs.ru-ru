---
title: Привязка к данным в ASP.NET Core Blazor
author: guardrex
description: Сведения о функциях привязки данных для компонентов и элементов модели DOM в приложениях Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: 5b49d2598a451ee607e034913bd1aeaa03f941c6
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511201"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a><span data-ttu-id="3f67a-103">Привязка к данным в ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="3f67a-103">ASP.NET Core Blazor data binding</span></span>

<span data-ttu-id="3f67a-104">Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Дэниэл Рот](https://github.com/danroth27) (Daniel Roth)</span><span class="sxs-lookup"><span data-stu-id="3f67a-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="3f67a-105">Компоненты Razor реализуют функции привязки данных для поля, свойства или значения выражения Razor с помощью атрибута HTML-элемента [`@bind`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="3f67a-105">Razor components provide data binding features via an HTML element attribute named [`@bind`](xref:mvc/views/razor#bind) with a field, property, or Razor expression value.</span></span>

<span data-ttu-id="3f67a-106">В следующем примере показана привязка свойства `CurrentValue` к значению текстового поля:</span><span class="sxs-lookup"><span data-stu-id="3f67a-106">The following example binds the `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="3f67a-107">Когда текстовое поле теряет фокус, значение свойства обновляется.</span><span class="sxs-lookup"><span data-stu-id="3f67a-107">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="3f67a-108">Текстовое поле обновляется в пользовательском интерфейсе только при отрисовке компонента, а не в ответ на изменение значения свойства.</span><span class="sxs-lookup"><span data-stu-id="3f67a-108">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="3f67a-109">Так как компоненты отрисовываются после выполнения кода обработчика событий, изменения свойств *обычно* отражаются в пользовательском интерфейсе сразу после активации обработчика событий.</span><span class="sxs-lookup"><span data-stu-id="3f67a-109">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="3f67a-110">Использование атрибута `@bind` со свойством `CurrentValue` (`<input @bind="CurrentValue" />`) фактически эквивалентно следующему:</span><span class="sxs-lookup"><span data-stu-id="3f67a-110">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="3f67a-111">Когда компонент отрисовывается, значение `value` элемента input берется из свойства `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-111">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="3f67a-112">Когда пользователь вводит данные в текстовом поле, а затем переводит фокус, инициируется событие `onchange`, и свойству `CurrentValue` присваивается измененное значение.</span><span class="sxs-lookup"><span data-stu-id="3f67a-112">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="3f67a-113">На практике код обычно сложнее, так как атрибут `@bind` применяется в случаях, когда выполняется преобразование типов.</span><span class="sxs-lookup"><span data-stu-id="3f67a-113">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="3f67a-114">Как правило, `@bind` связывает текущее значение выражения с атрибутом `value` и обрабатывает изменения с помощью зарегистрированного обработчика.</span><span class="sxs-lookup"><span data-stu-id="3f67a-114">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="3f67a-115">Чтобы привязать свойство или поле к другим событиям, можно использовать атрибут `@bind:event` с параметром `event`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-115">Bind a property or field on other events by also including an `@bind:event` attribute with an `event` parameter.</span></span> <span data-ttu-id="3f67a-116">В следующем примере свойство `CurrentValue` привязывается к событию `oninput`:</span><span class="sxs-lookup"><span data-stu-id="3f67a-116">The following example binds the `CurrentValue` property on the `oninput` event:</span></span>

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="3f67a-117">В отличие от события `onchange`, которое происходит, когда элемент теряет фокус, событие `oninput` происходит при изменении значения в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="3f67a-117">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="3f67a-118">Для привязки атрибутов элементов, отличных от `value`, используйте `@bind-{ATTRIBUTE}` с синтаксисом `@bind-{ATTRIBUTE}:event`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-118">Use `@bind-{ATTRIBUTE}` with `@bind-{ATTRIBUTE}:event` syntax to bind element attributes other than `value`.</span></span> <span data-ttu-id="3f67a-119">В следующем примере стиль абзаца обновляется при изменении значения `_paragraphStyle`:</span><span class="sxs-lookup"><span data-stu-id="3f67a-119">In the following example, the paragraph's style is updated when the `_paragraphStyle` value changes:</span></span>

```razor
@page "/binding-example"

<p>
    <input type="text" @bind="_paragraphStyle" />
</p>

<p @bind-style="_paragraphStyle" @bind-style:event="onchange">
    Blazorify the app!
</p>

@code {
    private string _paragraphStyle = "color:red";
}
```

<span data-ttu-id="3f67a-120">Привязка атрибута учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="3f67a-120">Attribute binding is case sensitive.</span></span> <span data-ttu-id="3f67a-121">Например, `@bind` допустимо использовать, а `@Bind` нет.</span><span class="sxs-lookup"><span data-stu-id="3f67a-121">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

## <a name="unparsable-values"></a><span data-ttu-id="3f67a-122">Значения, не поддающиеся анализу</span><span class="sxs-lookup"><span data-stu-id="3f67a-122">Unparsable values</span></span>

<span data-ttu-id="3f67a-123">Когда пользователь присваивает не поддающееся анализу значение элементу, привязанному к данным, при активации события привязки автоматически восстанавливается предыдущее значение.</span><span class="sxs-lookup"><span data-stu-id="3f67a-123">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="3f67a-124">Рассмотрим следующий сценарий.</span><span class="sxs-lookup"><span data-stu-id="3f67a-124">Consider the following scenario:</span></span>

* <span data-ttu-id="3f67a-125">Элемент `<input>` привязан к типу `int` с начальным значением `123`:</span><span class="sxs-lookup"><span data-stu-id="3f67a-125">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="3f67a-126">Пользователь изменяет значение элемента на `123.45` на странице и переводит фокус.</span><span class="sxs-lookup"><span data-stu-id="3f67a-126">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="3f67a-127">В приведенном выше сценарии восстанавливается значение элемента `123`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-127">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="3f67a-128">Когда значение `123.45` отклоняется и вместо него используется исходное значение `123`, пользователь понимает, что его значение не было принято.</span><span class="sxs-lookup"><span data-stu-id="3f67a-128">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="3f67a-129">По умолчанию привязка применяется к событию `onchange` элемента (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="3f67a-129">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="3f67a-130">Чтобы выполнить привязку к другому событию, используйте `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-130">Use `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}` to trigger binding on a different event.</span></span> <span data-ttu-id="3f67a-131">Для события `oninput` (`@bind:event="oninput"`) исходное значение восстанавливается после любого нажатия клавиши, которое приводит к вводу не поддающегося анализу значения.</span><span class="sxs-lookup"><span data-stu-id="3f67a-131">For the `oninput` event (`@bind:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="3f67a-132">При привязке типа `int` к событию `oninput` пользователь не может ввести символ `.`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-132">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="3f67a-133">Символ `.` немедленно удаляется, так что пользователь сразу понимает, что допустимы только целые числа.</span><span class="sxs-lookup"><span data-stu-id="3f67a-133">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="3f67a-134">В некоторых ситуациях восстанавливать исходное значение при возникновении события `oninput` нежелательно, например, когда пользователю следует дать возможность самому удалять не поддающиеся анализу значения `<input>`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-134">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="3f67a-135">Ниже представлены возможные альтернативы.</span><span class="sxs-lookup"><span data-stu-id="3f67a-135">Alternatives include:</span></span>

* <span data-ttu-id="3f67a-136">Не используйте событие `oninput`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-136">Don't use the `oninput` event.</span></span> <span data-ttu-id="3f67a-137">Используйте вместо этого событие `onchange` по умолчанию (укажите просто `@bind="{PROPERTY OR FIELD}"`). В этом случае недопустимое значение не будет меняться на исходное, пока элемент не потеряет фокус.</span><span class="sxs-lookup"><span data-stu-id="3f67a-137">Use the default `onchange` event (only specify `@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="3f67a-138">Выполните привязку к типу, допускающему значение NULL, например `int?` или `string`, и предоставьте пользовательскую логику для обработки недопустимых значений.</span><span class="sxs-lookup"><span data-stu-id="3f67a-138">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="3f67a-139">Используйте [компонент для проверки формы](xref:blazor/forms-validation), например `InputNumber` или `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-139">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="3f67a-140">Компоненты для проверки форм имеют встроенную поддержку управления недопустимыми входными данными.</span><span class="sxs-lookup"><span data-stu-id="3f67a-140">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="3f67a-141">Компоненты для проверки форм:</span><span class="sxs-lookup"><span data-stu-id="3f67a-141">Form validation components:</span></span>
  * <span data-ttu-id="3f67a-142">позволяют пользователю вводить недопустимые данные и получать ошибки проверки в соответствующем контексте `EditContext`;</span><span class="sxs-lookup"><span data-stu-id="3f67a-142">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="3f67a-143">выводят ошибки проверки в пользовательском интерфейсе, не мешая пользователю вводить дополнительные данные в веб-форме.</span><span class="sxs-lookup"><span data-stu-id="3f67a-143">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

## <a name="format-strings"></a><span data-ttu-id="3f67a-144">Строки формата</span><span class="sxs-lookup"><span data-stu-id="3f67a-144">Format strings</span></span>

<span data-ttu-id="3f67a-145">Привязка данных работает со строками формата <xref:System.DateTime> посредством атрибута [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="3f67a-145">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="3f67a-146">Другие выражения форматирования, например денежные или числовые форматы, в настоящее время недоступны.</span><span class="sxs-lookup"><span data-stu-id="3f67a-146">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="3f67a-147">В приведенном выше коде типом поля элемента `<input>` (`type`) по умолчанию является `text`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-147">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="3f67a-148">`@bind:format` поддерживается для привязки следующих типов .NET:</span><span class="sxs-lookup"><span data-stu-id="3f67a-148">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="3f67a-149"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="3f67a-149"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="3f67a-150"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="3f67a-150"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="3f67a-151">Атрибут `@bind:format` указывает формат даты, применяемый к значению `value` элемента `<input>`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-151">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="3f67a-152">Формат также используется для синтаксического анализа значения при возникновении события `onchange`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-152">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="3f67a-153">Указывать формат для типа поля `date` не рекомендуется, так как в Blazor есть встроенная поддержка форматирования дат.</span><span class="sxs-lookup"><span data-stu-id="3f67a-153">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="3f67a-154">Если все же для типа поля `date` указывается формат, для правильной работы привязки используйте только формат даты `yyyy-MM-dd`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-154">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a><span data-ttu-id="3f67a-155">Привязка родительского компонента к дочернему с помощью параметров компонентов</span><span class="sxs-lookup"><span data-stu-id="3f67a-155">Parent-to-child binding with component parameters</span></span>

<span data-ttu-id="3f67a-156">Привязка поддерживает параметры компонентов: с помощью атрибута `@bind-{PROPERTY}` можно выполнить привязку значения свойства от родительского компонента к дочернему.</span><span class="sxs-lookup"><span data-stu-id="3f67a-156">Binding recognizes component parameters, where `@bind-{PROPERTY}` can bind a property value from a parent component down to a child component.</span></span> <span data-ttu-id="3f67a-157">Привязка дочернего компонента к родительскому рассматривается в разделе [Привязка дочернего компонента к родительскому посредством цепочки привязки](#child-to-parent-binding-with-chained-bind).</span><span class="sxs-lookup"><span data-stu-id="3f67a-157">Binding from a child to a parent is covered in the [Child-to-parent binding with chained bind](#child-to-parent-binding-with-chained-bind) section.</span></span>

<span data-ttu-id="3f67a-158">Следующий дочерний компонент (`ChildComponent`) имеет параметр компонента `Year` и обратный вызов `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="3f67a-158">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="3f67a-159">Назначение структуры `EventCallback<T>` описывается в разделе <xref:blazor/event-handling#eventcallback>.</span><span class="sxs-lookup"><span data-stu-id="3f67a-159">`EventCallback<T>` is explained in <xref:blazor/event-handling#eventcallback>.</span></span>

<span data-ttu-id="3f67a-160">Рассмотрим представленный ниже родительский компонент.</span><span class="sxs-lookup"><span data-stu-id="3f67a-160">The following parent component uses:</span></span>

* <span data-ttu-id="3f67a-161">Он использует `ChildComponent`, а его параметр `ParentYear` привязывается к параметру `Year` дочернего компонента.</span><span class="sxs-lookup"><span data-stu-id="3f67a-161">`ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component.</span></span>
* <span data-ttu-id="3f67a-162">Событие `onclick` используется для активации метода `ChangeTheYear`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-162">The `onclick` event is used to trigger the `ChangeTheYear` method.</span></span> <span data-ttu-id="3f67a-163">Для получения дополнительной информации см. <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="3f67a-163">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="3f67a-164">При загрузке `ParentComponent` создается следующая разметка:</span><span class="sxs-lookup"><span data-stu-id="3f67a-164">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="3f67a-165">Если значение свойства `ParentYear` изменяется в результате нажатия кнопки в `ParentComponent`, свойство `Year` компонента `ChildComponent` обновляется.</span><span class="sxs-lookup"><span data-stu-id="3f67a-165">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="3f67a-166">Новое значение свойства `Year` отображается в пользовательском интерфейсе при повторной отрисовке компонента `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="3f67a-166">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="3f67a-167">Параметр `Year` допускает привязку, так как он имеет сопутствующее событие `YearChanged`, соответствующее типу параметра `Year`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-167">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="3f67a-168">В соответствии с соглашением `<ChildComponent @bind-Year="ParentYear" />` фактически эквивалентно следующему коду:</span><span class="sxs-lookup"><span data-stu-id="3f67a-168">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="3f67a-169">Как правило, свойство можно привязать к соответствующему обработчику событий, включив атрибут `@bind-{PROPRETY}:event`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-169">In general, a property can be bound to a corresponding event handler by including an `@bind-{PROPRETY}:event` attribute.</span></span> <span data-ttu-id="3f67a-170">Например, свойство `MyProp` можно привязать к `MyEventHandler` с помощью следующих двух атрибутов:</span><span class="sxs-lookup"><span data-stu-id="3f67a-170">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a><span data-ttu-id="3f67a-171">Привязка дочернего компонента к родительскому посредством цепочки привязки</span><span class="sxs-lookup"><span data-stu-id="3f67a-171">Child-to-parent binding with chained bind</span></span>

<span data-ttu-id="3f67a-172">Распространенным сценарием является связывание привязанного к данным параметра к элементу страницы в выходных данных компонента.</span><span class="sxs-lookup"><span data-stu-id="3f67a-172">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="3f67a-173">Такой сценарий называется *цепочкой привязки*, так как одновременно имеется несколько уровней привязки.</span><span class="sxs-lookup"><span data-stu-id="3f67a-173">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="3f67a-174">Цепочку привязки нельзя реализовать с помощью синтаксиса `@bind` в элементе страницы.</span><span class="sxs-lookup"><span data-stu-id="3f67a-174">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="3f67a-175">Обработчик событий и значение необходимо указывать отдельно.</span><span class="sxs-lookup"><span data-stu-id="3f67a-175">The event handler and value must be specified separately.</span></span> <span data-ttu-id="3f67a-176">Однако родительский компонент может использовать синтаксис `@bind` с параметром компонента.</span><span class="sxs-lookup"><span data-stu-id="3f67a-176">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="3f67a-177">Приведенный ниже компонент `PasswordField` (*PasswordField.razor*) делает следующее:</span><span class="sxs-lookup"><span data-stu-id="3f67a-177">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="3f67a-178">присваивает элементу `<input>` значение свойства `Password`;</span><span class="sxs-lookup"><span data-stu-id="3f67a-178">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="3f67a-179">сообщает об изменениях свойства `Password` родительскому компоненту посредством [EventCallback](xref:blazor/event-handling#eventcallback);</span><span class="sxs-lookup"><span data-stu-id="3f67a-179">Exposes changes of the `Password` property to a parent component with an [EventCallback](xref:blazor/event-handling#eventcallback).</span></span>
* <span data-ttu-id="3f67a-180">использует событие `onclick` для активации метода `ToggleShowPassword`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-180">Uses the `onclick` event is used to trigger the `ToggleShowPassword` method.</span></span> <span data-ttu-id="3f67a-181">Для получения дополнительной информации см. <xref:blazor/event-handling>.</span><span class="sxs-lookup"><span data-stu-id="3f67a-181">For more information, see <xref:blazor/event-handling>.</span></span>

```razor
<h1>Child Component</h2>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

<span data-ttu-id="3f67a-182">Компонент `PasswordField` используется в другом компоненте:</span><span class="sxs-lookup"><span data-stu-id="3f67a-182">The `PasswordField` component is used in another component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="3f67a-183">Для проверки пароля или перехвата ошибок в нем в предыдущем примере выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="3f67a-183">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="3f67a-184">Создайте резервное поле для `Password` (`_password` в следующем примере кода).</span><span class="sxs-lookup"><span data-stu-id="3f67a-184">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="3f67a-185">Выполните проверки или перехватите ошибки в методе задания `Password`.</span><span class="sxs-lookup"><span data-stu-id="3f67a-185">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="3f67a-186">В следующем примере пользователю немедленно сообщается о том, что в пароле есть пробел:</span><span class="sxs-lookup"><span data-stu-id="3f67a-186">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

## <a name="radio-buttons"></a><span data-ttu-id="3f67a-187">Переключатели</span><span class="sxs-lookup"><span data-stu-id="3f67a-187">Radio buttons</span></span>

<span data-ttu-id="3f67a-188">Сведения о привязке к переключателям в форме см. в разделе <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="3f67a-188">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>