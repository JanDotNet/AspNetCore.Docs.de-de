---
title: Fehlerbehandlung in ASP.NET Core-Web-APIs
author: pranavkm
description: Erfahren Sie, wie Sie Fehler in ASP.NET Core-Web-APIs beheben können.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/27/2019
uid: web-api/handle-errors
ms.openlocfilehash: dc21d4b2cf096b8d38b0a24d739e6874186004e7
ms.sourcegitcommit: 5d25a7f22c50ca6fdd0f8ecd8e525822e1b35b7a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2019
ms.locfileid: "71551742"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="b9d55-103">Fehlerbehandlung in ASP.NET Core-Web-APIs</span><span class="sxs-lookup"><span data-stu-id="b9d55-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="b9d55-104">Dieser Artikel beschreibt die Verarbeitung und Anpassung der Fehlerbehandlung bei ASP.NET Core-Web-APIs.</span><span class="sxs-lookup"><span data-stu-id="b9d55-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

<span data-ttu-id="b9d55-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b9d55-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/handle-errors/samples) ([How to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="b9d55-106">Seite mit Ausnahmen für Entwickler</span><span class="sxs-lookup"><span data-stu-id="b9d55-106">Developer Exception Page</span></span>

<span data-ttu-id="b9d55-107">Die [Seite mit Ausnahmen für Entwickler](xref:fundamentals/error-handling) ist ein nützliches Tool, um detaillierte Stapelüberwachungen für Serverfehler zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="b9d55-107">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span> <span data-ttu-id="b9d55-108">Es verwendet <xref:Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware>, um synchrone und asynchrone Ausnahmen aus der HTTP-Pipeline zu erfassen und um Fehlerantworten zu generieren.</span><span class="sxs-lookup"><span data-stu-id="b9d55-108">It uses <xref:Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware> to capture synchronous and asynchronous exceptions from the HTTP pipeline and to generate error responses.</span></span> <span data-ttu-id="b9d55-109">Betrachten Sie zur Darstellung die folgende Controlleraktion:</span><span class="sxs-lookup"><span data-stu-id="b9d55-109">To illustrate, consider the following controller action:</span></span>

[!code-csharp[](handle-errors/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_GetByCity)]

<span data-ttu-id="b9d55-110">Führen Sie den folgenden `curl`-Befehl aus, um die vorherige Aktion zu testen:</span><span class="sxs-lookup"><span data-stu-id="b9d55-110">Run the following `curl` command to test the preceding action:</span></span>

```bash
curl -i https://localhost:5001/weatherforecast/chicago
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b9d55-111">Die Seite mit Ausnahmen für Entwickler zeigt in ASP.NET Core 3.0 und später eine Nur-Text-Antwort an, wenn der Client keine HTML-formatierte Ausgabe anfordert.</span><span class="sxs-lookup"><span data-stu-id="b9d55-111">In ASP.NET Core 3.0 and later, the Developer Exception Page displays a plain-text response if the client doesn't request HTML-formatted output.</span></span> <span data-ttu-id="b9d55-112">Die folgende Ausgabe wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b9d55-112">The following output appears:</span></span>

```console
HTTP/1.1 500 Internal Server Error
Transfer-Encoding: chunked
Content-Type: text/plain
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
Date: Fri, 27 Sep 2019 16:13:16 GMT

