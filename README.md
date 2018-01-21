# Framework JavaScript AngularJS – podstawowe koncepcje

Aby rozpocząć wystarczy stworzyć plik `index.html`, który będzie głównym plikiem naszej aplikacji.

```
cd ~
mkdir angular-intro
cd angular-intro
touch index.html
code index.html
```

Na tym etapie aplikację developerską będziemy serwować przy użyciu lokalnego serwera `serve` którego zainstalowaliśmy wcześniej. Jeśli na maszynie na której się jesteśmy nie ma zainstalowanego `serve` instalujemy go poleceniem
```
sudo npm install -g serve
```

Na początku plik `index.html` powinien zawierać tylko podstawowy kod HTML:
```
<!doctype html>
<html>
  <head>
    <title>Angular Application</title>
  </head>
  <body>
    <h1>Hello Angular!</h1>
  </body>
</html>
```

Mając gotowy kod serwujemy go następującym poleceniem:
```
cd angular-intro
serve
```

Nasza aplikacja będzie serwowana pod adresem `localhost:5000`. Po przejściu pod wskazany adres w przeglądarce zobaczymy pustą stronę z nagłówkiem **Hello Angular!**.

Aby pisać aplikację w Angularze, należy dodać jego go jako pierwszy skrypt ładujący się na naszej stronie. W znaczniku `<head>` dodajemy linijkę:
```
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular.min.js"></script>

```

Dodany skrypt zainicjalizuje dla nas Angulara i będzie on dostępny jako globalny obiekt.

Aby sprawdzić czy Angular jest już dostępny, po odświeżeniu strony w przeglądarce przechodzimy do konsoli developerskiej Chrome i wpisujemy polecenie `angular`.

Jeśli angular będzie gotowy do użycia konsola zwróci nam fragment obiektu Angulara, który powinien wyglądać mniej-więcej tak:
```
{$interpolateMinErr: ƒ, element: ƒ, errorHandlingConfig: ƒ, bootstrap: ƒ, copy: ƒ, …}
```

W przypadku problemu z załadowaniem frameworka dostaniemy komunikat
```
Uncaught ReferenceError: angular is not defined
```

Na początkowym etapie w pliku `index.html` stworzymy skrypt, w którym implementować wymagane funkcjonalności. Tuż przed zamknięciem znacznika `<body>` dodajemy pusty skrypt:
```
<script>
  console.log('Hello Angular!')
</script>
```

## Angular templates

Pisanie kodu w Angularze pozwala nam dodawać do kodu HTML funkcjonalności, które nie są dostępne w samym języku HTML, przez co możemy dodać dynamikę do naszej statycznej strony.

Funkcjonalności są dodawane poprzez specjalne atrybuty w znacznikach html, nazywane **dyrektywami**.

Główną dyrektywą, która jest wymagana do działania naszej aplikacji jest dyrektywa `ng-app`, która poinformuje Angulara gdzie w drzewie DOM ma zainicjalizować naszą aplikację.

Dyrektywę `ng-app` dodamy do tagu `body` naszej aplikacji w następujący sposób:
```
<body ng-app>
```

Od teraz cała nasza strona jest aplikacją Angularową, w której mamy dostęp do wszystkich udogodniej które ten framework nam zapewnia. Możemy od teraz wykonywać dynamiczny kod w naszej przeglądarce.

Najważniejszą zmianą dodaną do HTML przez Angulara jest dynamiczna ewaluacja, którą deklarujemy używając podwójnych nawiasów klamrowych `{{ wyrażenie }}`.

Wszystko co znajduje się wewnątrz tych nawiasów zostanie zewaluowane i na ekranie pojawi się wynik wyrażenia a nie tekst, który w nim zawarliśmy. Aby sprawdzić działanie tej funkcjonalności w tagu `<body>` tuż pod znacznikiem `<h1>` dodajemy znacznik `<p>` wraz z poniższym kodem:

```
<p>{{ 1 + 2 }}</p>
```

Po odświeżeniu strony na ekranie poniżej nagłówka zobaczymy tylko cyfrę `3` a więc wyrażenie zawarte wewnątrz nawiasów klamrowych, które zostało już zewaluowane.

Jeśli usunęli byśmy dyrektywę `ng-app` Angular nie rozpoczął by procedury ewaluacji naszego kodu i na ekranie pojawiło by się tylko nasze wyrażenie w postaci `{{ 1 + 2 }}`.

## Przegląd podstawowych dyrektyw Angulara

### `ng-model`

