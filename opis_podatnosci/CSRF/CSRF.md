# **Cross-Site Request Forgery**



## Definicje



Definicja wg. **OWASP**:

    CSRF - zmuszenie przeglądarki ofiary do wykonania pewnej nieautoryzowanej akcji (wykonania żądania HTTP), a atakujący na cel bierze zalogowanego użytkownika

Definicja wg. **CWE**:

    [Aplikacja jest podatna na CSRF], kiedy nie sprawdza czy wysyłane do niej żądanie HTTP zostało świadomie wykonane przez użytkownika


**CSRF** jest podatnością wymagającą pewnego działania ofiary. Może nim być zwykłe korzystanie z aplikacji lub wejście na odpowiednio spreparowaną stronę WWW.

**CSRF** nie należy mylić z atakiem *Man-in-the-browser*, w którym atakujący może wpływać na działanie przeglądarki, ale wiąże się to z wcześniejszym zainstalowaniem w systemie ofiary malware'u.

**CSRF** to również nie to samo co **XSS**. Jeśli w aplikacji występuje **XSS** to jest możliwość zrealizowania **CSRF**, ale jeśli nasza aplikacja podatna jest na **CSRF**, to niekoniecznie musimy być podatani na **XSS***.



