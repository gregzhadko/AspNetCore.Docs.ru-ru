---
title: DevOps с помощью ASP.NET Core и Azure | Мониторинг и отладка
author: CamSoper
description: Рекомендации по созданию сквозного решения конвейера DevOps для приложения ASP.NET Core, размещенного в Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/monitor
ms.openlocfilehash: c2fc88493aee04d7ea2781d17e808581e89d2082
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2018
ms.locfileid: "42909606"
---
# <a name="monitor-and-debug"></a><span data-ttu-id="6ba4e-103">Мониторинг и отладка</span><span class="sxs-lookup"><span data-stu-id="6ba4e-103">Monitor and debug</span></span>

<span data-ttu-id="6ba4e-104">Произведя развертывание приложения и построение конвейера DevOps, важно понять, как выполнять мониторинг и устранение неполадок приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-104">Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.</span></span>

<span data-ttu-id="6ba4e-105">В этом разделе вы выполните следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="6ba4e-105">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="6ba4e-106">Найти базовый мониторинг и устранение неполадок данных на портале Azure</span><span class="sxs-lookup"><span data-stu-id="6ba4e-106">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="6ba4e-107">Узнайте, как Azure Monitor обеспечивает более подробное рассмотрение метрики во всех службах Azure</span><span class="sxs-lookup"><span data-stu-id="6ba4e-107">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="6ba4e-108">Подключение веб-приложения с помощью Application Insights для профилирования приложения</span><span class="sxs-lookup"><span data-stu-id="6ba4e-108">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="6ba4e-109">Включите ведение журнала и узнать, где можно скачать журналы</span><span class="sxs-lookup"><span data-stu-id="6ba4e-109">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="6ba4e-110">Stream журналами в реальном времени</span><span class="sxs-lookup"><span data-stu-id="6ba4e-110">Stream logs in real time</span></span>
* <span data-ttu-id="6ba4e-111">Узнать, где можно настроить оповещения</span><span class="sxs-lookup"><span data-stu-id="6ba4e-111">Learn where to set up alerts</span></span>
* <span data-ttu-id="6ba4e-112">Дополнительные сведения об удаленной отладки веб-приложений службы приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-112">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="basic-monitoring-and-troubleshooting"></a><span data-ttu-id="6ba4e-113">Базовый мониторинг и устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="6ba4e-113">Basic monitoring and troubleshooting</span></span>

<span data-ttu-id="6ba4e-114">Веб-приложениях службы приложений, легко отслеживаются в реальном времени.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-114">App Service web apps are easily monitored in real time.</span></span> <span data-ttu-id="6ba4e-115">Портал Azure отображает метрики в простой для понимания диаграмм и графиков.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-115">The Azure portal renders metrics in easy-to-understand charts and graphs.</span></span>