`ng-model` to dyrektywa, która wiąże model danych do tagu `<input>`. Sprawdzić jej działanie dodamy do naszego kodu zamiast wcześniej stworzonego tagu `<p>`:
```
<input type="text" ng-model="yourName" placeholder="Enter a name here">
```

Dyrektywa `ng-model` przyjmuje parametr, który jest nazwą zmiennej pod którą będzie dostępny nasz model, w tym przypadku String z zawartością naszego inputa.

Model inputa jest teraz dostępny w zmiennej o nazwie `yourName` wewnątrz naszego kodu HTML. Możemy ją teraz wykorzystać np w zadeklarowanym wcześniej znaczniku `<h1>`, poprzez wykorzystanie nawiasów klamrowych:

```
<h1>Hello {{ yourName }}!</h1>
```

Po odświeżeniu strony każda zmiana zawartości inputa będzie skutkowała aktualizacją danych w tagu `<h1>`.

### `ng-app`

Najważniejszą dyrektywą w Angularze jest poznana wcześniej dyrektywa `ng-app`. Dyrektywa ta może przyjmować parametr mówiący o nazwie głównego modułu aplikacji, który ma zostać wykonany podczas inicjalizacji aplikacji.

Informację o głównym module aplikacji dodajemy w następujący sposób:
```
ng-app="myApp"
```

Po dodaniu informacji o module i odświeżeniu strony, w konsoli developerskiej zobaczymy błąd
```
angular.js:88 Uncaught Error: [$injector:modulerr]
```

Jest to spowodowane tym, że Angular nie znalazł w naszym kodzie deklaracji modułu o nazwie `myApp`.

Dodamy taką deklarację w tagu `<script>` na dole strony:
```
angular.module('myApp', []);
```

Moduły w aplikacji Angularowej pozwalają nam na wygodny podział naszego kodu na małe fragmenty, które mogą zostać wielokrotnie użyte bez konieczności kopiowania kodu.

Deklaracja modułu polega na wywołaniu metody `module` na globalnym obiekcie Angulara, która przyjmuje 2 parametry:

Pierwszym jest nazwa modułu czyli unikalny w obrębie naszej aplikacji identyfikator, który podajemy jako parametr do dyrektywy `ng-app`.

Drugi to tablica identyfikatorów modułów, których deklarowany moduł wymaga do działania. W tym przypadku podajemy pustą tablicę, ponieważ główny moduł aplikacji na tym etapie nie ma dodatkowych zależności.

Moduł możemy przypisać do zmiennej, aby w łatwy sposób się później do niego odwoływać:
```
var myApp = angular.module('myApp', []);
```

### `ng-controller`

Dotychczas nasza aplikacja korzystała tylko z wbudowanych w Angulara mechanizmów. Aby zaimplementować własne funkcjonalności będziemy potrzebowali specjalnej funkcji, nazywanej **kontrolerem**, w której będziemy mogli dodać własną logikę aplikacji.

Kontrolery definiujemy dyrektywą `ng-controller` z parametrem mówiącym o nazwie kontrolera:
```
<div ng-controller="NotesListController"></div>
```

Aby odróżnić funkcje będące kontrolerami w naszej aplikacji przyjmuje się konwencję dodawania do nazwy sufiksu `Controller` jak w naszym przypadku `NotesListController`.

Nazwy kontrolerów również muszą być unikalne w obrębie aplikacji co może skutkować długimi identyfikatorami do których będziemy się odwoływać w kodzie, dlatego możemy rozbudować parametr `ng-controller` definiując alias dla naszego kontrolera:
```
<div ng-controller="NotesListController as notesList"></div>
```

Odtąd w obrębie kodu HTML nie będziemy musieli odwoływać się do kontrolera podając jego pełną nazwę, lecz tylko zdefiniowany alias - usprawni to czytelność kodu.

Deklaracja kontrolera w kodzie JavaScript polega na wywołaniu na obiekcie modułu metody `controller` z dwoma parametrami.

Pierwszy parametr to unikalna nazwa kontrolera, drugi to funkcja definiująca naszą funkcjonalność. Wewnątrz kontrolera przypisujemy aktualny kontekst wywołania funkcji do zmiennej o nazwie `notesList`, aby w prosty sposób odwoływać się do kontrolera w taki sam sposób jak wszablonie HTML.

```
myApp.controller('NotesListController', function() {
  var notesList = this;
})
```

### `ng-submit`

Zadeklarowany kontroler pozwala nam przejąć kontrolę nad tym co dzieje się w przeglądarce podobnie jak poznany wcześniej mechanizm przechwytywania zdarzeń poprzez metodę `document.addEventListener`, jest on natomiast dużo wygodniejszy w użyciu.

