---
title: 'Настройка ASP.NET Core:::no-loc(Identity):::'
author: AdrienTorris
description: 'Изучите ASP.NET Core :::no-loc(Identity)::: значения по умолчанию и Узнайте, как настроить :::no-loc(Identity)::: свойства для использования пользовательских значений.'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
no-loc:
- ':::no-loc(Blazor):::'
- ':::no-loc(Blazor Server):::'
- ':::no-loc(Blazor WebAssembly):::'
- ':::no-loc(Identity):::'
- ":::no-loc(Let's Encrypt):::"
- ':::no-loc(Razor):::'
- ':::no-loc(SignalR):::'
uid: security/authentication/identity-configuration
ms.openlocfilehash: 5c999b426742cf75b1997f5b40223e2dda112901
ms.sourcegitcommit: 1b89fc58114a251926abadfd5c69c120f1ba12d8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2020
ms.locfileid: "87160288"
---
# <a name="configure-aspnet-core-no-locidentity"></a>Настройка ASP.NET Core:::no-loc(Identity):::

ASP.NET Core :::no-loc(Identity)::: использует значения по умолчанию для таких параметров, как политика паролей, блокировка и конфигурация файлов cookie. Эти параметры можно переопределить в `Startup` классе.

## <a name="no-locidentity-options"></a>Параметры :::no-loc(Identity):::

Класс [ :::no-loc(Identity)::: Options](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) представляет параметры, которые можно использовать для настройки :::no-loc(Identity)::: системы. `:::no-loc(Identity):::Options`необходимо задать **после** вызова метода `Add:::no-loc(Identity):::` или `AddDefault:::no-loc(Identity):::` .

### <a name="claims-no-locidentity"></a>Претензи:::no-loc(Identity):::

[ :::no-loc(Identity)::: Options. Claims :::no-loc(Identity)::: ](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.claimsidentity) указывает [ :::no-loc(Identity)::: Параметры утверждений](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions) со свойствами, приведенными в следующей таблице.

| Свойство | Описание | По умолчанию |
| -------- | ----------- | :-----: |
| [ролеклаимтипе](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.roleclaimtype) | Возвращает или задает тип утверждения, используемого для утверждения роли. | [ClaimTypes. Role](/dotnet/api/system.security.claims.claimtypes.role) |
| [секуритистампклаимтипе](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.securitystampclaimtype) | Возвращает или задает тип утверждения, используемого для утверждения метки безопасности. | `AspNet.:::no-loc(Identity):::.SecurityStamp` |
| [UserIdClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.useridclaimtype) | Возвращает или задает тип утверждения, используемого для утверждения идентификатора пользователя. | [ClaimTypes. NameIdentifier](/dotnet/api/system.security.claims.claimtypes.nameidentifier) |
| [UserNameClaimType](/dotnet/api/microsoft.aspnetcore.identity.claimsidentityoptions.usernameclaimtype) | Возвращает или задает тип утверждения, используемого для утверждения имени пользователя. | [ClaimTypes.Name](/dotnet/api/system.security.claims.claimtypes.name) |

### <a name="lockout"></a>Блокирование

Блокировка задается в методе [пассвордсигнинасинк](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync#Microsoft_AspNetCore_:::no-loc(Identity):::_SignInManager_1_PasswordSignInAsync_System_String_System_String_System_Boolean_System_Boolean_) :

[!code-csharp[](identity-configuration/sample/Areas/:::no-loc(Identity):::/Pages/Account/Login.cshtml.cs?name=snippet&highlight=9)]

Приведенный выше код основан на `Login` :::no-loc(Identity)::: шаблоне. 

Параметры блокировки задаются в `StartUp.ConfigureServices` :

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_lock)]

Приведенный выше код задает [ :::no-loc(Identity)::: Параметры](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) [локкаутоптионс](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) со значениями по умолчанию.

Успешная проверка подлинности сбрасывает количество неудачных попыток доступа и сбрасывает часы.

[ :::no-loc(Identity)::: Options. блокировкой](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.lockout) задает [локкаутоптионс](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions) со свойствами, показанными в таблице.

