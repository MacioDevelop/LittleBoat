# BoatNavigator - Dokumentacja Techniczna

## 1. Wstęp
BoatNavigator to mobilna aplikacja 2D symulująca żeglugę łódką po jeziorze. Użytkownik steruje łódką za pomocą przycisków kierunkowych, a symulacja uwzględnia zmienny wiatr wpływający na ruch łódki. Aplikacja została zaprojektowana z myślą o nauce podstawowych zasad sterowania łodzią w różnych warunkach atmosferycznych.

---

## 2. Funkcjonalności
1. **Widok gry (2D)**:
   - Główne okno aplikacji wyświetla jezioro oraz łódkę, którą można sterować.
   - Grafika oparta na prostych kształtach i wektorach.

2. **Sterowanie**:
   - Cztery przyciski na dole ekranu umożliwiają:
     - Ruch na północ (przód).
     - Ruch na południe (tył).
     - Ruch na wschód (prawo).
     - Ruch na zachód (lewo).

3. **Wiatr**:
   - Wiatr zmienia się co 10 sekund.
   - Parametry wiatru:
     - Kierunek: losowy (północ, południe, wschód, zachód, lub kombinacje kierunków np. północny-wschód).
     - Prędkość: losowa w zakresie np. od 0 do 20 km/h.
   - Ruch łódki jest modyfikowany przez siłę i kierunek wiatru:
     - Płynięcie pod wiatr jest wolniejsze.
     - Płynięcie z wiatrem jest szybsze.

4. **Zdobywanie punktów**:
   - Użytkownik zdobywa punkty za płynięcie w kierunku wiatru.
     - Jeśli kierunek łódki odpowiada kierunkowi wiatru, użytkownik zdobywa punkty.
     - Punkty są naliczane za każdy postęp w kierunku wiatru.
     - Liczba punktów jest zależna od prędkości wiatru i czasu płynięcia w odpowiednim kierunku.

5. **Interfejs użytkownika**:
   - Na ekranie wyświetlane są informacje o:
     - Kierunku i sile wiatru.
     - Aktualnym położeniu łódki na jeziorze (np. współrzędne X, Y).
     - Aktualnej liczbie punktów.

---

## 3. Architektura aplikacji
1. **Warstwa prezentacji**:
   - Zbudowana przy użyciu komponentów .NET MAUI.
   - Używa kontrolki `Canvas` do renderowania jeziora i łódki.
   - Przyciski sterujące to `Button` osadzone w dolnym panelu.

2. **Logika gry**:
   - Logika ruchu łódki i działania wiatru jest obsługiwana w głównej warstwie aplikacji przy użyciu `MVVM` (Model-View-ViewModel).
   - Ruch łódki jest symulowany jako zmiana współrzędnych w osi X i Y.

3. **Generowanie wiatru**:
   - Wykorzystanie klasy `Timer` do zmiany parametrów wiatru co 10 sekund.
   - Wiatr generowany przez funkcję losową (`Random`).

4. **Fizyka ruchu**:
   - Prędkość ruchu łódki zależy od wiatru:
     - Wzór na prędkość:  
       `V_eff = V_base + WindModifier`  
       Gdzie:
       - `V_base`: Podstawowa prędkość łódki (np. 5 jednostek na sekundę).
       - `WindModifier`: Wpływ wiatru zależny od kierunku (pozytywny lub negatywny).
   - **System punktacji**:
     - Jeśli łódka porusza się w tym samym kierunku co wiatr, użytkownik zdobywa punkty.
     - Punkty naliczane są na podstawie prędkości wiatru:  
       `Points = WindSpeed * TimeInDirection`  
       Gdzie:
       - `WindSpeed`: Prędkość wiatru w km/h.
       - `TimeInDirection`: Czas spędzony płynąc w tym samym kierunku co wiatr.

---

## 4. Struktura kodu
1. **Pliki projektu**:
   - `MainPage.xaml`: Interfejs użytkownika (widok jeziora, łódki i przycisków).
   - `MainPage.xaml.cs`: Logika obsługi przycisków i interfejsu użytkownika.
   - `Boat.cs`: Klasa reprezentująca łódkę (położenie, prędkość, punkty).
   - `Wind.cs`: Klasa reprezentująca wiatr (kierunek, prędkość).
   - `GameEngine.cs`: Logika gry (ruch łódki, wpływ wiatru, punktacja).

