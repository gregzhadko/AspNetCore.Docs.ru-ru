---
title: Авторизация на основе представлений в ASP.NET Core MVC
author: rick-anderson
description: В этом документе показано, как внедрить и использовать службу авторизации в Razor представлении ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/views
ms.openlocfilehash: ce2365dbdcffc8c44d0d9d7cc6c9d29d0fe1895c
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408933"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>Авторизация на основе представлений в ASP.NET Core MVC

Разработчик часто хочет показать, скрыть или иным образом изменить пользовательский интерфейс на основе удостоверения текущего пользователя. Доступ к службе авторизации можно получить в представлениях MVC с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection). Чтобы внедрить службу авторизации в Razor представление, используйте `@inject` директиву:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Если вы хотите, чтобы служба авторизации была в любом представлении, поместите `@inject` директиву в файл *_ViewImports. cshtml* каталога *views* . Дополнительные сведения: [Внедрение зависимостей в представления](xref:mvc/views/dependency-injection).

Используйте внедренную службу авторизации для вызова `AuthorizeAsync` точно так же, как при [авторизации на основе ресурсов](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

В некоторых случаях ресурсом будет модель представления. Вызов `AuthorizeAsync` выполняется точно так же, как при [авторизации на основе ресурсов](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

В приведенном выше коде модель передается как ресурс, который следует учитывать при оценке политики.

> [!WARNING]
> Не полагайтесь на переключение видимости элементов пользовательского интерфейса приложения в качестве единственной проверки авторизации. Скрытие элемента пользовательского интерфейса может не полностью препятствовать доступу к связанному действию контроллера. Например, рассмотрим кнопку в предыдущем фрагменте кода. Пользователь может вызвать `Edit` метод действия, если ему известен относительный URL-адрес ресурса — */Document/Edit/1*. По этой причине `Edit` метод действия должен выполнять собственную проверку авторизации.