| Свойство | Описание | По умолчанию |
| -------- | ----------- | :-----: |
| [алловедфорневусерс](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.allowedfornewusers) | Определяет, можно ли заблокировать нового пользователя. | `true` |
| [дефаултлоккауттимеспан](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan) | Количество времени, в течение которого пользователь блокируется в случае блокировки. | 5 минут |
| [максфаиледакцессаттемптс](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) | Число неудачных попыток доступа, пока пользователь не будет заблокирован, если блокировка включена. | 5 |

### <a name="password"></a>Пароль

По умолчанию :::no-loc(Identity)::: требует, чтобы пароли содержали символы в верхнем регистре, символы нижнего регистра, цифры и символы, отличные от буквенно-цифровых. Пароли должны иметь длину не менее шести символов.

Для паролей настраивается:

* <xref:Microsoft.AspNetCore.:::no-loc(Identity):::.PasswordOptions> в `Startup.ConfigureServices`;
* [ `[StringLength]` атрибуты](xref:System.ComponentModel.DataAnnotations.StringLengthAttribute) `Password` свойств, если :::no-loc(Identity)::: в приложении сформировано [формирование шаблонов](xref:security/authentication/scaffold-identity). `InputModel``Password`Свойства находятся в следующих файлах:
  * `Areas/:::no-loc(Identity):::/Pages/Account/Register.cshtml.cs`
  * `Areas/:::no-loc(Identity):::/Pages/Account/ResetPassword.cshtml.cs`

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_pw)]

[ :::no-loc(Identity)::: Параметры. Password](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.password) задает [пассвордоптионс](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions) с свойствами, показанными в таблице.

| Свойство | Описание | По умолчанию |
| -------- | ----------- | :-----: |
| [рекуиредигит](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredigit) | Требуется число от 0-9 в пароле. | `true` |
| [рекуиредленгс](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requiredlength) | Минимальная длина пароля. | 6 |
| [рекуиреловеркасе](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirelowercase) | В пароле требуется символ нижнего регистра. | `true` |
| [рекуиреноналфанумерик](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requirenonalphanumeric) | В пароле требуется символ, отличный от буквенно-цифрового. | `true` |
| [рекуиредуникуечарс](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireduniquechars) | Применяется только к ASP.NET Core 2,0 или более поздней версии.<br><br> Требуется число уникальных символов в пароле. | 1 |
| [рекуиреупперкасе](/dotnet/api/microsoft.aspnetcore.identity.passwordoptions.requireuppercase) | В пароле требуется символ верхнего регистра. | `true` |

### <a name="sign-in"></a>Вход

Следующий код задает `SignIn` Параметры (значения по умолчанию):

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_si)]

[ :::no-loc(Identity)::: Параметры. Signing](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.signin) указывает [сигниноптионс](/dotnet/api/microsoft.aspnetcore.identity.signinoptions) со свойствами, показанными в таблице.

| Свойство | Описание | По умолчанию |
| -------- | ----------- | :-----: |
| [рекуиреконфирмедемаил](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedemail) | Для входа требуется подтвержденное электронное письмо. | `false` |
| [рекуиреконфирмедфоненумбер](/dotnet/api/microsoft.aspnetcore.identity.signinoptions.requireconfirmedphonenumber) | Для входа требуется подтвержденный номер телефона. | `false` |

### <a name="tokens"></a>Токены

[ :::no-loc(Identity)::: Options. Tokens](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.tokens) указывает [токеноптионс](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions) с помощью свойств, показанных в таблице.

| Свойство | Описание |
| -------- | ----------- |
| [аусентикатортокенпровидер](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.authenticatortokenprovider) | Возвращает или задает объект, `AuthenticatorTokenProvider` используемый для проверки двухфакторной регистрации с помощью средства проверки подлинности. |
| [чанжеемаилтокенпровидер](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changeemailtokenprovider) | Возвращает или задает объект, `ChangeEmailTokenProvider` используемый для создания токенов, используемых в сообщениях электронной почты с подтверждением изменений. |
| [чанжефоненумбертокенпровидер](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.changephonenumbertokenprovider) | Возвращает или задает объект, `ChangePhoneNumberTokenProvider` используемый для создания маркеров, используемых при изменении номеров телефонов. |
| [емаилконфирматионтокенпровидер](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.emailconfirmationtokenprovider) | Возвращает или задает поставщик токенов, используемый для создания токенов, используемых в сообщениях электронной почты с подтверждением учетной записи. |
| [пассвордресеттокенпровидер](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.passwordresettokenprovider) | Возвращает или задает [иусертвофактортокенпровидер \<TUser> ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactortokenprovider-1) , используемый для создания токенов, используемых в сообщениях электронной почты для сброса пароля. |
| [провидермап](/dotnet/api/microsoft.aspnetcore.identity.tokenoptions.providermap) | Используется для создания [поставщика пользовательского токена](/dotnet/api/microsoft.aspnetcore.identity.tokenproviderdescriptor) с ключом, используемым в качестве имени поставщика. |

