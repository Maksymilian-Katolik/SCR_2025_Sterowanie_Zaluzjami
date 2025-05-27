# System sterowania żaluzjami

## Autor
Maksymilian Katolik

maxkat@student.agh.edu.pl

## Opis projektu
Opis ogólny

System sterowania żaluzjami służy do automatycznego otwierania i zamykania żaluzji w oknach. Składa się z czujników (nasłonecznienia i temperatury), silników poruszających żaluzje oraz jednostki sterującej, która przetwarza dane i podejmuje decyzje. System może działać samodzielnie (automatycznie) albo być sterowany przez użytkownika.


Opis dla użytkownika

Użytkownik może sam ustawić, kiedy żaluzje mają się otwierać lub zamykać oraz w jakim stopniu (czy w pełni a może 75%), albo włączyć tryb automatyczny, w którym system sam podejmuje decyzje – np. zamyka żaluzje, gdy słońce świeci zbyt mocno.


## Stan na 27.05
Projket pozwala na zrealizowanie systemu do sterowania żaluzjami za pomocą systemu zewnętrzego (apki) wydającej polecenia.

Zdeklarowany system zawiera:

### Podkomponenty i połączenia między nimi:
Podsystemy (devices, processes, processor, buses):
remoteActionDispatcher: odbiera zdalne polecenia.
server: serwer sterujący systemem.
shutterEngine: silnik żaluzji.
shutterController: jednostka zarządzająca żaluzją.
cpu, ram: procesor i pamięć.
eth, hwc: magistrale (Ethernet, połączenie sprzętowe).

### Procesy (logika aplikacyjna):
ServerController: zarządza zdalnymi komendami.
ShutterLogicController: podejmuje decyzje na podstawie komend i stanu silnika.
ShutterEngineController: kontroluje silnik na podstawie stanu.

### Połączenia:
Łączą komponenty przez: magistrale (bus access), porty danych (port)

### Procesy i implementacje
ServerController
Zajmuje się odbieraniem i przekazywaniem zdalnych danych.
Ma wątek ProcessRemoteDataThread, który przetwarza dane.

LogicController
Logika podejmowania decyzji:
Odbiera zdalne dane (remote_data_receive)
Odbiera stan silnika (engine_state_data_receive)
Wysyła nowe komendy (send_engine_state_data)
Wysyła zaktualizowane dane (send_remote_data)
Składa się z:
LogicControllerThread: przetwarza komendy.
LogicTransformThread: przekształca dane na działania silnika.

EngineController
Zajmuje się aktualnym stanem silnika. Jeden wątek:
EngineControllerThread: przesyła aktualny stan silnika.


### Wątki
Każdy wątek:
odbiera dane,
przetwarza,
przesyła dalej.

### Urządzenia (devices)
remote_action_dispatcher: wysyła akcje do serwera.
smart_server: przetwarza i wysyła polecenia.
shutter_engine: odbiera komendy i wysyła aktualny stan.
shutter_controller: integruje dane z serwera i silnika.