System.ArgumentException: We don't offer a weather forecast for chicago. (Parameter 'city')
   at WebApiSample.Controllers.WeatherForecastController.Get(String city) in C:\working_folder\aspnet\AspNetCore.Docs\aspnetcore\web-api\handle-errors\samples\3.x\Controllers\WeatherForecastController.cs:line 34
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ActionMethodExecutor.SyncObjectResultExecutor.Execute(IActionResultTypeMapper mapper, ObjectMethodExecutor executor, Object controller, Object[] arguments)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeActionMethodAsync>g__Logged|12_1(ControllerActionInvoker invoker)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeNextActionFilterAsync>g__Awaited|10_0(ControllerActionInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Rethrow(ActionExecutedContextSealed context)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Next(State& next, Scope& scope, Object& state, Boolean& isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.InvokeInnerFilterAsync()
--- End of stack trace from previous location where exception was thrown ---
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeFilterPipelineAsync>g__Awaited|19_0(ResourceInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeAsync>g__Logged|17_1(ResourceInvoker invoker)
   at Microsoft.AspNetCore.Routing.EndpointMiddleware.<Invoke>g__AwaitRequestTask|6_0(Endpoint endpoint, Task requestTask, ILogger logger)
   at Microsoft.AspNetCore.Authorization.AuthorizationMiddleware.Invoke(HttpContext context)
   at Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware.Invoke(HttpContext context)

HEADERS
=======
Accept: */*
Host: localhost:44312
User-Agent: curl/7.55.1
```

<span data-ttu-id="b9d55-113">Legen Sie den HTTP-Anforderungsheader `Accept` auf den Medientyp `text/html` fest, um stattdessen eine HTML-formatierte Antwort anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b9d55-113">To display an HTML-formatted response instead, set the `Accept` HTTP request header to the `text/html` media type.</span></span> <span data-ttu-id="b9d55-114">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b9d55-114">For example:</span></span>

```bash
curl -i -H "Accept: text/html" https://localhost:5001/weatherforecast/chicago
```

<span data-ttu-id="b9d55-115">Beachten Sie den folgenden Auszug aus der HTTP-Antwort:</span><span class="sxs-lookup"><span data-stu-id="b9d55-115">Consider the following excerpt from the HTTP response:</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="b9d55-116">In ASP.NET Core 2.2 und früher wird auf der Seite mit Ausnahmen für Entwickler eine HTML-formatierte Antwort angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b9d55-116">In ASP.NET Core 2.2 and earlier, the Developer Exception Page displays an HTML-formatted response.</span></span> <span data-ttu-id="b9d55-117">Beachten Sie beispielsweise den folgenden Auszug aus der HTTP-Antwort:</span><span class="sxs-lookup"><span data-stu-id="b9d55-117">For example, consider the following excerpt from the HTTP response:</span></span>

::: moniker-end

```console
HTTP/1.1 500 Internal Server Error
Transfer-Encoding: chunked
Content-Type: text/html; charset=utf-8
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
Date: Fri, 27 Sep 2019 16:55:37 GMT

<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta charset="utf-8" />
        <title>Internal Server Error</title>
        <style>
            body {
    font-family: 'Segoe UI', Tahoma, Arial, Helvetica, sans-serif;
    font-size: .813em;
    color: #222;
    background-color: #fff;
}
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b9d55-118">Die HTML-formatierte Antwort wird beim Durchführen von Tests in Tools wie etwa Postman nützlich.</span><span class="sxs-lookup"><span data-stu-id="b9d55-118">The HTML-formatted response becomes useful when testing via tools like Postman.</span></span> <span data-ttu-id="b9d55-119">Auf der folgenden Bildschirmaufnahme sind jeweils die in Klartext verfassten und die HTML-formatierte Antworten in Postman dargestellt:</span><span class="sxs-lookup"><span data-stu-id="b9d55-119">The following screen capture shows both the plain-text and the HTML-formatted responses in Postman:</span></span>

![Testen auf der Seite mit Ausnahmen für Entwickler in Postman](handle-errors/_static/developer-exception-page-postman.gif)

::: moniker-end

> [!WARNING]
> <span data-ttu-id="b9d55-121">Aktivieren Sie die Seite mit Ausnahmen für Entwickler **nur dann, wenn die App in der Entwicklungsumgebung ausgeführt wird**.</span><span class="sxs-lookup"><span data-stu-id="b9d55-121">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="b9d55-122">Wenn die App in der Produktionsumgebung ausgeführt wird, sollten Sie keine detaillierten Ausnahmeinformationen öffentlich teilen.</span><span class="sxs-lookup"><span data-stu-id="b9d55-122">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="b9d55-123">Weitere Informationen zum Konfigurieren der Umgebungen finden Sie unter <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="b9d55-123">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="b9d55-124">Ausnahmehandler</span><span class="sxs-lookup"><span data-stu-id="b9d55-124">Exception handler</span></span>

<span data-ttu-id="b9d55-125">In nicht zur Entwicklung dienenden Umgebungen kann [Middleware zur Ausnahmebehandlung](xref:fundamentals/error-handling) verwendet werden, um eine Fehlernutzlast zu erzeugen:</span><span class="sxs-lookup"><span data-stu-id="b9d55-125">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="b9d55-126">Rufen Sie <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> in `Startup.Configure` auf, um die Middleware zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="b9d55-126">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_UseExceptionHandler&highlight=9)]

    ::: moniker-end

1. <span data-ttu-id="b9d55-127">Konfigurieren Sie eine Controlleraktion, um auf die `/error`-Route zu antworten:</span><span class="sxs-lookup"><span data-stu-id="b9d55-127">Configure a controller action to respond to the `/error` route:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorController)]

    ::: moniker-end

<span data-ttu-id="b9d55-128">Die oben stehende `Error`-Aktion sendet eine mit [RFC7807](https://tools.ietf.org/html/rfc7807) kompatible Nutzlast an den Client.</span><span class="sxs-lookup"><span data-stu-id="b9d55-128">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="b9d55-129">Middleware zur Ausnahmebehandlung kann auch eine detailliertere Ausgabe der Inhaltsaushandlung in der lokalen Entwicklungsumgebung bieten.</span><span class="sxs-lookup"><span data-stu-id="b9d55-129">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="b9d55-130">Führen Sie die folgenden Schritte aus, um für ein konsistentes Nutzlastformat in Entwicklungs- und Produktionsumgebungen zu sorgen:</span><span class="sxs-lookup"><span data-stu-id="b9d55-130">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="b9d55-131">Registrieren Sie umgebungsspezifische Middleware zur Ausnahmebehandlung in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="b9d55-131">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    ::: moniker-end

    <span data-ttu-id="b9d55-132">Im oben stehenden Code ist die Middleware mit Folgendem registriert:</span><span class="sxs-lookup"><span data-stu-id="b9d55-132">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="b9d55-133">Route `/error-local-development` in der Entwicklungsumgebung.</span><span class="sxs-lookup"><span data-stu-id="b9d55-133">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="b9d55-134">Route `/error` in einer nicht zur Entwicklung dienenden Umgebung.</span><span class="sxs-lookup"><span data-stu-id="b9d55-134">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="b9d55-135">Wenden Sie das Attributrouting auf Controlleraktionen an:</span><span class="sxs-lookup"><span data-stu-id="b9d55-135">Apply attribute routing to controller actions:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

    ::: moniker range="<= aspnetcore-2.2"

    [!code-csharp[](handle-errors/samples/2.x/2.2/Controllers/ErrorController.cs?name=snippet_ErrorControllerEnvironmentSpecific)]

    ::: moniker-end

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="b9d55-136">Verwenden von Ausnahmen zum Ändern der Antwort</span><span class="sxs-lookup"><span data-stu-id="b9d55-136">Use exceptions to modify the response</span></span>

<span data-ttu-id="b9d55-137">Der Inhalt der Antwort kann außerhalb des Controllers geändert werden.</span><span class="sxs-lookup"><span data-stu-id="b9d55-137">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="b9d55-138">In der ASP.NET 4.x-Web-API war die einzige Möglichkeit hierfür die Verwendung des Typs <xref:System.Web.Http.HttpResponseException>.</span><span class="sxs-lookup"><span data-stu-id="b9d55-138">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="b9d55-139">ASP.NET Core enthält keinen entsprechenden Typ.</span><span class="sxs-lookup"><span data-stu-id="b9d55-139">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="b9d55-140">Die Unterstützung für `HttpResponseException` kann mit folgenden Schritten hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="b9d55-140">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="b9d55-141">Erstellen Sie einen bekannten Ausnahmetyp namens `HttpResponseException`:</span><span class="sxs-lookup"><span data-stu-id="b9d55-141">Create a well-known exception type named `HttpResponseException`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Exceptions/HttpResponseException.cs?name=snippet_HttpResponseException)]

1. <span data-ttu-id="b9d55-142">Erstellen Sie einen Aktionsfilter namens `HttpResponseExceptionFilter`:</span><span class="sxs-lookup"><span data-stu-id="b9d55-142">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    [!code-csharp[](handle-errors/samples/3.x/Filters/HttpResponseExceptionFilter.cs?name=snippet_HttpResponseExceptionFilter)]

1. <span data-ttu-id="b9d55-143">Fügen Sie den Aktionsfilter in `Startup.ConfigureServices` der Filtersammlung hinzu:</span><span class="sxs-lookup"><span data-stu-id="b9d55-143">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ::: moniker range=">= aspnetcore-3.0"

    [!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.2"
    
    [!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

    ::: moniker range="= aspnetcore-2.1"

    [!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_AddExceptionFilter)]

    ::: moniker-end

## <a name="validation-failure-error-response"></a><span data-ttu-id="b9d55-144">Antwort auf Überprüfungsfehler</span><span class="sxs-lookup"><span data-stu-id="b9d55-144">Validation failure error response</span></span>

<span data-ttu-id="b9d55-145">Bei Web-API-Controllern antwortet MVC mit dem Antworttyp <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>, wenn bei der Modellüberprüfung ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="b9d55-145">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="b9d55-146">MVC verwendet die Ergebnisse von <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>, um die Fehlerantwort auf einen Überprüfungsfehler zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b9d55-146">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="b9d55-147">Das folgende Beispiel verwendet die Factory, um den Standardantworttyp von <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices` zu ändern:</span><span class="sxs-lookup"><span data-stu-id="b9d55-147">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](handle-errors/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](handle-errors/samples/2.x/2.2/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](handle-errors/samples/2.x/2.1/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=6-15)]

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="b9d55-148">Fehlerantwort des Clients</span><span class="sxs-lookup"><span data-stu-id="b9d55-148">Client error response</span></span>

<span data-ttu-id="b9d55-149">Ein *Fehlerergebnis* ist als Ergebnis mit dem HTTP-Statuscode 400 oder höher definiert.</span><span class="sxs-lookup"><span data-stu-id="b9d55-149">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="b9d55-150">Bei Web-API-Controllern transformiert MVC ein Fehlerergebnis in ein Ergebnis mit <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="b9d55-150">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b9d55-151">Die Fehlerantwort kann auf eine der folgenden Arten konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="b9d55-151">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="b9d55-152">Implementieren der ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="b9d55-152">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="b9d55-153">Verwenden von ApiBehaviorOptions.ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="b9d55-153">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="b9d55-154">Implementieren der ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="b9d55-154">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="b9d55-155">MVC verwendet die `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory`, um alle Instanzen von <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> und <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> zu generieren.</span><span class="sxs-lookup"><span data-stu-id="b9d55-155">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="b9d55-156">Hierzu gehören Clientfehlerantworten, Antworten auf Überprüfungsfehler sowie die Hilfsmethoden `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` und <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem>.</span><span class="sxs-lookup"><span data-stu-id="b9d55-156">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="b9d55-157">Um die Antwort mit Problemdetails anzupassen, registrieren Sie eine benutzerdefinierte Implementierung von `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b9d55-157">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="b9d55-158">Die Fehlerantwort kann so konfiguriert werden wie im Abschnitt [Verwenden von ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="b9d55-158">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="b9d55-159">Verwenden von ApiBehaviorOptions.ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="b9d55-159">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="b9d55-160">Verwenden Sie die <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*>-Eigenschaft zum Konfigurieren des Inhalts der `ProblemDetails`-Antwort.</span><span class="sxs-lookup"><span data-stu-id="b9d55-160">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="b9d55-161">Der folgende Code in `Startup.ConfigureServices` aktualisiert beispielsweise die `type`-Eigenschaft für 404-Antworten:</span><span class="sxs-lookup"><span data-stu-id="b9d55-161">For example, the following code in `Startup.ConfigureServices` updates the `type` property for 404 responses:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end