Odtąd wszystko co zostanie przypisane do zmiennej `notesList` w kontrolerze będzie dostępne pod tą samą nazwą w szablonie HTML:
```
myApp.controller('NotesListController', function() {
  var notesList = this;

  notesList.noteText = '';

  notesList.addNote = function() {
    console.log('form submitted with value ' + notesList.noteText);
    notesList.noteText = '';
  }
})
```

Do kontrolera dodaliśmy zmienną `noteText` w której będziemy przechowywać wartość z tagu `<input>` oraz funkcję, która będzie obsługiwała dodawanie notatki.

Tworzymy teraz w HTML prosty formularz:
```
<form>
  <input type="text">
  <input type="submit" value="add note">
</form>
```

Nie musimy już teraz globalnie definiować funkcji obsługującej zdarzenie `submit` na formularzu, wystarczy  dodać do niego dyrektywę `ng-submit` z argumentem będącym nazwą metody w kontrolerze, która powinna zostać wywołana podczas wysłania formularza. Pamiętajmy też o dyrektywie `ng-model` w tagu `<input>` oraz zmiennej, w której będziemy przechowywać wartość.

```
<form ng-submit="notesList.addNote()">
  <input type="text" ng-model="notesList.noteText">
  <input type="submit" value="add note">
</form>
```

### `ng-repeat`

Następnym krokiem w budowie aplikacji będzie przechowywanie stanu naszej aplikacji w zmiennej `notes`, która będzie tablicą obiektów.

Tworzymy w kontrolerze zmienną `notes`:
```
notesList.notes = [];
```

Następnie w funkcji służącej do obsługi wywłania formularza zamiast `console.log` implementujemy funkcjonalność, która doda nową notatkę do zadeklarowanej wcześniej tablicy:
```
notesList.notes.push(notesList.noteText);
```

bądź aby nowe notatki były dodawane na początku listy:
```
notesList.notes.unshift(notesList.noteText);
```

Kolejnym krokiem jest użycie tablicy w szablonie HTML, poprzez dyrektywę `ng-repeat`, która na podstawie tablicy zbuduje za nas kod html, który wyświetli notatki:
```
<ol>
  <li ng-repeat="note in notesList.notes">
    <span>
      {{ note }}
    </span>
  </li>
</ol>
```

Dyrektywa `ng-repeat` jako argument przyjmuje specjalną składnię mówiącą o tym jak ma nazywać się aktualny obiekt podczas iteracji po całej tablicy po której będziemy iterować.

W kilku linijkach czytelnego kodu, przy pomocy Angulara, udało nam się osiągnąć to co potrafi sprawiać sporo problemów w czystym kodzie JavaScript.

Angular dzięki omawianej dyrektywie sam zadba o przebudowywanie drzewa DOM podczas zmian w modelu danych, czyli tablicy notatek. Problematyczne dla Angulara będzie natomiast wielokrotne wystąpienie notatki o takiej samej treści.

Jeśli dodamy dwie takie same notatki w konsoli developerskiej zobaczymy błąd:
```
Error: [ngRepeat:dupes]
```

Aby temu zapobiec należy rozszerzyć argument dyrektywy od informację w jaki sposób Angular powinien rozróżniać kolejne elementy w tablicy. Służy do tego polecenie `track by`:
```
  <li ng-repeat="note in notesList.notes track by $index">
```

Jako parametr użyty w `track by` wykorzystamy zmienną w której Angular przechowuje informację o kroku iteracji i na tej podstawie rozróżnimy notatki.

### `ng-click`

Ważnym elementem budowy aplikacji webowej jest możliwość przechwytywanie kliknięć użytkownika. W Angularze służy do tego dyrektywa `ng-click`.

Dzięki tej dyrektywie dodamy obsługę kasowania notatek po kliknięciu.

Do tagu `<span>` w którym wyświetlana jest notatka dodajemy dyrektywę `ng-click` wraz z argumentem mówiącym o tym jaką metodę kontrolera wywołać po kliknięciu:
```
<span ng-click="notesList.deleteNote($index)">
```

Następnie w kontrolerze definiujemy metodę, która ma zostać wywołana oraz parametr, którym odróżnimy notatkę, która ma zostać skasowana:
```
notesList.deleteNote = function($index) {
  notesList.notes.splice($index, 1);
}
```

Używając metody `splice` na tablicy kasujemy notatkę o indeksie zgodnym z klikniętą notatką. Dzięki Angularowi nie musimy się trudzić samodzielnym poszukiwaniem tego indeksu, Angular o to zadbał.

### `ng-show` i `ng-if`

Kolejnym bardzo przydatnym mechanizmem Angulara są dyrektywy `ng-show` oraz `ng-if`, których działanie jest bardzo analogiczne, daltego zostały ujęte w tym samym punkcie.

Dyrektywy te służą do warunkowego wyświetlenia zawartości tagu do którego są przypięte, jeśli warunek podany jako parametr jest spełniony.
```
ng-if="notesList.notes.length == 0"
```
bądź

```
ng-show="notesList.notes.length == 0"
```

Różnica pomiędzy nimi jest taka, że dyrektywa `ng-show` dodaje tag wraz z zawartością do drzewa DOM po czym go ukrywa, natomiast dyrektywa `ng-if` w ogóle nie dodaje elementu dopóki warunek nie jest spełniony.

Sposób użycia w szablonie HTML:
```
<div ng-if="notesList.notes.length == 0">Notes list is empty.</div>
```

Powyższy kod wyświetli informację o tym, że lista notatek jest pusta, kiedy długość tablicy będzie równa zero.

W analogiczny sposób możemy dodać mechanizm służący do wyświetlenia nagłówka, kiedy przynajmniej jedna notatka będzie dostępna:

```
<p ng-if="notesList.notes.length">Notes list:</p>
```

### `ng-disabled`

Dyrektywa `ng-disabled` służy do wyłączenia możliwości interakcji przez użytkownika na dowolnym elemencie typu `input` więc także na elemencie `button`.

W naszej aplikacji możemy dodać przycisk służący do kasowania wszystkich notatek kiedy lista nie jest pusta
```
  <button ng-click="notesList.deleteAll()" ng-disabled="notesList.notes.length">
    delete all notes
  </button>
```

Możemy też wyłączyć możliwość wysłania formularza dodając do przycisku dodającego walidację długości notatki, tzn jeśli notatka miała by mieć zero znaków, to uznajemy ją za niepotrzebną i nie pozwalamy na dodanie tego typu notatki.
```
<input type="submit" value="add note" ng-disabled="notesList.noteText.length === 0">
```

### `ng-cloak`

Ostatnią z najważniejszych dyrektyw jest `ng-cloak`, która służy do przykrycia części aplikacji, zanim zostanie ona zewaluowana przez angulara. Warto ją dodać dodatkowo do tagu do którego przypięliśmy dyrektywę `ng-app`.

## Komponenty

Do tej pory poznaliśmy w Angularze pojęcia modułów, kontrollerów oraz dyrektywy. Na tym etapie, nasza aplikacja jest w pełni funkcjonalna, natomiast jej wygląd pozostawia wiele do życzenia.

Aby poprawić wygląd naszej aplikacji możemy przy pomocy CSS ostylować każdy z elementów naszej aplikacji, bądź wykorzystać do tego część Angulara nazywaną **komponentami**, które są po prostu dyrektywami z dodatkowymi możliwościami.

Komponenty działają podobnie do dyrektyw, to znaczy dodają dodatkowe funkcjonalności do podstawowych elementów HTML. Różnica jest taka, dzięki komponentom tworzymy własne tagi HTML wraz z ich stylami, itp.

Na dzisiejszych zajęciach wykorzystamy zbiór gotowych komponentów z biblioteki o nazwie Angular Material, która definiuje elementy HTML zgodne ze specyfikacją metodologii stworzeonej przez firmę Google o nazwie Material Design.

## Angular Material

Aby mieć możliwość skorzystania z **modułu** o nazwie `ngMaterial`, zawierającej definicję wszystkich komponentów i dyrektyw Material Design musimy wykonać dwie czynności:

Po pierwsze musimy dodać odpowiednie skrypty do naszego pliku HTML:
```
<link rel="stylesheet" href="https://ajax.googleapis.com/ajax/libs/angular_material/1.1.0/angular-material.min.css">

<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular-animate.min.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular-aria.min.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular-messages.min.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/angular_material/1.1.0/angular-material.min.js"></script>
```

Po drugie musimy poinformować dodać moduł `ngMaterial` jako zależność naszego głównego modułu aplikacji:
```
var myApp = angular.module('myApp', ['ngMaterial']);
```

Specyfikacja wszystkich komponentów Angular Material znajduje się na stronie https://material.angularjs.org

### `mdButton`

Aby poprawić wygląd przycisków w naszej aplikacji zastąpimy użycie tagów `<button>` oraz `<input type="submit">` komponentem o nazwie `mdButton` którego tag html to `<md-button>`:

