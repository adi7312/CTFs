# **Cross-Site Scripting**

## **Definicja**

Jest to atak na serwis WWW, który polega na możliwości osadzenia w treści strony własnego kodu *JavaScript*, co w konsekwencji może doprowadzić do wykonania niepożądanych akcji przez użytkowników odwiedzających tę stronę.

## **Reflected XSS**

Atak ***Reflected XSS*** to taki atak w którym kod HTML/JS zawarty w dowolnym parametrze zapytania (GET, POST, nawet w plikach *cookies*) wyświetalny jest następnie w odpowiedzi (wartość parametru zostaje odbita, stąd nazwa *reflected*).

**Praktycznie wykorzystanie**: wysłanie ofierze odpowiednio spreparowanego linku zawierającego złośliwy kod.

## **Persistent/Stored XSS**

W tym przypadku złośliwy kod JS zostaje zapisany w bazie danych/ innym zewnętrznym systemie i wykonany automatycznie po przejściu na odpowiednią podstronę. W takiej sytuacji zazwyczaj nie trzeba wysyłać ofierze linku, bo można założyć że sama w końcu odwiedzi zaatakowaną podstronę.

## **DOM-based XSS**

W rzeczywistości nie każdy XSS musi wiązać się z komunikacją z serwerem. Czasem możliwość wykonania własnego kodu JS występuje już w istniejącym kodzie JS. Rozważmy poniższy kod skutkujący podatnością DOM-based XSS

```javascript
<script>
    window.addEventListener('hashchange', ev =>{
        let id = unescape(location.hash.slice(1));
        document.getElementaryById('imageholder').innerHTML =
        `<img src = "//example.com/image${id}.png">`;
    });
</script>
```

1. W pierwszej kolejności nasłuchujemy na zdarzennie *onhashchange*. Jest ono wyzwalane gdy w adresie URL zmieni się ta zcęść, która znajduje się po haszu. Czyli np. jeśli w adresie URL znajdzie się najpierw http://exmaple.com#abc, a potem użytkownik zmieni na http://example.com#def to zdarzenie zostanie wyzwolone.
2. Zmienna *id* będzie zawierała część URL-a znajdującą się za haszem. Czyli jeśli np. użytkownik wejdzie na stronę http://example.com#123 to id będzie równe 123.
3. Do elementu HTML o id imageholder zostaje przypisany fragment HTML z elementem \<img>, którego atrybut *src* zawiera adres do obrazka zbudowany na bazie zmiennej *id*.

Gdzie tu leży problem? Rodzi go budowanie kodu HTML na bazie wejścia użytkownika bez jakiegokolwiek enkodowania danych. Jeżeli napastnik spróbuje przejść pod następujący adres URL: http://example.com#qweqwe"%20onerror=alert(1)//. To wówczas:

1. Zmienna id przyjmie wartość qweqwe" onerror=alert(1)//.
2. Zostanie utworzony HTML o treści poniżej. Utworzony obrazek ma więć nie tylko atrybut *src* ale również *onerror* z naszym własnym kodem JS.

```html
<img src="//example.com/imageqweqwe"onerror=alert(1)//.png">
```
3. Przeglądarka spróbuje pobrać obrazek z aresy http://exmaple.com/imageqweqwe. Po pewnym czasie gdy okaże się że taki plik nie istenieje przeglądarka wykona zdarzenie *onerror* na tagu *img*, efektywanie wykonując XSS.
    