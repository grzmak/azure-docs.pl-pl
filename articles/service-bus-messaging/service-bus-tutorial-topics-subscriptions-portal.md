---
title: Samouczek — Aktualizowanie asortymentu magazynu sklepu sieciowego przy użyciu kanałów publikowania/subskrypcji i filtrów tematów za pomocą witryny Azure Portal | Microsoft Docs
description: W tym samouczku przedstawiono, jak wysyłać i odbierać komunikaty z tematu i subskrypcji oraz jak dodawać reguły filtru i używać ich za pomocą programu .NET
services: service-bus-messaging
author: spelluru
manager: timlt
ms.author: spelluru
ms.date: 09/22/2018
ms.topic: tutorial
ms.service: service-bus-messaging
ms.custom: mvc
ms.openlocfilehash: 357bdcbbee348d8cb89e2d75060a3e7ba05e2c86
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406238"
---
# <a name="tutorial-update-inventory-using-azure-portal-and-topicssubscriptions"></a>Samouczek: aktualizowanie magazynu przy użyciu witryny Azure Portal oraz tematów/subskrypcji

Usługa Microsoft Azure Service Bus to wielodostępna usługa przesyłania komunikatów w chmurze, która przesyła informacje między aplikacjami i usługami. Operacje asynchroniczne umożliwiają elastyczne przesyłanie komunikatów obsługiwanych przez brokera oraz ustrukturyzowane przesyłanie komunikatów typu „pierwszy na wejściu — pierwszy na wyjściu” (FIFO, first-in, first-out) i zapewniają możliwości publikowania/subskrybowania. W tym samouczku przedstawiono, jak używać tematów i subskrypcji usługi Service Bus w scenariuszu obejmującym magazyn sklepu sieciowego, gdy kanały publikowania/subskrypcji korzystają z witryny Azure Portal oraz programu .NET.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Tworzenie tematu usługi Service Bus i co najmniej jednej subskrypcji do tego tematu przy użyciu witryny Azure Portal
> * Dodawanie filtrów tematu przy użyciu kodu platformy .NET
> * Tworzenie dwóch komunikatów z różną zawartością
> * Wysyłanie komunikatów oraz sprawdzanie, czy dotarły do spodziewanych subskrypcji
> * Odbieranie komunikatów z subskrypcji

Przykładem tego scenariusza jest aktualizacja asortymentu magazynu na potrzeby wielu sklepów detalicznych. W tym scenariuszu każdy sklep, lub grupa sklepów, otrzymuje komunikaty o potrzebie aktualizacji swojego asortymentu. W tym samouczku przedstawiono, jak wdrożyć ten scenariusz przy użyciu subskrypcji i filtrów. Najpierw utwórz temat z 3 subskrypcjami, dodaj jakieś reguły i filtry, a następnie wysyłaj i odbieraj komunikaty z tematu i subskrypcji.

![temat](./media/service-bus-tutorial-topics-subscriptions-portal/about-service-bus-topic.png)

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem możesz utworzyć [bezpłatne konto][].

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć kroki tego samouczka, upewnij się, że zainstalowano następujące elementy:

