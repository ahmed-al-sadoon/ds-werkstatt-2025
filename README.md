# DS Werkstatt 2025 - Serenity BDD with Cucumber

## Serenity BDD 
ist ein leistungsstarkes Framework, das die Erstellung von qualitativ hochwertigen automatisierten Akzeptanztests erleichtert. Es bietet umfangreiche Berichts- und Dokumentationsfunktionen, die es ermöglichen, die Testergebnisse und den Fortschritt der Tests auf eine sehr anschauliche Weise darzustellen.
Mehr Informationen über Serenity BBD sind unter [Serenity BDD User Manual](https://serenity-bdd.github.io/) zu finden.

## Cucumber
ist ein Tool, das die BDD (Behavior Driven Development) Methodik unterstützt. Es ermöglicht das Schreiben von Tests in einer menschenlesbaren Sprache, die von allen Beteiligten verstanden werden kann. Mehr Informationen über Cucumber sind unter [Cucumber Documentation](https://cucumber.io/docs) zu finden.

```Gherkin
Funktionalität: Suche nach Schlüsselwort

  Szenario: Suche nach einem Begriff
    Angenommen Sergey recherchiert Dinge im Internet
    Wenn er nach "Cucumber" sucht
    Dann sollte er Informationen über "Cucumber" sehen
```

## Serenity BDD und Cucumber 
arbeiten hervorragend zusammen, um automatisierte Tests zu erstellen, die sowohl für **Web-Tests** mit Selenium als auch für **API-Tests** mit RestAssured geeignet sind. Serenity fördert gutes Testautomationsdesign und unterstützt mehrere Designmuster, einschließlich klassischer Page Objects, Lean Page Objects/Action Classes und des flexiblen Screenplay-Musters.

### Page Objects
Page Object ist eine Klasse, die die Interaktion mit Web-Elementen in einer Anwendung kapselt. Sie repräsentieren eine Seite oder einen Teil einer Seite in der Anwendung und enthalten Methoden, um mit den Elementen auf dieser Seite zu interagieren.
````java
public class LoginPage extends PageObject {

    @FindBy(id = "username")
    private WebElementFacade usernameField;

    @FindBy(id = "password")
    private WebElementFacade passwordField;

    @FindBy(id = "login")
    private WebElementFacade loginButton;

    public void loginAs(String username, String password) {
        usernameField.type(username);
        passwordField.type(password);
        loginButton.click();
    }
}
````
#### WebElementFacade
ist eine von Serenity BDD bereitgestellte Erweiterung des Selenium-Interfaces WebElement. Sie bietet zusätzliche Methoden für Synchronisation, Abfragen und Interaktionen mit Webelementen. Dadurch werden Tests stabiler und der Code lesbarer. Mehr Informationen über Web-Elemente definieren sind unter [Serenity Page Elements](https://serenity-bdd.github.io/docs/guide/page_elements) zu finden.
### Lean Page Objects/Action Classes
Lean Page Objects/Action Classes sind eine vereinfachte Version von Page Objects, die sich auf die Aktionen konzentrieren, die auf einer Seite ausgeführt werden können, anstatt auf die Struktur der Seite selbst. Sie enthalten Methoden, die bestimmte Aktionen ausführen, wie z.B. das Ausfüllen eines Formulars oder das Klicken auf einen Button.
#### Lean Page Objects
````java
class SearchForm {
    static By SEARCH_FIELD = By.cssSelector("#searchInput");
}
````
#### Action Classes
````java
public class SearchFor extends UIInteractionSteps {

    @Step("Suche nach Begriff {0}")
    public void term(String term) {
        $(SearchForm.SEARCH_FIELD).clear();
        $(SearchForm.SEARCH_FIELD).sendKeys(term, Keys.ENTER);
    }
}
````

### Screenplay-Muster
Das Screenplay-Muster ist ein modernes Testautomatisierungs-Pattern, das von Serenity BDD unterstützt wird. Es beschreibt Tests als Interaktionen von „Akteuren“ (Actors), die Aufgaben (Tasks) und Interaktionen (Actions) auf einer Anwendung ausführen. Web-Elementen werden als Target-Objekte definiert.
#### Eigenschaften
* Ein Actor besitzt Fähigkeiten (Abilities), z. B. das Bedienen eines Browsers.
* Ein Actor führt Aktionen aus, z. B. das Klicken auf Buttons oder das Eingeben von Text.
* Ein Actor beantwortet Fragen (Questions), um Informationen aus der Anwendung abzufragen (z. B. Text, Sichtbarkeit, Status).
* Ein Actor kann Informationen speichern und später wieder abrufen – mit den Methoden remember und recall.
* Ein Actor wird in den Step-Definitionen verwendet, um Testschritte natürlich und lesbar zu beschreiben.
#### Beispiel:
````java
Actor sergey = Actor.named("Sergey");
sergey.can(BrowseTheWeb.with(driver));
sergey.attemptsTo(Open.url("https://wikipedia.org"));
sergey.remember("searchTerm", "Cucumber");
// Später im Test
String term = sergey.recall("searchTerm");
````
#### Einige Beispiele, wie remember und recall mit verschiedenen Datentypen:
````java
actor.remember("userId", 42);
Integer userId = actor.recall("userId");

List<String> items = Arrays.asList("Apfel", "Banane");
actor.remember("warenkorb", items);
List<String> warenkorb = actor.recall("warenkorb");

User user = new User("Max", "Mustermann");
actor.remember("aktuellerUser", user);
User aktuellerUser = actor.recall("aktuellerUser");
````
#### Mehr Informationen über Screenplay sind unter [Screenplay Fundamentals](https://serenity-bdd.github.io/docs/screenplay/screenplay_fundamentals) zu finden.

# Entwicklungsumgebung einrichten
* ## [Java 21](https://www.oracle.com/java/technologies/downloads/#jdk21-windows). Wähle JDK 21 -> Windows -> x64 Compressed Archive
  Herunterladen und unter C:\Program Files entpacken
* ## [Maven 3.9.9](https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.9.9/). Wähle _apache-maven-3.9.9-bin.zip_
  Herunterladen und unter C:\Program Files entpacken
* ## JAVA_HOME & MAVEN_HOME:
  * Öffene PowerShell als Administrator
  * Kopiere das folgende Script in PowerShell und ändere die Pfade entsprechend und dann Ausführen
    ````batch
    [System.Environment]::SetEnvironmentVariable('JAVA_HOME', 'C:\Pfad\zu\deinem\jdk', [System.EnvironmentVariableTarget]::Machine)
    [System.Environment]::SetEnvironmentVariable('MAVEN_HOME', 'C:\Pfad\zu\deinem\maven', [System.EnvironmentVariableTarget]::Machine)
    ````
* ## [Chrome Enterprise](https://chromeenterprise.google/download/)
* ## [Chrome-Driver](https://googlechromelabs.github.io/chrome-for-testing/#stable)
* ## [IntelliJ](https://www.jetbrains.com/de-de/idea/download/?section=windows) Klicke auf dem Dropdown "Herunterladen" und Whäle .zip (Windows)
  Herunterladen und unter C:\Program Files entpacken 