2. **Główne klasy**:
   - **Boat**:
     ```csharp
     public class Boat
     {
         public float X { get; set; }
         public float Y { get; set; }
         public float Speed { get; set; }
         public int Points { get; set; } // Nowa zmienna przechowująca punkty

         public void Move(float deltaX, float deltaY)
         {
             X += deltaX;
             Y += deltaY;
         }

         public void AddPoints(int points)
         {
             Points += points; // Funkcja do dodawania punktów
         }
     }
     ```
   - **Wind**:
     ```csharp
     public class Wind
     {
         public string Direction { get; set; }
         public float Speed { get; set; }
     }
     ```
   - **GameEngine**:
     ```csharp
     public class GameEngine
     {
         private Boat _boat;
         private Wind _currentWind;

         public void UpdateWind()
         {
             // Generowanie losowego kierunku i prędkości wiatru
         }

         public void UpdateBoatMovement(string direction)
         {
             // Logika zmieniająca położenie łódki w zależności od wiatru i kierunku
             if (direction == _currentWind.Direction)
             {
                 // Jeśli płyniemy w kierunku wiatru, dodajemy punkty
                 int points = (int)(_currentWind.Speed * 0.1f); // Naliczanie punktów
                 _boat.AddPoints(points);
             }
         }
     }
     ```
---
# Przypadki testowe dla BoatNavigator

## 1. Testowanie sterowania łódką

### 1.1 Testowanie ruchu łódki na północ w warunkach bez wiatru
- **Opis**: Użytkownik naciśnie przycisk "Północ" przy braku wiatru.
- **Oczekiwany wynik**: Łódka porusza się o stałą prędkość do przodu (zwiększa współrzędną Y).

### 1.2 Testowanie ruchu łódki na południe w warunkach bez wiatru
- **Opis**: Użytkownik naciśnie przycisk "Południe" przy braku wiatru.
- **Oczekiwany wynik**: Łódka porusza się o stałą prędkość do tyłu (zmniejsza współrzędną Y).

### 1.3 Testowanie zmiany kierunku łódki na wschód z przyciskiem "Wschód"
- **Opis**: Użytkownik naciśnie przycisk "Wschód" przy braku wiatru.
- **Oczekiwany wynik**: Łódka porusza się na wschód, zwiększając współrzędną X.

### 1.4 Testowanie zmiany kierunku łódki na zachód z przyciskiem "Zachód"
- **Opis**: Użytkownik naciśnie przycisk "Zachód" przy braku wiatru.
- **Oczekiwany wynik**: Łódka porusza się na zachód, zmniejszając współrzędną X.

### 1.5 Testowanie równoczesnego naciśnięcia przycisków "Północ" i "Wschód"
- **Opis**: Użytkownik naciśnie przyciski "Północ" i "Wschód" jednocześnie.
- **Oczekiwany wynik**: Łódka porusza się w kierunku północno-wschodnim (zmienia współrzędne X i Y).

---

## 2. Testowanie wpływu wiatru na ruch łódki

### 2.1 Testowanie wiatru o prędkości 0 km/h
- **Opis**: Ustawienie wiatru o prędkości 0 km/h w losowym kierunku i naciśnięcie przycisku "Południe".
- **Oczekiwany wynik**: Łódka porusza się z normalną prędkością bez wpływu wiatru (prędkość zależna tylko od prędkości łódki).

### 2.2 Testowanie wiatru o prędkości 20 km/h
- **Opis**: Ustawienie wiatru o prędkości 20 km/h z kierunkiem południowym, a użytkownik naciśnie przycisk "Południe".
- **Oczekiwany wynik**: Łódka porusza się szybciej, korzystając z wiatru, z większą prędkością niż normalnie.

### 2.3 Testowanie płynięcia pod wiatr z kierunkiem północnym przy wietrze z południa
- **Opis**: Użytkownik płynie w kierunku północnym przy wietrze wiejącym z południa.
- **Oczekiwany wynik**: Łódka porusza się wolniej z powodu oporu wiatru, spowolnienie ruchu.

### 2.4 Testowanie płynięcia w stronę wiatru bocznego
- **Opis**: Ustawienie wiatru bocznego (np. wschodniego) i naciśnięcie przycisku "Wschód".
- **Oczekiwany wynik**: Łódka porusza się normalną prędkością w kierunku wschodnim, wiatr nie wpływa na ruch.

