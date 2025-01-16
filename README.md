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

4. **Interfejs użytkownika**:
   - Na ekranie wyświetlane są informacje o:
     - Kierunku i sile wiatru.
     - Aktualnym położeniu łódki na jeziorze (np. współrzędne X, Y).

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

---

## 4. Struktura kodu
1. **Pliki projektu**:
   - `MainPage.xaml`: Interfejs użytkownika (widok jeziora, łódki i przycisków).
   - `MainPage.xaml.cs`: Logika obsługi przycisków i interfejsu użytkownika.
   - `Boat.cs`: Klasa reprezentująca łódkę (położenie, prędkość).
   - `Wind.cs`: Klasa reprezentująca wiatr (kierunek, prędkość).
   - `GameEngine.cs`: Logika gry (ruch łódki, wpływ wiatru).

2. **Główne klasy**:
   - **Boat**:
     ```csharp
     public class Boat
     {
         public float X { get; set; }
         public float Y { get; set; }
         public float Speed { get; set; }

         public void Move(float deltaX, float deltaY)
         {
             X += deltaX;
             Y += deltaY;
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
         }
     }
     ```

---



