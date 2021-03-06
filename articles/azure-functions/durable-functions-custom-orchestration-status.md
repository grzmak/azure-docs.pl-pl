---
title: Stan niestandardowej aranżacji w funkcje trwałe - Azure
description: Dowiedz się, jak skonfigurować i stan niestandardowej aranżacji na użytek funkcje trwałe.
services: functions
author: kadimitr
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: azfuncdf
ms.openlocfilehash: c8eb2be6836e11ddbaed81970024ea7200ea819d
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093095"
---
# <a name="custom-orchestration-status-in-durable-functions-azure-functions"></a>Stan niestandardowej aranżacji w funkcje trwałe (usługi Azure Functions)

Stan niestandardowej aranżacji pozwala ustawić wartość stanu niestandardowych dla funkcji programu orchestrator. Ten stan jest oferowana w ramach interfejsu API GetStatus protokołu HTTP lub `DurableOrchestrationClient.GetStatusAsync` interfejsu API.

## <a name="sample-use-cases"></a>Przykładowe przypadki użycia 

### <a name="visualize-progress"></a>Wizualizacja postępu

Klienci mogą sondować stan punktu końcowego i wyświetlanie postępu interfejsu użytkownika, która wizualizuje bieżący etap wykonywania. W poniższym przykładzie pokazano udostępnianie postępu:

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  var outputs = new List<string>();

  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
  context.SetCustomStatus("Tokyo");
  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
  context.SetCustomStatus("Seattle");
  outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));
  context.SetCustomStatus("London");

  // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
  return outputs;
}

[FunctionName("E1_SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
  return $"Hello {name}!";
}
```

A następnie klient otrzyma dane wyjściowe aranżacji tylko wtedy, gdy `CustomStatus` pole jest ustawione na "Londyn":

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
  [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
  [OrchestrationClient] DurableOrchestrationClientBase starter,
  string functionName,
  ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    DurableOrchestrationStatus durableOrchestrationStatus = await starter.GetStatusAsync(instanceId);
    while (durableOrchestrationStatus.CustomStatus.ToString() != "London")
    {
      await Task.Delay(200);
      durableOrchestrationStatus = await starter.GetStatusAsync(instanceId);
    }

    HttpResponseMessage httpResponseMessage = new HttpResponseMessage(HttpStatusCode.OK)
    {
      Content = new StringContent(JsonConvert.SerializeObject(durableOrchestrationStatus))
    };

    return httpResponseMessage;
  }
}
```

### <a name="output-customization"></a>Dostosowywanie danych wyjściowych 

Inny scenariusz interesujące jest segmentacji użytkowników, zwracając dostosowanych danych wyjściowych na podstawie unikatowych cechach systemu operacyjnego lub interakcji. Za pomocą stan niestandardowej aranżacji pozostaną ogólny kod po stronie klienta. Wszystkie modyfikacje głównego nastąpi po stronie serwera, jak pokazano w następującym przykładzie:

```csharp
[FunctionName("CityRecommender")]
public static void Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  int userChoice = context.GetInput<int>();

  switch (userChoice)
  {
    case 1:
    context.SetCustomStatus(new
    {
      recommendedCities = new[] {"Tokyo", "Seattle"},
      recommendedSeasons = new[] {"Spring", "Summer"}
     });
      break;
    case 2:
      context.SetCustomStatus(new
      {
        recommendedCity = new[] {"Seattle, London"},
        recommendedSeasons = new[] {"Summer"}
      });
        break;
      case 3:
      context.SetCustomStatus(new
      {
        recommendedCity = new[] {"Tokyo, London"},
        recommendedSeasons = new[] {"Spring", "Summer"}
      });
        break;
  }

  // Wait for user selection and refine the recommendation
} 
```

### <a name="instruction-specification"></a>Specyfikacja instrukcji 

Koordynatora może zapewnić instrukcje unikatowy dla klientów za pośrednictwem stanów niestandardowych. Instrukcje stan niestandardowego będą mapowane z procedury opisanej w kodzie orchestration:

```csharp
[FunctionName("ReserveTicket")]
public static async Task<bool> Run(
  [OrchestrationTrigger] DurableOrchestrationContextBase context)
{
  string userId = context.GetInput<string>();

  int discount = await context.CallActivityAsync<int>("CalculateDiscount", userId);

  context.SetCustomStatus(new
  {
    discount = discount,
    discountTimeout = 60,
    bookingUrl = "https://www.myawesomebookingweb.com",
  });

  bool isBookingConfirmed = await context.WaitForExternalEvent<bool>("BookingConfirmed");

  context.SetCustomStatus(isBookingConfirmed
    ? new {message = "Thank you for confirming your booking."}
    : new {message = "The booking was not confirmed on time. Please try again."});

  return isBookingConfirmed;
}
```

## <a name="sample"></a>Sample

W poniższym przykładzie ustawiono najpierw; stan niestandardowego

```csharp
public static async Task SetStatusTest([OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    var customStatus = new { nextActions = new [] {"A", "B", "C"}, foo = 2, };
    ctx.SetCustomStatus(customStatus);

    // ...do more work...
}
```

Po uruchomieniu aranżacji klientów zewnętrznych można pobrać ten stan niestandardowy:

```http
GET /admin/extensions/DurableTaskExtension/instances/instance123

```

Klienci otrzymają następującą odpowiedź: 

```http
{
  "runtimeStatus": "Running",
  "input": null,
  "customStatus": { "nextActions": ["A", "B", "C"], "foo": 2 },
  "output": null,
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T19:40:30Z"
}
```

> [!WARNING]
>  Stan niestandardowy ładunek jest ograniczony do 16 KB tekstu JSON UTF-16, ponieważ wymagane jest, aby mieściły się w kolumnie Azure Table Storage. Deweloperzy mogą używać magazynu zewnętrznego, gdy potrzebują większych ładunku.


## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Więcej informacji na temat interfejsów API protokołu HTTP w funkcje trwałe](durable-functions-http-api.md)