### 2.5 Testowanie wpływu wiatru na prędkość łódki przy różnych kierunkach wiatru
- **Opis**: Ustawienie wiatru z kierunkiem wschodnim oraz naciśnięcie przycisku "Zachód".
- **Oczekiwany wynik**: Łódka porusza się wolniej w kierunku przeciwnym do wiatru (wiatr wschodni, a ruch zachodni).

---

## 3. Testowanie zdobywania punktów

### 3.1 Testowanie zdobywania punktów za płynięcie z wiatrem
- **Opis**: Użytkownik płynie z wiatrem (np. południe przy wietrze z południa).
- **Oczekiwany wynik**: Punkty są naliczane na podstawie prędkości wiatru (np. 20 km/h), a liczba punktów powinna się zwiększyć.

### 3.2 Testowanie naliczania punktów przy płynięciu z wiatrem o zmiennej prędkości
- **Opis**: Wiatr zmienia prędkość z 5 km/h na 15 km/h, a użytkownik płynie w kierunku wiatru przez 1 minutę.
- **Oczekiwany wynik**: Punkty naliczane są odpowiednio do zmieniającej się prędkości wiatru (np. 5 punktów za pierwsze 5 km/h i 15 punktów za następne).

### 3.3 Testowanie braku punktów za płynięcie pod wiatr
- **Opis**: Użytkownik płynie pod wiatr (np. północ przy wietrze z południa).
- **Oczekiwany wynik**: Punkty nie są naliczane, ponieważ ruch jest spowolniony przez wiatr w przeciwnym kierunku.

### 3.4 Testowanie liczby punktów po płynięciu przez określony czas z wiatrem
- **Opis**: Użytkownik płynie z wiatrem przez 3 minuty, gdy wiatr ma prędkość 10 km/h.
- **Oczekiwany wynik**: Punkty powinny zostać naliczone na podstawie formuły `Points = WindSpeed * TimeInDirection`. Wynik: 30 punktów.

### 3.5 Testowanie punktów za płynięcie z wiatrem przy różnych poziomach prędkości
- **Opis**: Użytkownik płynie z wiatrem o prędkości 5 km/h przez 2 minuty, a następnie zmienia prędkość wiatru na 15 km/h na 3 minuty.
- **Oczekiwany wynik**: Liczba punktów powinna być naliczona osobno za oba okresy, tj. 10 punktów za pierwszy okres i 45 punktów za drugi.

---

## 4. Testowanie zmiany wiatru

### 4.1 Testowanie wpływu zmiany wiatru na punktację
- **Opis**: Wiatr zmienia kierunek z południowego na wschodni w trakcie płynięcia.
- **Oczekiwany wynik**: Po zmianie kierunku, punkty powinny być naliczane za nowy kierunek wiatru, jeśli łódka porusza się w tym samym kierunku.

### 4.2 Testowanie zmiany wiatru podczas ruchu łódki
- **Opis**: Wiatr zmienia kierunek z północnego na zachodni podczas płynięcia na południe.
- **Oczekiwany wynik**: Łódka powinna dostosować swoją prędkość i kierunek w zależności od nowego kierunku wiatru.

### 4.3 Testowanie zmiany wiatru po 10 sekundach
- **Opis**: Po upływie 10 sekund, wiatr zmienia kierunek z wschodniego na zachodni.
- **Oczekiwany wynik**: Aplikacja powinna zaktualizować wyświetlanie kierunku wiatru oraz zmienić prędkość łódki zgodnie z nowym kierunkiem.

---

## 5. Testowanie interfejsu użytkownika

### 5.1 Testowanie aktualizacji informacji o wietrze
- **Opis**: Wiatr zmienia się co 10 sekund, a aplikacja powinna na bieżąco wyświetlać nowe informacje o jego prędkości i kierunku.
- **Oczekiwany wynik**: Interfejs użytkownika powinien aktualizować dane o wietrze w czasie rzeczywistym.

### 5.2 Testowanie aktualizacji współrzędnych łódki
- **Opis**: Użytkownik porusza łódką w różnych kierunkach, a aplikacja powinna zaktualizować jej współrzędne X i Y.
- **Oczekiwany wynik**: Współrzędne X i Y powinny zmieniać się zgodnie z ruchem łódki na ekranie.

### 5.3 Testowanie wyświetlania punktów
- **Opis**: Użytkownik zdobywa punkty za płynięcie z wiatrem, a wynik powinien być widoczny na ekranie.
- **Oczekiwany wynik**: Liczba punktów powinna być aktualizowana po każdym zdobyciu punktów.



