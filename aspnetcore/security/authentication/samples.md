---
title: Примеры проверки подлинности для ASP.NET Core
author: rick-anderson
description: Содержит ссылки на примеры проверки подлинности в репозитории ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/samples
ms.openlocfilehash: 95915c28b132a1a48fb772b2663794754006627f
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405358"
---
# <a name="authentication-samples-for-aspnet-core"></a>Примеры проверки подлинности для ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

::: moniker range=">= aspnetcore-3.0"

[Репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/Samples* :

* [Преобразование утверждений](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/ClaimsTransformation)
* [Проверка подлинности файлов cookie](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Cookies)
* [Поставщик настраиваемой политики — Иаусоризатионполиципровидер](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/CustomPolicyProvider)
* [Схемы и параметры динамической проверки подлинности](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/DynamicSchemes)
* [Внешние утверждения](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Identity.ExternalClaims)
* [Выбор между файлами cookie и другой схемой проверки подлинности на основе запроса](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/PathSchemeSelection)
* [Разрешает доступ к статическим файлам](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Запуск шаблонов

* Выберите [ветвь](https://github.com/dotnet/AspNetCore). Например `release/3.1`.
* Клонировать или скачать [репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore).
* Убедитесь, что установлена версия [пакет SDK для .NET Core](https://dotnet.microsoft.com/download/dotnet-core) , соответствующая клону репозитория ASP.NET Core.
* Перейдите к примеру в *AspNetCore/src/Security/Samples* и запустите пример с помощью `dotnet run` .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[Репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore) содержит следующие примеры проверки подлинности в папке *AspNetCore/src/Security/Samples* :

* [Преобразование утверждений](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Проверка подлинности файлов cookie](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Поставщик настраиваемой политики — Иаусоризатионполиципровидер](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Схемы и параметры динамической проверки подлинности](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Внешние утверждения](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Выбор между файлами cookie и другой схемой проверки подлинности на основе запроса](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Разрешает доступ к статическим файлам](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Запуск шаблонов

* Выберите [ветвь](https://github.com/dotnet/AspNetCore). Например `release/2.2`.
* Клонировать или скачать [репозиторий ASP.NET Core](https://github.com/dotnet/AspNetCore).
* Убедитесь, что установлена версия [пакет SDK для .NET Core](https://dotnet.microsoft.com/download/dotnet-core) , соответствующая клону репозитория ASP.NET Core.
* Перейдите к примеру в *AspNetCore/src/Security/Samples* и запустите пример с помощью `dotnet run` .

::: moniker-end