- [Program Visual Studio 2017 Update 3 (wersja 15.3, 26730.01)](http://www.visualstudio.com/vs) lub nowszy.
- [Zestaw NET Core SDK](https://www.microsoft.com/net/download/windows), wersja 2.0 lub nowsza.

## <a name="service-bus-topics-and-subscriptions"></a>Tematy i subskrypcje usługi Service Bus

Każda [subskrypcja tematu](service-bus-messaging-overview.md#topics) może otrzymywać kopie wszystkich komunikatów. Tematy są w pełni protokołowane i semantycznie zgodne z kolejkami usługi Service Bus. Tematy usługi Service Bus obsługują najróżniejsze reguły wyboru z warunkami filtru, z użyciem opcjonalnych akcji, które ustawiają lub modyfikują właściwości komunikatów. Za każdym razem, gdy reguła pasuje, generuje komunikat. Aby dowiedzieć się więcej o regułach, filtrach i akcjach, kliknij ten [link](topic-filters.md).

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Przejdź do witryny [Azure Portal][Azure portal] i zaloguj się przy użyciu subskrypcji platformy Azure. Pierwszym krokiem jest utworzenie przestrzeni nazw usługi Service Bus typu **Komunikaty**.

## <a name="create-a-service-bus-namespace"></a>Tworzenie przestrzeni nazw usługi Service Bus

Przestrzeń nazw obsługi komunikatów w usłudze Service Bus udostępnia unikatowy kontener zakresu przywoływany przy użyciu jego [w pełni kwalifikowanej nazwy domeny][], w którym można utworzyć co najmniej jedną kolejkę, temat i subskrypcję. W poniższym przykładzie jest tworzona przestrzeń nazw obsługi komunikatów w usłudze Service Bus w nowej lub istniejącej [grupie zasobów](/azure/azure-resource-manager/resource-group-portal):

1. W lewym okienku nawigacji portalu kliknij kolejno pozycje **+ Utwórz zasób**, **Integracja w przedsiębiorstwie** i **Service Bus**.
2. W oknie dialogowym **Tworzenie przestrzeni nazw** wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.
3. Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz warstwę cenową (Standardowa lub Premium).
4. W polu **Subskrypcja** wybierz subskrypcję platformy Azure, w której ma zostać utworzona przestrzeń nazw.
5. W polu **Grupa zasobów** wybierz istniejącą grupę zasobów, w której znajdzie się przestrzeń nazw, lub utwórz nową.      
6. W polu **Lokalizacja** wybierz kraj lub region, w którym powinna być hostowana przestrzeń nazw.
7. Kliknij pozycję **Utwórz**. W systemie zostanie utworzona i włączona przestrzeń nazw. Proces aprowizacji zasobów dla konta w systemie może potrwać kilka minut.

  ![przestrzeń nazw](./media/service-bus-tutorial-topics-subscriptions-portal/create-namespace.png)

### <a name="obtain-the-management-credentials"></a>Uzyskiwanie poświadczeń zarządzania

Utworzenie nowej przestrzeni nazw powoduje automatyczne wygenerowanie początkowej reguły sygnatury dostępu współdzielonego ze skojarzoną parą kluczy podstawowego i pomocniczego, która przyznaje pełną kontrolę nad wszystkimi aspektami przestrzeni nazw. Aby skopiować początkową regułę, wykonaj następujące kroki:

1. Kliknij pozycję **Wszystkie zasoby**, a następnie kliknij nowo utworzoną nazwę przestrzeni nazw.
2. W oknie przestrzeni nazw kliknij pozycję **Zasady dostępu współdzielonego**.
3. Na ekranie **Zasady dostępu współdzielonego** kliknij pozycję **RootManageSharedAccessKey**.
4. W oknie **Zasady: RootManageSharedAccessKey** kliknij przycisk **Kopiuj** obok pozycji **Podstawowe parametry połączenia**, aby skopiować parametry połączenia do schowka w celu późniejszego użycia. Wklej tę wartość do Notatnika lub innej tymczasowej lokalizacji.

    ![connection-string][connection-string]
5. Powtórz poprzedni krok, kopiując i wklejając wartość pozycji **Klucz podstawowy** w lokalizacji tymczasowej do późniejszego użycia.

## <a name="create-a-topic-and-subscriptions"></a>Tworzenie tematu i subskrypcji

Aby utworzyć temat usługi Service Bus, określ przestrzeń nazw, w ramach której chcesz ją utworzyć. Poniższy przykład pokazuje, jak utworzyć temat w portalu:

1. W lewym okienku nawigacji portalu kliknij pozycję **Service Bus** (jeśli pozycja **Service Bus** nie jest widoczna, kliknij pozycję **Wszystkie usługi**).
2. Kliknij przestrzeń nazw, w której chcesz utworzyć temat.
3. W oknie przestrzeni nazw kliknij pozycję **Tematy**, a następnie w oknie **Tematy** kliknij pozycję **+ Tematy**.
4. Wprowadź **Nazwę tematu**, a pozostałe wartości pozostaw domyślne.
5. W dolnej części okna kliknij pozycję **Utwórz**.
6. Zanotuj nazwę tematu.
7. Wybierz utworzony temat.
8. Kliknij pozycję **+ Subskrypcja**, wprowadź nazwę subskrypcji **S1**, a wszystkie pozostałe wartości pozostaw domyślne.
9. Powtórz poprzedni krok jeszcze dwa razy i utwórz subskrypcje o nazwach **S2** i **S3**.

## <a name="create-filter-rules-on-subscriptions"></a>Tworzenie reguł filtrowania dla subskrypcji

Po aprowizowaniu przestrzeni nazw i tematu/subskrypcji i jeśli posiadasz niezbędne poświadczenia, możesz utworzyć reguły filtrowania w subskrypcji, a następnie wysyłać i odbierać komunikaty. Kod można analizować w [tym folderze przykładów usługi GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/GettingStarted\BasicSendReceiveTutorialwithFilters).

### <a name="send-and-receive-messages"></a>Wysyłanie i odbieranie komunikatów

Aby uruchomić kod, wykonaj następujące czynności:

1. W wierszu polecenia lub wierszu polecenia programu PowerShell sklonuj [repozytorium GitHub usługi Service Bus](https://github.com/Azure/azure-service-bus/), uruchamiając następujące polecenie:

   ```shell
   git clone https://github.com/Azure/azure-service-bus.git
   ```

2. Przejdź do folderu przykładów `azure-service-bus\samples\DotNet\GettingStarted\BasicSendReceiveTutorialwithFilters`.

3. Uzyskaj parametry połączenia skopiowane do Notatnika w sekcji [Uzyskiwanie poświadczeń zarządzania](#obtain-the-management-credentials) tego samouczka. Potrzebna Ci będzie również nazwa tematu utworzonego w poprzedniej sekcji.

4. W wierszu polecenia wpisz następujące polecenie:

   ```shell
   dotnet build
   ```

5. Przejdź do folderu `BasicSendReceiveTutorialwithFilters\bin\Debug\netcoreapp2.0`.

6. Wpisz następujące polecenie, aby uruchomić program. Pamiętaj, aby zastąpić `myConnectionString` wcześniej uzyskaną wartością, a `myTopicName` nazwą utworzonego przez siebie tematu:

   ```shell
   dotnet BasicSendReceiveTutorialwithFilters.dll -ConnectionString "myConnectionString" -TopicName "myTopicName"
   ``` 
7. Postępuj zgodnie z instrukcjami w konsoli, aby najpierw wybrać tworzenie filtru. Tworzenie filtrów obejmuje usuwanie filtrów domyślnych. Gdy używasz programu PowerShell lub interfejsu wiersza polecenia, nie musisz usuwać filtra domyślnego, ale jeśli pracujesz w kodzie, jest to konieczne. Polecenia konsoli 1 i 3 ułatwiają zarządzanie filtrami we wcześniej utworzonych subskrypcjach:

   - Wykonaj 1: aby usunąć filtry domyślne.
   - Wykonaj 2: aby dodać własne filtry.
   - Wykonaj 3: aby opcjonalnie usunąć własne filtry. Pamiętaj, że nie spowoduje to odtworzenia filtrów domyślnych.

    ![Wyświetlanie danych wyjściowych 2](./media/service-bus-tutorial-topics-subscriptions-portal/create-rules.png)

8. Po utworzeniu filtra możesz wysyłać komunikaty. Naciśnij 4 i obserwuj wysyłanie 10 komunikatów do tematu:

    ![Wysyłanie danych wyjściowych](./media/service-bus-tutorial-topics-subscriptions-portal/send-output.png)

9. Naciśnij 5 i obserwuj odbieranie komunikatów. Jeśli 10 komunikatów nie zostało zwróconych, naciśnij „m”, aby wyświetlić menu, następnie naciśnij ponownie 5.

    ![Odbieranie danych wyjściowych](./media/service-bus-tutorial-topics-subscriptions-portal/receive-output.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Usuń przestrzeń nazw i kolejkę, jeśli nie są już potrzebne. W tym celu wybierz te zasoby w portalu i kliknij przycisk **Usuń**.

## <a name="understand-the-sample-code"></a>Omówienie przykładowego kodu

Ta sekcja zawiera więcej szczegółów na temat operacji wykonywanych przez przykładowy kod.

### <a name="get-connection-string-and-topic"></a>Pobieranie parametrów połączenia i tematu

Najpierw w kodzie jest deklarowany zestaw zmiennych, które realizują pozostałe operacje programu.

```csharp
string ServiceBusConnectionString;
string TopicName;

static string[] Subscriptions = { "S1", "S2", "S3" };
static IDictionary<string, string[]> SubscriptionFilters = new Dictionary<string, string[]> {
    { "S1", new[] { "StoreId IN('Store1', 'Store2', 'Store3')", "StoreId = 'Store4'"} },
    { "S2", new[] { "sys.To IN ('Store5','Store6','Store7') OR StoreId = 'Store8'" } },
    { "S3", new[] { "sys.To NOT IN ('Store1','Store2','Store3','Store4','Store5','Store6','Store7','Store8') OR StoreId NOT IN ('Store1','Store2','Store3','Store4','Store5','Store6','Store7','Store8')" } }
};
// You can have only have one action per rule and this sample code supports only one action for the first filter, which is used to create the first rule. 
static IDictionary<string, string> SubscriptionAction = new Dictionary<string, string> {
    { "S1", "" },
    { "S2", "" },
    { "S3", "SET sys.Label = 'SalesEvent'"  }
};
static string[] Store = { "Store1", "Store2", "Store3", "Store4", "Store5", "Store6", "Store7", "Store8", "Store9", "Store10" };
static string SysField = "sys.To";
static string CustomField = "StoreId";
static int NrOfMessagesPerStore = 1; // Send at least 1.
```

Parametry połączenia i nazwa tematu są przekazywane za pośrednictwem parametrów wiersza polecenia, jak to pokazano, a następnie odczytywane w metodzie `Main()`:

```csharp
static void Main(string[] args)
{
    string ServiceBusConnectionString = "";
    string TopicName = "";

    for (int i = 0; i < args.Length; i++)
    {
        if (args[i] == "-ConnectionString")
        {
            Console.WriteLine($"ConnectionString: {args[i + 1]}");
            ServiceBusConnectionString = args[i + 1]; // Alternatively enter your connection string here.
        }
        else if (args[i] == "-TopicName")
        {
            Console.WriteLine($"TopicName: {args[i + 1]}");
            TopicName = args[i + 1]; // Alternatively enter your queue name here.
        }
    }

    if (ServiceBusConnectionString != "" && TopicName != "")
    {
        Program P = StartProgram(ServiceBusConnectionString, TopicName);
        P.PresentMenu().GetAwaiter().GetResult();
    }
    else
    {
        Console.WriteLine("Specify -Connectionstring and -TopicName to execute the example.");
        Console.ReadKey();
    }
}
```

### <a name="remove-default-filters"></a>Usuwanie filtrów domyślnych

Podczas tworzenia subskrypcji usługa Service Bus tworzy filtr domyślny dla każdej subskrypcji. Ten filtr umożliwia odbieranie wszystkich komunikatów wysłanych do tematu. Jeśli chcesz używać filtrów niestandardowych, możesz usunąć filtr domyślny, jak pokazano w poniższym kodzie:

```csharp
private async Task RemoveDefaultFilters()
{
    Console.WriteLine($"Starting to remove default filters.");

    try
    {
        foreach (var subscription in Subscriptions)
        {
            SubscriptionClient s = new SubscriptionClient(ServiceBusConnectionString, TopicName, subscription);
            await s.RemoveRuleAsync(RuleDescription.DefaultRuleName);
            Console.WriteLine($"Default filter for {subscription} has been removed.");
            await s.CloseAsync();
        }

        Console.WriteLine("All default Rules have been removed.\n");
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.ToString());
    }

    await PresentMenu();
}
```

### <a name="create-filters"></a>Tworzenie filtrów

Poniższy kod dodaje filtry niestandardowe zdefiniowane w tym samouczku:

```csharp
private async Task CreateCustomFilters()
{
    try
    {
        for (int i = 0; i < Subscriptions.Length; i++)
        {
            SubscriptionClient s = new SubscriptionClient(ServiceBusConnectionString, TopicName, Subscriptions[i]);
            string[] filters = SubscriptionFilters[Subscriptions[i]];
            if (filters[0] != "")
            {
                int count = 0;
                foreach (var myFilter in filters)
                {
                    count++;

                    string action = SubscriptionAction[Subscriptions[i]];
                    if (action != "")
                    {
                        await s.AddRuleAsync(new RuleDescription
                        {
                            Filter = new SqlFilter(myFilter),
                            Action = new SqlRuleAction(action),
                            Name = $"MyRule{count}"
                        });
                    }
                    else
                    {
                        await s.AddRuleAsync(new RuleDescription
                        {
                            Filter = new SqlFilter(myFilter),
                            Name = $"MyRule{count}"
                        });
                    }
                }
            }

            Console.WriteLine($"Filters and actions for {Subscriptions[i]} have been created.");
        }

        Console.WriteLine("All filters and actions have been created.\n");
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.ToString());
    }

    await PresentMenu();
}
```

### <a name="remove-your-custom-created-filters"></a>Usuwanie utworzonych filtrów niestandardowych

Jeśli chcesz usunąć wszystkie filtry w ramach subskrypcji, poniższy kod pokazuje, jak to zrobić:

```csharp
private async Task CleanUpCustomFilters()
{
    foreach (var subscription in Subscriptions)
    {
        try
        {
            SubscriptionClient s = new SubscriptionClient(ServiceBusConnectionString, TopicName, subscription);
            IEnumerable<RuleDescription> rules = await s.GetRulesAsync();
            foreach (RuleDescription r in rules)
            {
                await s.RemoveRuleAsync(r.Name);
                Console.WriteLine($"Rule {r.Name} has been removed.");
            }
            await s.CloseAsync();
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.ToString());
        }
    }
    Console.WriteLine("All default filters have been removed.\n");

    await PresentMenu();
}
```

### <a name="send-messages"></a>Wysyłanie komunikatów

Wysyłanie komunikatów do tematu przypomina wysyłanie komunikatów do kolejki. W tym przykładzie pokazano, jak wysyłać komunikaty z wykorzystaniem listy zadań i przetwarzania asynchronicznego:

```csharp
public async Task SendMessages()
{
    try
    {
        TopicClient tc = new TopicClient(ServiceBusConnectionString, TopicName);

        var taskList = new List<Task>();
        for (int i = 0; i < Store.Length; i++)
        {
            taskList.Add(SendItems(tc, Store[i]));
        }

        await Task.WhenAll(taskList);
        await tc.CloseAsync();
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.ToString());
    }
    Console.WriteLine("\nAll messages sent.\n");
}

private async Task SendItems(TopicClient tc, string store)
{
    for (int i = 0; i < NrOfMessagesPerStore; i++)
    {
        Random r = new Random();
        Item item = new Item(r.Next(5), r.Next(5), r.Next(5));

        // Note the extension class which is serializing an deserializing messages
        Message message = item.AsMessage();
        message.To = store;
        message.UserProperties.Add("StoreId", store);
        message.UserProperties.Add("Price", item.getPrice().ToString());
        message.UserProperties.Add("Color", item.getColor());
        message.UserProperties.Add("Category", item.getItemCategory());

        await tc.SendAsync(message);
        Console.WriteLine($"Sent item to Store {store}. Price={item.getPrice()}, Color={item.getColor()}, Category={item.getItemCategory()}"); ;
    }
}
```

### <a name="receive-messages"></a>Odbieranie komunikatów

Komunikaty są ponownie odbierane za pośrednictwem listy zadań, a kod korzysta z dzielenia na partie. Umożliwia wysyłanie i odbieranie z wykorzystaniem dzielenia na partie, ale w tym przykładzie pokazano tylko, jak odbierać partie. W rzeczywistości nie należy wychodzić z pętli, ale kontynuować zapętlenie i ustawić większy przedział czasu, np. jedną minutę. Wywołanie receive do brokera pozostaje przez ten czas otwarte i jeśli dotrą komunikaty, zostaną od razu zwrócone i zostanie wysłane nowe wywołanie receive. To pojęcie jest nazywane *długim sondowaniem*. Częstszym rozwiązaniem jest użycie pompy receive opisane w przewodniku [Szybki start](service-bus-quickstart-portal.md) oraz w kilku innych przykładach w repozytorium.

```csharp
public async Task Receive()
{
    var taskList = new List<Task>();
    for (var i = 0; i < Subscriptions.Length; i++)
    {
        taskList.Add(this.ReceiveMessages(Subscriptions[i]));
    }

    await Task.WhenAll(taskList);
}

private async Task ReceiveMessages(string subscription)
{
    var entityPath = EntityNameHelper.FormatSubscriptionPath(TopicName, subscription);
    var receiver = new MessageReceiver(ServiceBusConnectionString, entityPath, ReceiveMode.PeekLock, RetryPolicy.Default, 100);

    while (true)
    {
        try
        {
            IList<Message> messages = await receiver.ReceiveAsync(10, TimeSpan.FromSeconds(2));
            if (messages.Any())
            {
                foreach (var message in messages)
                {
                    lock (Console.Out)
                    {
                        Item item = message.As<Item>();
                        IDictionary<string, object> myUserProperties = message.UserProperties;
                        Console.WriteLine($"StoreId={myUserProperties["StoreId"]}");

                        if (message.Label != null)
                        {
                            Console.WriteLine($"Label={message.Label}");
                        }

                        Console.WriteLine(
                            $"Item data: Price={item.getPrice()}, Color={item.getColor()}, Category={item.getItemCategory()}");
                    }

                    await receiver.CompleteAsync(message.SystemProperties.LockToken);
                }
            }
            else
            {
                break;
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.ToString());
        }
    }

    await receiver.CloseAsync();
}
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku zainicjowano zasoby przy użyciu witryny Azure Portal, a następnie wysłano i odebrano komunikaty z tematu usługi Service Bus i jego subskrypcji. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie tematu usługi Service Bus i co najmniej jednej subskrypcji do tego tematu przy użyciu witryny Azure Portal
> * Dodawanie filtrów tematu przy użyciu kodu platformy .NET
> * Tworzenie dwóch komunikatów z różną zawartością
> * Wysyłanie komunikatów oraz sprawdzanie, czy dotarły do spodziewanych subskrypcji
> * Odbieranie komunikatów z subskrypcji

Więcej przykładów dotyczących wysyłania i odbierania komunikatów znajduje się w temacie [Service Bus samples on GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted) (Przykłady dotyczące usługi Service Bus w serwisie GitHub).

Przejdź do następnego samouczka, aby dowiedzieć się więcej o korzystaniu z możliwości publikowania/subskrypcji usługi Service Bus.

> [!div class="nextstepaction"]
> [Aktualizowanie magazynu przy użyciu programu PowerShell oraz tematów/subskrypcji](service-bus-tutorial-topics-subscriptions-powershell.md)

[bezpłatne konto]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[w pełni kwalifikowanej nazwy domeny]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[Azure portal]: https://portal.azure.com/

[connection-string]: ./media/service-bus-tutorial-topics-subscriptions-portal/connection-string.png
[service-bus-flow]: ./media/service-bus-tutorial-topics-subscriptions-portal/about-service-bus-topic.png