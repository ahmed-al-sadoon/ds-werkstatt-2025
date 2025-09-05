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
ist eine von Serenity BDD bereitgestellte Erweiterung des Selenium-Interfaces WebElement. Sie bietet zusätzliche Methoden für Synchronisation, Abfragen und Interaktionen mit Webelementen. Dadurch werden Tests stabiler und der Code lesbarer. Mehr Informationen über Web-Elemente definieren sind unter [Serenity Page Elements](https://serenity-bdd.github.io/docs/guide/page_elements)
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








## https://googlechromelabs.github.io/chrome-for-testing/#stable
Serenity BDD is a library that makes it easier to write high quality automated acceptance tests, with powerful reporting and living documentation features. It has strong support for both web testing with Selenium, and API testing using RestAssured.


Serenity strongly encourages good test automation design, and supports several design patterns, including classic Page Objects, the newer Lean Page Objects/ Action Classes approach, and the more sophisticated and flexible Screenplay pattern.

The latest version of Serenity supports Cucumber 6.x.

## The starter project
The best place to start with Serenity and Cucumber is to clone or download the starter project on Github ([https://github.com/serenity-bdd/serenity-cucumber-starter](https://github.com/serenity-bdd/serenity-cucumber-starter)). This project gives you a basic project setup, along with some sample tests and supporting classes. There are two versions to choose from. The master branch uses a more classic approach, using action classes and lightweight page objects, whereas the **[screenplay](https://github.com/serenity-bdd/serenity-cucumber-starter/tree/screenplay)** branch shows the same sample test implemented using Screenplay.

### The project directory structure
The project has build scripts for both Maven and Gradle, and follows the standard directory structure used in most Serenity projects:
```Gherkin
src
  + main
  + test
    + java                        Test runners and supporting code
    + resources
      + features                  Feature files
     + search                  Feature file subdirectories 
             search_by_keyword.feature
```

Serenity 2.2.13 introduced integration with WebdriverManager to download webdriver binaries.

## The sample scenario
Both variations of the sample project uses the sample Cucumber scenario. In this scenario, Sergey (who likes to search for stuff) is performing a search on the internet:

```Gherkin
Feature: Search by keyword

  Scenario: Searching for a term
    Given Sergey is researching things on the internet
    When he looks up "Cucumber"
    Then he should see information about "Cucumber"
```

### The Screenplay implementation
The sample code in the master branch uses the Screenplay pattern. The Screenplay pattern describes tests in terms of actors and the tasks they perform. Tasks are represented as objects performed by an actor, rather than methods. This makes them more flexible and composable, at the cost of being a bit more wordy. Here is an example:
```java
    @Given("{actor} is researching things on the internet")
    public void researchingThings(Actor actor) {
        actor.wasAbleTo(NavigateTo.theWikipediaHomePage());
    }

    @When("{actor} looks up {string}")
    public void searchesFor(Actor actor, String term) {
        actor.attemptsTo(
                LookForInformation.about(term)
        );
    }

    @Then("{actor} should see information about {string}")
    public void should_see_information_about(Actor actor, String term) {
        actor.attemptsTo(
                Ensure.that(WikipediaArticle.HEADING).hasText(term)
        );
    }
```

Screenplay classes emphasise reusable components and a very readable declarative style, whereas Lean Page Objects and Action Classes (that you can see further down) opt for a more imperative style.

The `NavigateTo` class is responsible for opening the Wikipedia home page:
```java
public class NavigateTo {
    public static Performable theWikipediaHomePage() {
        return Task.where("{0} opens the Wikipedia home page",
                Open.browserOn().the(WikipediaHomePage.class));
    }
}
```

The `LookForInformation` class does the actual search:
```java
public class LookForInformation {
    public static Performable about(String searchTerm) {
        return Task.where("{0} searches for '" + searchTerm + "'",
                Enter.theValue(searchTerm)
                        .into(SearchForm.SEARCH_FIELD)
                        .thenHit(Keys.ENTER)
        );
    }
}
```

In Screenplay, we keep track of locators in light weight page or component objects, like this one:
```java
class SearchForm {
    static Target SEARCH_FIELD = Target.the("search field")
                                       .locatedBy("#searchInput");

}
```

The Screenplay DSL is rich and flexible, and well suited to teams working on large test automation projects with many team members, and who are reasonably comfortable with Java and design patterns. 

### The Action Classes implementation.

A more imperative-style implementation using the Action Classes pattern can be found in the `action-classes` branch. The glue code in this version looks this this:

```java
    @Given("^(?:.*) is researching things on the internet")
    public void i_am_on_the_Wikipedia_home_page() {
        navigateTo.theHomePage();
    }

    @When("she/he looks up {string}")
    public void i_search_for(String term) {
        searchFor.term(term);
    }

    @Then("she/he should see information about {string}")
    public void all_the_result_titles_should_contain_the_word(String term) {
        assertThat(searchResult.displayed()).contains(term);
    }
```

These classes are declared using the Serenity `@Steps` annotation, shown below:
```java
    @Steps
    NavigateTo navigateTo;

    @Steps
    SearchFor searchFor;

    @Steps
    SearchResult searchResult;
```

The `@Steps`annotation tells Serenity to create a new instance of the class, and inject any other steps or page objects that this instance might need.

Each action class models a particular facet of user behaviour: navigating to a particular page, performing a search, or retrieving the results of a search. These classes are designed to be small and self-contained, which makes them more stable and easier to maintain.

The `NavigateTo` class is an example of a very simple action class. In a larger application, it might have some other methods related to high level navigation, but in our sample project, it just needs to open the DuckDuckGo home page:
```java
public class NavigateTo {

    WikipediaHomePage homePage;

    @Step("Open the Wikipedia home page")
    public void theHomePage() {
        homePage.open();
    }
}
```

It does this using a standard Serenity Page Object. Page Objects are often very minimal, storing just the URL of the page itself:
```java
@DefaultUrl("https://wikipedia.org")
public class WikipediaHomePage extends PageObject {}
```

The second class, `SearchFor`, is an interaction class. It needs to interact with the web page, and to enable this, we make the class extend the Serenity `UIInteractionSteps`. This gives the class full access to the powerful Serenity WebDriver API, including the `$()` method used below, which locates a web element using a `By` locator or an XPath or CSS expression:
```java
public class SearchFor extends UIInteractionSteps {

    @Step("Search for term {0}")
    public void term(String term) {
        $(SearchForm.SEARCH_FIELD).clear();
        $(SearchForm.SEARCH_FIELD).sendKeys(term, Keys.ENTER);
    }
}
```

The `SearchForm` class is typical of a light-weight Page Object: it is responsible uniquely for locating elements on the page, and it does this by defining locators or occasionally by resolving web elements dynamically.
```java
class SearchForm {
    static By SEARCH_FIELD = By.cssSelector("#searchInput");
}
```

The last step library class used in the step definition code is the `SearchResult` class. The job of this class is to query the web page, and retrieve a list of search results that we can use in the AssertJ assertion at the end of the test. This class also extends `UIInteractionSteps` and
```java
public class SearchResult extends UIInteractionSteps {
    public String displayed() {
        return find(WikipediaArticle.HEADING).getText();
    }
}
```

The `WikipediaArticle` class is a lean Page Object that locates the article titles on the results page:
```java
public class WikipediaArticle {
    public static final By HEADING =  By.id("firstHeading");
}
```

The main advantage of the approach used in this example is not in the lines of code written, although Serenity does reduce a lot of the boilerplate code that you would normally need to write in a web test. The real advantage is in the use of many small, stable classes, each of which focuses on a single job. This application of the _Single Responsibility Principle_ goes a long way to making the test code more stable, easier to understand, and easier to maintain.

## Executing the tests
To run the sample project, you can either just run the `CucumberTestSuite` test runner class, or run either `mvn verify` or `gradle test` from the command line.

By default, the tests will run using Chrome. You can run them in Firefox by overriding the `driver` system property, e.g.
```json
$ mvn clean verify -Ddriver=firefox
```
Or
```json
$ gradle clean test -Pdriver=firefox
```

The test results will be recorded in the `target/site/serenity` directory.

## Generating the reports
Since the Serenity reports contain aggregate information about all of the tests, they are not generated after each individual test (as this would be extremenly inefficient). Rather, The Full Serenity reports are generated by the `serenity-maven-plugin`. You can trigger this by running `mvn serenity:aggregate` from the command line or from your IDE.

They reports are also integrated into the Maven build process: the following code in the `pom.xml` file causes the reports to be generated automatically once all the tests have completed when you run `mvn verify`?

```
             <plugin>
                <groupId>net.serenity-bdd.maven.plugins</groupId>
                <artifactId>serenity-maven-plugin</artifactId>
                <version>${serenity.maven.version}</version>
                <configuration>
                    <tags>${tags}</tags>
                </configuration>
                <executions>
                    <execution>
                        <id>serenity-reports</id>
                        <phase>post-integration-test</phase>
                        <goals>
                            <goal>aggregate</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
```

## Simplified WebDriver configuration and other Serenity extras
The sample projects both use some Serenity features which make configuring the tests easier. In particular, Serenity uses the `serenity.conf` file in the `src/test/resources` directory to configure test execution options.  
### Webdriver configuration
The WebDriver configuration is managed entirely from this file, as illustrated below:
```java
webdriver {
    driver = chrome
}
headless.mode = true

chrome.switches="""--start-maximized;--test-type;--no-sandbox;--ignore-certificate-errors;
                   --disable-popup-blocking;--disable-default-apps;--disable-extensions-file-access-check;
                   --incognito;--disable-infobars,--disable-gpu"""

```

Serenity uses WebDriverManager to download the WebDriver binaries automatically before the tests are executed.

### Environment-specific configurations
We can also configure environment-specific properties and options, so that the tests can be run in different environments. Here, we configure three environments, __dev__, _staging_ and _prod_, with different starting URLs for each:
```json
environments {
  default {
    webdriver.base.url = "https://duckduckgo.com"
  }
  dev {
    webdriver.base.url = "https://duckduckgo.com/dev"
  }
  staging {
    webdriver.base.url = "https://duckduckgo.com/staging"
  }
  prod {
    webdriver.base.url = "https://duckduckgo.com/prod"
  }
}
```

You use the `environment` system property to determine which environment to run against. For example to run the tests in the staging environment, you could run:
```json
$ mvn clean verify -Denvironment=staging
```

See [**this article**](https://johnfergusonsmart.com/environment-specific-configuration-in-serenity-bdd/) for more details about this feature.

## Want to learn more?
For more information about Serenity BDD, you can read the [**Serenity BDD Book**](https://serenity-bdd.github.io/theserenitybook/latest/index.html), the official online Serenity documentation source. Other sources include:
* **[Learn Serenity BDD Online](https://expansion.serenity-dojo.com/)** with online courses from the Serenity Dojo Training Library
* **[Byte-sized Serenity BDD](https://www.youtube.com/channel/UCav6-dPEUiLbnu-rgpy7_bw/featured)** - tips and tricks about Serenity BDD
* For regular posts on agile test automation best practices, join the **[Agile Test Automation Secrets](https://www.linkedin.com/groups/8961597/)** groups on [LinkedIn](https://www.linkedin.com/groups/8961597/) and [Facebook](https://www.facebook.com/groups/agiletestautomation/)
* [**Serenity BDD Blog**](https://johnfergusonsmart.com/category/serenity-bdd/) - regular articles about Serenity BDD