1. <span data-ttu-id="6ba4e-116">Откройте [портала Azure](https://portal.azure.com), а затем перейдите к *mywebapp\<unique_number\>*  службы приложений.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-116">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>

1. <span data-ttu-id="6ba4e-117">**Обзор** вкладка отображает полезные сведения «в краткие», включая диаграммы, отображения последних метрик.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-117">The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.</span></span>

    ![Панель "Обзор"](./media/monitoring/overview.png)

    * <span data-ttu-id="6ba4e-119">**HTTP 5xx**: число ошибок на стороне сервера, обычно исключения в коде ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-119">**Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.</span></span>
    * <span data-ttu-id="6ba4e-120">**Данные в**: объем входящих данных, поступающих на веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-120">**Data In**: Data ingress coming into your web app.</span></span>
    * <span data-ttu-id="6ba4e-121">**Исходящие данные**: данных исходящий трафик от веб-приложения на клиентах.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-121">**Data Out**: Data egress from your web app to clients.</span></span>
    * <span data-ttu-id="6ba4e-122">**Запросы**: число HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-122">**Requests**: Count of HTTP requests.</span></span>
    * <span data-ttu-id="6ba4e-123">**Среднее время ответа**: среднее время для веб-приложения отвечать на запросы HTTP.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-123">**Average Response Time**: Average time for the web app to respond to HTTP requests.</span></span>

    <span data-ttu-id="6ba4e-124">Несколько средств самообслуживания, для устранения неполадок и оптимизации, можно также найти на этой странице.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-124">Several self-service tools for troubleshooting and optimization are also found on this page.</span></span>

    ![Средства самообслуживания](./media/monitoring/wizards.png)

    * <span data-ttu-id="6ba4e-126">**Диагностика и устранение неполадок** — это средство самостоятельного устранения неполадок.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-126">**Diagnose and solve problems** is a self-service troubleshooter.</span></span>
    * <span data-ttu-id="6ba4e-127">**Application Insights** — для профилирования производительности и поведение приложения и рассматривается далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-127">**Application Insights** is for profiling performance and app behavior, and is discussed later in this section.</span></span>
    * <span data-ttu-id="6ba4e-128">**Помощник по службе приложений** делает рекомендации для настройки работы приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-128">**App Service Advisor** makes recommendations to tune your app experience.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="6ba4e-129">Расширенный мониторинг</span><span class="sxs-lookup"><span data-stu-id="6ba4e-129">Advanced monitoring</span></span>

<span data-ttu-id="6ba4e-130">[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) — централизованная служба для всех метрик мониторинга и настройки оповещений между службами Azure.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-130">[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services.</span></span> <span data-ttu-id="6ba4e-131">В Azure Monitor администраторы могут детально отслеживать производительность и определять тенденции.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-131">Within Azure Monitor, administrators can granularly track performance and identify trends.</span></span> <span data-ttu-id="6ba4e-132">Каждая служба Azure предлагает собственную [набор метрик](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) в Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-132">Each Azure service offers its own [set of metrics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.</span></span>

## <a name="profile-with-application-insights"></a><span data-ttu-id="6ba4e-133">Профиль с помощью Application Insights</span><span class="sxs-lookup"><span data-stu-id="6ba4e-133">Profile with Application Insights</span></span>

<span data-ttu-id="6ba4e-134">[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) — это служба Azure для анализа производительности и стабильности веб-приложений и как их использовать пользователи.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-134">[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them.</span></span> <span data-ttu-id="6ba4e-135">Данные из Application Insights — исследование, отличный от Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-135">The data from Application Insights is broader and deeper than that of Azure Monitor.</span></span> <span data-ttu-id="6ba4e-136">Данные можно предоставить, разработчики и Администраторы получают необходимую информацию для улучшения приложений.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-136">The data can provide developers and administrators with key information for improving apps.</span></span> <span data-ttu-id="6ba4e-137">Можно добавить Application Insights для ресурса службы приложений Azure без изменения кода.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-137">Application Insights can be added to an Azure App Service resource without code changes.</span></span>

1. <span data-ttu-id="6ba4e-138">Откройте [портала Azure](https://portal.azure.com), а затем перейдите к *mywebapp\<unique_number\>*  службы приложений.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-138">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="6ba4e-139">Из **Обзор** щелкните **Application Insights** плитку.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-139">From the **Overview** tab, click the **Application Insights** tile.</span></span>

    ![Плитку службы Application Insights](./media/monitoring/app-insights.png)

1. <span data-ttu-id="6ba4e-141">Выберите **создать новый ресурс** "переключатель".</span><span class="sxs-lookup"><span data-stu-id="6ba4e-141">Select the **Create new resource** radio button.</span></span> <span data-ttu-id="6ba4e-142">Используемое имя ресурса по умолчанию и выберите расположение для ресурса Application Insights.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-142">Use the default resource name, and select the location for the Application Insights resource.</span></span> <span data-ttu-id="6ba4e-143">Расположение не обязательно должен соответствовать, веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-143">The location doesn't need to match that of your web app.</span></span>

    ![Программа установки Application Insights](./media/monitoring/new-app-insights.png)

1. <span data-ttu-id="6ba4e-145">Для **среда выполнения или платформа**выберите **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-145">For **Runtime/Framework**, select **ASP.NET Core**.</span></span> <span data-ttu-id="6ba4e-146">Примите параметры по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-146">Accept the default settings.</span></span>
1. <span data-ttu-id="6ba4e-147">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-147">Select **OK**.</span></span> <span data-ttu-id="6ba4e-148">Если запрос на подтверждение, установите **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-148">If prompted to confirm, select **Continue**.</span></span>
1. <span data-ttu-id="6ba4e-149">После создания ресурса, щелкните имя ресурса Application Insights, чтобы перейти непосредственно к странице Application Insights.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-149">After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.</span></span>

    ![Новый ресурс Application Insights будет готов](./media/monitoring/new-app-insights-done.png)

<span data-ttu-id="6ba4e-151">Так как используется приложение, данные накапливаются.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-151">As the app is used, data accumulates.</span></span> <span data-ttu-id="6ba4e-152">Выберите **обновить** перезагрузить в колонке с новыми данными.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-152">Select **Refresh** to reload the blade with new data.</span></span>

![Вкладка обзора Application Insights](./media/monitoring/app-insights-overview.png)

<span data-ttu-id="6ba4e-154">Application Insights предоставляет полезную информацию на сервере без дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-154">Application Insights provides useful server-side information with no additional configuration.</span></span> <span data-ttu-id="6ba4e-155">Чтобы получить максимальную пользу из Application Insights, [инструментирование приложения с помощью пакета SDK Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="6ba4e-155">To get the most value from Application Insights, [instrument your app with the Application Insights SDK](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core).</span></span> <span data-ttu-id="6ba4e-156">При правильной настройке эта служба предоставляет end-to-end monitoring в веб-сервера и браузера, включая производительности на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-156">When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance.</span></span> <span data-ttu-id="6ba4e-157">Дополнительные сведения см. в разделе [документация по Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="6ba4e-157">For more information, see the [Application Insights documentation](https://docs.microsoft.com/azure/application-insights/app-insights-overview).</span></span>

## <a name="logging"></a><span data-ttu-id="6ba4e-158">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="6ba4e-158">Logging</span></span>

<span data-ttu-id="6ba4e-159">Веб-журналы сервера и приложения будут отключены по умолчанию в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-159">Web server and app logs are disabled by default in Azure App Service.</span></span> <span data-ttu-id="6ba4e-160">Включите журналы, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-160">Enable the logs with the following steps:</span></span>

1. <span data-ttu-id="6ba4e-161">Откройте [портала Azure](https://portal.azure.com)и перейдите к *mywebapp\<unique_number\>*  службы приложений.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-161">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="6ba4e-162">В меню слева, прокрутите вниз до раздела **мониторинг** раздел.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-162">In the menu to the left, scroll down to the **Monitoring** section.</span></span> <span data-ttu-id="6ba4e-163">Выберите **журналы диагностики**.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-163">Select **Diagnostics logs**.</span></span>

    ![Ссылка на журналы диагностики](./media/monitoring/logging.png)

1. <span data-ttu-id="6ba4e-165">Включите **ведение журнала приложения (файловая система)**.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-165">Turn on **Application Logging (Filesystem)**.</span></span> <span data-ttu-id="6ba4e-166">При появлении соответствующего запроса, щелкните окно, чтобы установить расширения, чтобы позволить приложению, ведение журнала в веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-166">If prompted, click the box to install the extensions to enable app logging in the web app.</span></span>
1. <span data-ttu-id="6ba4e-167">Задайте **журналы веб-сервера** для **файловой системы**.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-167">Set **Web server logging** to **File System**.</span></span>
1. <span data-ttu-id="6ba4e-168">Введите **срок** в днях.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-168">Enter the **Retention Period** in days.</span></span> <span data-ttu-id="6ba4e-169">Например, 30.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-169">For example, 30.</span></span>
1. <span data-ttu-id="6ba4e-170">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-170">Click **Save**.</span></span>

<span data-ttu-id="6ba4e-171">ASP.NET Core и веб-сервер (служба приложений) журналы создаются для веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-171">ASP.NET Core and web server (App Service) logs are generated for the web app.</span></span> <span data-ttu-id="6ba4e-172">Их можно загрузить с помощью FTP или FTPS информация.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-172">They can be downloaded using the FTP/FTPS information displayed.</span></span> <span data-ttu-id="6ba4e-173">Пароль совпадает со значением учетные данные развертывания, созданную ранее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-173">The password is the same as the deployment credentials created earlier in this guide.</span></span> <span data-ttu-id="6ba4e-174">Журналы могут быть [потоковом режиме непосредственно на локальном компьютере с помощью PowerShell или интерфейса командной строки Azure](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#download).</span><span class="sxs-lookup"><span data-stu-id="6ba4e-174">The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#download).</span></span> <span data-ttu-id="6ba4e-175">Журналы также может быть [просмотреть в Application Insights](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span><span class="sxs-lookup"><span data-stu-id="6ba4e-175">Logs can also be [viewed in Application Insights](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span></span>

## <a name="log-streaming"></a><span data-ttu-id="6ba4e-176">Потоковая передача журналов</span><span class="sxs-lookup"><span data-stu-id="6ba4e-176">Log streaming</span></span>

<span data-ttu-id="6ba4e-177">Приложения и веб-журналы сервера может передаваться в режиме реального времени на портале.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-177">App and web server logs can be streamed in real time through the portal.</span></span>

1. <span data-ttu-id="6ba4e-178">Откройте [портала Azure](https://portal.azure.com)и перейдите к *mywebapp\<unique_number\>*  службы приложений.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-178">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="6ba4e-179">В меню слева, прокрутите вниз до раздела **мониторинг** и выберите **поток журналов**.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-179">In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.</span></span>

    ![Ссылка журнала потока](./media/monitoring/log-stream.png)

<span data-ttu-id="6ba4e-181">Журналы также может быть [потоковую передачу с помощью Azure CLI или Azure PowerShell](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), в том числе через Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-181">Logs can also be [streamed via Azure CLI or Azure PowerShell](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.</span></span>

## <a name="alerts"></a><span data-ttu-id="6ba4e-182">Предупреждения</span><span class="sxs-lookup"><span data-stu-id="6ba4e-182">Alerts</span></span>

<span data-ttu-id="6ba4e-183">Azure Monitor также предоставляет [оповещения в режиме реального времени](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal) на основе метрик, административные события и другим критериям.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-183">Azure Monitor also provides [real time alerts](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.</span></span>

> <span data-ttu-id="6ba4e-184">*Примечание: В настоящее время оповещения о показателей веб-приложения доступен только в службе оповещений (Классическая модель).*</span><span class="sxs-lookup"><span data-stu-id="6ba4e-184">*Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*</span></span>

<span data-ttu-id="6ba4e-185">[Оповещений (Классическая) служба](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) можно найти в Azure Monitor или в разделе **мониторинг** раздел параметров приложения службы.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-185">The [Alerts (classic) service](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.</span></span>

![Ссылка на уведомления (классический)](./media/monitoring/alerts.png)

## <a name="live-debugging"></a><span data-ttu-id="6ba4e-187">Динамическая Отладка</span><span class="sxs-lookup"><span data-stu-id="6ba4e-187">Live debugging</span></span>

<span data-ttu-id="6ba4e-188">Служба приложений Azure может быть [отлаживать удаленно с помощью Visual Studio](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) когда журналы не предоставляли достаточно информации.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-188">Azure App Service can be [debugged remotely with Visual Studio](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information.</span></span> <span data-ttu-id="6ba4e-189">Тем не менее для удаленной отладки требуется приложение компилируется с отладочными символами.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-189">However, remote debugging requires the app to be compiled with debug symbols.</span></span> <span data-ttu-id="6ba4e-190">Отладка не должно выполняться в рабочей среде, за исключением в качестве последнего средства.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-190">Debugging shouldn't be done in production, except as a last resort.</span></span>

## <a name="conclusion"></a><span data-ttu-id="6ba4e-191">Заключение</span><span class="sxs-lookup"><span data-stu-id="6ba4e-191">Conclusion</span></span>

<span data-ttu-id="6ba4e-192">В этом разделе вы выполнили следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="6ba4e-192">In this section, you completed the following tasks:</span></span>

* <span data-ttu-id="6ba4e-193">Найти базовый мониторинг и устранение неполадок данных на портале Azure</span><span class="sxs-lookup"><span data-stu-id="6ba4e-193">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="6ba4e-194">Узнайте, как Azure Monitor обеспечивает более подробное рассмотрение метрики во всех службах Azure</span><span class="sxs-lookup"><span data-stu-id="6ba4e-194">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="6ba4e-195">Подключение веб-приложения с помощью Application Insights для профилирования приложения</span><span class="sxs-lookup"><span data-stu-id="6ba4e-195">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="6ba4e-196">Включите ведение журнала и узнать, где можно скачать журналы</span><span class="sxs-lookup"><span data-stu-id="6ba4e-196">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="6ba4e-197">Stream журналами в реальном времени</span><span class="sxs-lookup"><span data-stu-id="6ba4e-197">Stream logs in real time</span></span>
* <span data-ttu-id="6ba4e-198">Узнать, где можно настроить оповещения</span><span class="sxs-lookup"><span data-stu-id="6ba4e-198">Learn where to set up alerts</span></span>
* <span data-ttu-id="6ba4e-199">Дополнительные сведения об удаленной отладки веб-приложений службы приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="6ba4e-199">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="6ba4e-200">Дополнительные материалы для чтения</span><span class="sxs-lookup"><span data-stu-id="6ba4e-200">Additional reading</span></span>

* [<span data-ttu-id="6ba4e-201">Устранение неполадок ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="6ba4e-201">Troubleshoot ASP.NET Core on Azure App Service</span></span>](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="6ba4e-202">Справочник по общим ошибкам для службы приложений Azure и служб IIS с ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6ba4e-202">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="6ba4e-203">Мониторинг производительности Azure веб-приложения с помощью Application Insights</span><span class="sxs-lookup"><span data-stu-id="6ba4e-203">Monitor Azure web app performance with Application Insights</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-azure-web-apps)
* [<span data-ttu-id="6ba4e-204">Включение функции ведения журналов диагностики для веб-приложений в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="6ba4e-204">Enable diagnostics logging for web apps in Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log)
* [<span data-ttu-id="6ba4e-205">Устранение неполадок веб-приложения в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ba4e-205">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="6ba4e-206">Создание классических оповещений метрик в Azure Monitor для служб Azure — портал Azure</span><span class="sxs-lookup"><span data-stu-id="6ba4e-206">Create classic metric alerts in Azure Monitor for Azure services - Azure portal</span></span>](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal)