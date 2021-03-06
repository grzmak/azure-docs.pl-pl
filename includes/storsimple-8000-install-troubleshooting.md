<!--author=alkohli last changed: 08/29/17-->

## <a name="troubleshooting-update-failures"></a>Rozwiązywanie problemów dotyczących niepowodzenia aktualizacji
**Co się stanie, jeśli zostanie wyświetlone powiadomienie o niepowodzeniu testów przed uaktualnieniem?**

Jeśli test wstępny nie powiedzie się, przeczytaj informacje z paska powiadomień szczegółowych w dolnej części strony. Znajdują się tam wskazówki dotyczące rodzaju testu wstępnego, który się nie powiódł. Na przykład otrzymasz powiadomienie, które nie powiodły sprawdzenie kondycji kontrolera i sprawdzanie kondycji składnika sprzętu. Przejdź do **Monitor > kondycji sprzętu**. Należy upewnić się, że zarówno kontrolery są w dobrej kondycji i w trybie online. Należy również upewnić się, że wszystkie składniki sprzętowe w urządzeniu StorSimple są wyświetlane, aby wskazywać dobrej kondycji, w tym bloku. Po spełnieniu tych warunków można spróbować zainstalować aktualizacje. Jeśli nie jesteś w stanie rozwiązać problemów związanych ze składnikami sprzętowymi, konieczne będzie skontaktowanie się z pomocą techniczną firmy Microsoft w celu uzyskania dalszych instrukcji.

**Co zrobić, jeśli pojawi się komunikat o błędzie „Nie można zainstalować aktualizacji”, a zaleceniem jest odwołanie się do przewodnika rozwiązywania problemów dotyczących aktualizacji w celu określenia przyczyny awarii?**

Jedną z prawdopodobnych przyczyn może być brak połączenia z serwerami usługi Microsoft Update. Poniżej opisano sprawdzanie ręczne, które należy wykonać. Jeśli utracisz łączność z serwerem aktualizacji, zadanie aktualizacji zakończy się niepowodzeniem. Łączność możesz sprawdzić, uruchamiając następujące polecenie cmdlet z interfejsu programu Windows PowerShell urządzenia StorSimple:

 `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter>`

Uruchom to polecenie cmdlet na obu kontrolerach.

Jeśli sprawdzono, że połączenie istnieje, ale problem nadal występuje, skontaktuj się z pomocą techniczną firmy Microsoft w celu uzyskania dalszych instrukcji.

**Co zrobić, jeśli podczas aktualizowania urządzenia do aktualizacji Update 4 jest wyświetlany komunikat o niepowodzeniu aktualizacji, a na obu kontrolerach działa aktualizacja Update 4?**

Począwszy od aktualizacji Update 4, jeśli na obu kontrolerach działa ta sama wersja oprogramowania i wystąpi niepowodzenie aktualizacji, kontrolery nie przejdą w tryb odzyskiwania. Ta sytuacja może wystąpić, jeśli poprawka oprogramowania urządzenia (aktualizacja 1-rzędna) zostanie zastosowana pomyślnie na obu kontrolerach, ale inne poprawki (2-rzędna i 3-rzędna) mają dopiero zostać zastosowane. Począwszy od aktualizacji Update 4, kontrolery przejdą w tryb odzyskiwania tylko wtedy, gdy na obu kontrolerach działają różne wersje oprogramowania. 

Jeśli użytkownik zobaczy niepowodzenie aktualizacji po uruchomieniu aktualizacji Update 4 na obu kontrolerach, zalecane jest odczekanie kilku minut i wykonanie ponownej próby przeprowadzenia aktualizacji. Jeśli ponowna próba się nie powiedzie, należy skontaktować się z pomocą techniczną firmy Microsoft.