```
<md-button type="submit"
           class="md-raised md-accent"
           ng-disabled="notesList.noteText.length === 0">
  add note
</md-button>

<md-button ng-click="notesList.deleteAll()"
           class="md-raised md-warn"
           ng-disabled="notesList.notes.length === 0">
  delete all notes
</md-button>
```

Aby wybrać styl przycisków możemy dodać im odpowiednie klasy spośród zdefiniowanych przez bibliotekę:
`md-raised`, `md-primary`, `md-accent`, `md-warn`.

### `mdInputContainer`

Aby poprawić wygląd pola tekstowego do wprowadzania notatek wykorzystamy komponent `mdInputContainer` który będzie kontenerem na nasz obecny `<input>`
```
<md-input-container>
  <label>Note text</label>
  <input type="text" ng-model="notesList.noteText">
</md-input-container>
```

### `mdCard`

Następnie sprawimy, że nasze notatki będą wyglądały ciekawiej, zastępując wykorzystanie uporządkowanej listy `ol` listą komponentów `mdCard`

```
<div>
  <md-card ng-repeat="note in notesList.notes track by $index">
    <md-card-header ng-click="notesList.deleteNote($index)">
      <md-card-header-text class="md-title">
        {{ note }}
      </md-card-header-text>
    </md-card-header>
  </md-card>
</div>
```

Składnia `mdCard` jest dość zawiła, natomiast dzięki niej nasze karty będą wyglądały znacznie lepiej.

### `mdToolbar` i `mdContent`

Aby nasza aplikacja wyglądała jeszcze ciekawiej przeniesiemy część interfejsu odpowiedzialną za obsługę notatek do tak zwanego toolbara, czyli paska narzędziowego. Wykorzystamy do tego komponent `mdToolbar`.
```
<md-toolbar md-scroll-shrink>
  <div class="md-toolbar-tools">
    <h1>Notes</h1>

    <form ng-submit="notesList.addNote()">
      ...
    </form>

  </div>
</md-toolbar>
```

Niestety formularz w toolbarze zawiera buga, dlatego naprawimy go dodając do `<head>` w naszym pliku specjalne style CSS, które to naprawią:
```
<style>
  md-input-container .md-errors-spacer {
    min-height: 0px !important;
  }
</style>
```

Aby nasz toolbar wyglądał jeszcze profesjonalniej warto dodać dwie dyrektywy do tagu `<h1>`:
```
<h1 md-truncate flex>Notes</h1>
```

Natomiast pozostałą zawartość związaną z zawartością notatek przeniesiemy do komponentu `mdContent`:
<md-content>
  ...
</md-content>

Aby nasza aplikacja po dodaniu wielu notatek nasza aplikacja przewijała się w ciekawszy sposób możemy do komponentu toolbara dodać odpowiednią dyrektywę:
```
<md-toolbar md-scroll-shrink>
```

### Layout

W tym momencie nasza aplikacja jest prawie gotowa, natomiast jest w niej jeszcze kilka elementów, których pozycje są nie do końca odpowiednie.

Aby to naprawić użyjemy wbudowanej w Angular Material funkcjonalności `layout`:
```
<div ng-controller="NotesListController as notesList" layout="column" layout-fill>
…
<md-content>
  <div layout="row" layout-align="center center">
…
<md-card-header-text class="md-title" layout="row" layout-align="space-around center">
```

### `$mdThemingProvider`

Na koniec możemy zająć się dopracowaniem kolorów naszej aplikacji. Wykorzystamy do tego metodę `config` na obiekcie naszej aplikacji, która jako argument przyjmie zmienną o nazwie `$mdThemingProvider` i wywołamy na niej kilka metod odpowiedzialnych za kolory dla użytych wcześniej klas `md-primary`, `md-accent`, itp:

```
myApp.config(function($mdThemingProvider) {
  $mdThemingProvider.theme('default')
    .primaryPalette('pink')
    .accentPalette('light-green')
    .warnPalette('red')
    .backgroundPalette('purple');
});
```

Dostępne domyślnie kolory to:
```
red, pink, purple, deep-purple, indigo, blue, light-blue, cyan, teal, green, light-green, lime, yellow, amber, orange, deep-orange, brown, grey, blue-grey
```

Jeżeli zdedydujemy, żeby nasza aplikacja wyglądała minimalistycznie możemy też dzięki `$mdThemingProvider` wyłączyć całkowicie kolorowanie aplikacji:
```
$mdThemingProvider.disableTheming();
```