### <a name="user"></a>Пользователь

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_user)]

[ :::no-loc(Identity)::: Параметры. User](/dotnet/api/microsoft.aspnetcore.identity.identityoptions.user) указывает [UserOptions](/dotnet/api/microsoft.aspnetcore.identity.useroptions) со свойствами, показанными в таблице.

| Свойство | Описание | По умолчанию |
| -------- | ----------- | :-----: |
| [алловедусернамечарактерс](/dotnet/api/microsoft.aspnetcore.identity.useroptions.allowedusernamecharacters) | Допустимые символы в имени пользователя. | абкдефгхижклмнопкрстуввксиз<br>ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>0123456789<br>-.\_@+ |
| [рекуиреуникуимаил](/dotnet/api/microsoft.aspnetcore.identity.useroptions.requireuniqueemail) | Требует, чтобы каждый пользователь имел уникальный адрес электронной почты. | `false` |

### <a name="cookie-settings"></a>Параметры файлов cookie

Настройте файл cookie приложения в `Startup.ConfigureServices` . [Конфигуреаппликатионкукие](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie#Microsoft_Extensions_DependencyInjection_:::no-loc(Identity):::ServiceCollectionExtensions_ConfigureApplicationCookie_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_Cookies_CookieAuthenticationOptions__) должен вызываться **после** вызова метода `Add:::no-loc(Identity):::` или `AddDefault:::no-loc(Identity):::` .

[!code-csharp[](identity-configuration/sample/Startup.cs?name=snippet_cookie)]

Дополнительные сведения см. в разделе [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions).

## <a name="password-hasher-options"></a>Параметры хэша пароля

<xref:Microsoft.AspNetCore.:::no-loc(Identity):::.PasswordHasherOptions>Возвращает и задает параметры хэширования паролей.

| Параметр | Описание |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.:::no-loc(Identity):::.PasswordHasherOptions.CompatibilityMode> | Режим совместимости, используемый при хэшировании новых паролей. По умолчанию — <xref:Microsoft.AspNetCore.:::no-loc(Identity):::.PasswordHasherCompatibilityMode.:::no-loc(Identity):::V3>. Первый байт хэшированного пароля, называемый *маркером формата*, указывает версию алгоритма хэширования, используемого для хэширования пароля. При проверке пароля по хэшу <xref:Microsoft.AspNetCore.:::no-loc(Identity):::.PasswordHasher`1.VerifyHashedPassword*> метод выбирает правильный алгоритм на основе первого байта. Клиент может проходить проверку подлинности независимо от того, какая версия алгоритма использовалась для хэширования пароля. Установка режима совместимости влияет на хэширование *новых паролей*. |
| <xref:Microsoft.AspNetCore.:::no-loc(Identity):::.PasswordHasherOptions.IterationCount> | Число итераций, используемых при хэшировании паролей с помощью PBKDF2. Это значение используется только в том случае, если <xref:Microsoft.AspNetCore.:::no-loc(Identity):::.PasswordHasherOptions.CompatibilityMode> свойству присвоено значение <xref:Microsoft.AspNetCore.:::no-loc(Identity):::.PasswordHasherCompatibilityMode.:::no-loc(Identity):::V3> . Значение должно быть положительным целым числом, а по умолчанию — `10000` . |

В следующем примере для задано значение <xref:Microsoft.AspNetCore.:::no-loc(Identity):::.PasswordHasherOptions.IterationCount> `12000` в `Startup.ConfigureServices` :

```csharp
// using Microsoft.AspNetCore.:::no-loc(Identity):::;

services.Configure<PasswordHasherOptions>(option =>
{
    option.IterationCount = 12000;
});
```

## <a name="globally-require-all-users-to-be-authenticated"></a>Глобально требовать проверку подлинности всех пользователей

[!INCLUDE[](~/includes/requireAuth.md)]