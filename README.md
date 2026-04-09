  # Lazik_Sterujacy_Maciej_Ratuszny
Zdalnie sterowany łazik 2WD oparty na ESP32-S3. Projekt obejmuje autorski serwer WWW do sterowania przez WiFi, implementację sygnałów PWM (ledc) oraz autorski mechanizm bezpieczeństwa (watchdog timeout) zapobiegający utracie kontroli nad robotem.

  # Funkcje robota i zasada działania
* Sterowanie przez WiFi. Łazik tworzy własną sieć (Access Point). Wystarczy się z nią połączyć z dowolnego telefonu lub laptopa i wpisać adres w przeglądarce, aby zobaczyć panel sterowania.
* Płynna jazda (PWM). Silniki nie działają tylko w trybie 0/1. Wykorzystałem sprzętowe sygnały PWM, dzięki czemu łazik precyzyjnie reaguje na polecenia.
* Tryb bezpieczny (Timeout). To kluczowa funkcja bezpieczeństwa. Jeśli łazik straci zasięg WiFi albo użytkownik zamknie stronę, mikrokontroler automatycznie odcina zasilanie silników po 0,5 sekundy. Dzięki temu robot nie uderzy w przeszkodę w razie awarii komunikacji.
* W kodzie zaimplementowałem asynchroniczny serwer HTTP. Kliknięcie przycisku na stronie w telefonie wysyła krótkie zapytanie w tle (bez przeładowywania strony), które ESP32 interpretuje jako rozkaz dla konkretnego silnika. Całość została napisana w środowisku Arduino (C++).

  # Konstrukcja Robota
Pełna lista użytych komponentów:
* Mózg systemu. Mikrokontroler ESP32-S3 (odpowiada za logikę, serwer HTTP i sterowanie).
* Napęd. Gotowe podwozie łazika 2WD (akrylowa rama, koła, koło podporowe) napędzane przez dwa silniki DC z przekładni (tzw. silniki żółte).
* Sterowanie napędem. Mostek H L298N (odbiera sygnały z ESP32 i przekazuje duży prąd na silniki).
* Zasilanie (Paliwo).Ogniwa Li-ion 18650 (Samsung) w dedykowanym koszyku. Dwa takie ogniwa połączone szeregowo dają napięcie ok. 8.4V i mają ogromną wydajność prądową. Zapewniają silnikom potężny moment obrotowy i eliminują ryzyko spadków napięcia przy gwałtownym starcie robota.
* Stabilizacja logiki. Przetwornica Step-Down LM2596S. Obniża napięcie z baterii do stabilnych 5V, chroniąc delikatny procesor ESP32 przed spaleniem.
* Bezpieczeństwo elektryczne. Przełącznik kołyskowy 6-pinowy (DPDT). Użyty jako główny odłącznik – jego konstrukcja pozwala na fizyczne odcięcie obu linii (zarówno plusa, jak i minusa) z baterii, co gwarantuje pełną izolację układu po wyłączeniu.
* Połączenia. Płytka stykowa (Breadboard), pełniąca rolę węzła dla wspólnej masy i zasilania.

  # Jak go uruchomić?
1. Wgraj kod na ESP32-S3.
2. Włącz zasilanie z baterii (przełącznikiem kołyskowym DPDT).
3. Połącz się z WiFi o nazwie `Lazik` (hasło: `12345678`).
4. Wpisz w przeglądarce adres `192.168.4.1`.
5. Mozesz juz sterować łazikiem.
