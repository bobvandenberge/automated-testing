# Automated front-end testing	
## Soorten testing (Bron 1 en 21)
Binnen het onderwerp geautomatiseerd front-end testing kun je veel kanten op. Afhankelijk van het soort project dat je aan het ontwikkelen bent kun je verschillende - of natuurlijk meerdere - soorten tests inzetten. Volgens Eric Elliton - schrijver van verschillende bekende boeken zoals Programming JavaScript Applications - zou het schrijven van tests zou voor elke developer een belangrijk onderdeel van zijn of haar werk proces moeten zijn. Met name Test Driven Development - ook wel TDD - zouden developers volgens Elliot moeten kunnen dromen. Ondanks dat dit proces op voorhand meer tijd zal kosten zul je er ter zijner tijd vanzelfsprekend ervaring in krijgen en als maar beter in worden. Tevens kan het veel bugs voorkomen alvorens ze op acceptatie of productie geplaatst worden. Het oplossen van bugs kan namelijk erg veel tijd in beslag nemen. Hierbij moet je niet alleen denken aan verhelpen van het probleem, maar het gehele proces. Deze stappen zijn opgebouwd op het artikel van Eric Elliot en uit persoonlijke ervaringen binnen Label A:

- Klant of gebruikers ondervinden problemen met hun product. De gebruikerservaring is onderbroken en wordt een bug ingeschoten bij het support team
- Elke bug zal gevalideerd moeten worden door een support medewerker
- De bug gaat vervolgens naar het development team. Daar moet gekeken worden wie er tijd heeft op dit probleem op te pakken
- Een dergelijke onderbreking kost voor een developer al vaak minimaal 20 minuten om uit te zoeken waar het probleem door veroorzaakt was
- Het daadwerkelijke probleem moet worden opgelost
- De ticket gaat terug naar support medewerker die het testen
- De oplossing moet naar productie worden gezet en de klant moet hiervan op de hoogte worden gesteld

Dit is een proces dat per bug al gauw aan aantal uren in beslag kan nemen. Een aantal problemen welke hierdoor veroorzaakt kunnen worden zijn developers die volledig uit de flow van hun huidige project raken, of erger een deadlines niet halen. Boze klanten en (bij herhalende bugs)  zou dit zelfs klanten kunnen kosten. 

Om onder andere deze redenen zijn automatische testen ontstaan. Omdat een project verschillende aspecten heeft zijn er ook verschillende soorten tests. Binnen de front-end wereld zijn drie soorten testing het meest voorkomend; unit-, integration en functional (ook wel end-to-end of user-interface) testing. 
Deze tests kunnen naast elkaar leven. Het is niet de bedoeling dat je een keuze moet maken over welke soort test je gaat schrijven. Iedere soort test heeft zijn eigen rol. Voor de meeste applicaties zal je een unit test moeten schrijven, maar ook functional tests zijn vaak belangrijk, omdat een duidelijk beeld te geven van wat de gebruiker ervaart. De integration test wordt vaak alleen toegepast bij grotere applicaties: 

- ~Unit tests~ testen of individuele componenten werken zoals verwacht. Meestal zijn dit losse kleine componenten of functies in een groter component. Deze tests worden op hetzelfde uitgevoerd als linting; tijdens je development proces. Hierdoor moeten deze tests snel zijn, daarom geen async acties
- ~Integration tests~ testen of de samenwerkingen van verschillende componenten werken zoals verwacht. Denk hierbij aan het ophalen van data uit een API en dit doorgeven aan een component, maar bijvoorbeeld ook het samenwerken van verschillende classes
- ~Functional tests~ worden gedaan vanuit het oogpunt van de gebruiker. Ze voeden de user interface met input. Omdat hiermee eigenlijk de gehele applicatie word getest - vanaf hoe de UI er uit ziet en reageert tot de calls die gedaan worden naar de back-end - wordt dit ook wel end-to-end testing genoemd

Volgens Bart Waardenburg is er ook nog een vierde level van testing; static analysis. Hieronder vallen tools gebruikt voor linting zoals ESLint of JSLint en static type libraries zoals Typescript of Flow. 

#### Voorbeelden
Wanneer je wilt URLs naar bijbehorende pagina’s laten verwijzen door middel van route handlers. Een Unit test kan worden uitgevoerd om te kijken of het juiste component wordt ingeladen bij een specifieke route. Maar als je vervolgens wilt testen of er bij een POST van de URL een rij word toegevoegd aan de database zul je een Integration test moeten gebruiken. Typische functional tests worden gebruik voor “happy-paths” zoals een login- en registratie formulier of aankoop flows. Een andere goed voorbeeld van een functional test is infinity scrolling. Worden er nieuwe elementen geladen wanneer de gebruiker de onderkant van de pagina bereikt?

## Begrippen (Bron 3)
- **Testing Structure**: Organisatie van je tests. Meest voorkomen in de vorm van behavior driven development (BDD). Dit kan lijken op een acceptatie criteria:
``` javascript
describe('calculator', function() {
  // Describes a module with nested "describe" functions
  describe('add', function() {
    // Specify the expected behavior
    it('should add 2 numbers', function() {
       // Use assertion functions to test the expected behavior    
    })
  })
})
``` // Mocha

- **Assertions**: Functies die kijken of de uitkomst is wat je verwachting: 
``` javascript
assert.typeOf(foo, 'string')
assert.equal(foo, 'bar')
``` // Chai

- **Spies**: Informatie over functies. Bijvoorbeeld hoe vaak iets wordt aangeroepen:
``` javascript
it('should call method once with the argument 3', () => {
  const spy = sinon.spy(object, 'method')

  spy.withArgs(3)
  object.method(3)
  assert(spy.withArgs(3).calledOnce)
})
``` // Sinon

- **Stubs**: Vervangt functies om verwachtte uitkomst te simuleren:
``` javascript
sinon.stub(user, 'isValid').returns(true)
``` // Sinon

- **Mocks**: Nadoen van modules of functies om een bepaalde output te genereren. Sinon kan bijvoorbeeld server faken voor snelle, verwachte output. Een ander voorbeeld hiervan zijn het vervangen van SVG’s. Gezien deze niet erg belangrijk zijn voor tests kunnen ze vervangen worden met een mock, mocht je problemen ondervinden met het inladen.

-  **Snapshots**:  Vergelijk de uitkomst van een test met eerdere uitkomsten. Een component wordt omgezet naar JSON, wat later vergeleken zal worden met eerdere outputs. Hierdoor kunnen wijzigingen in de UI opgemerkt worden. 
``` javascript
it('renders correctly', () => {
  const linkInstance = (
    <Link page="http://www.facebook.com">Facebook</Link>
  )

  const tree = renderer.create(linkInstance).toJSON()
  expect(tree).toMatchSnapshot()
})
``` // Jest (Jasmine)

Dit zorgt voor een bug report zoals onderstaand:
![](Automated%20front-end%20testing/Automated%20front-end%20testing/4C0A8309-C549-4DDC-A64F-07ADA204960F.png)
 
## Tools
Voor de verschillende soorten testing zijn er verscheidene tools die je kunt gebruiken. Hieronder een overzicht per test soort.

### Unit tests (Bron 2, 3, 4 en 10)
Er zijn een aantal grote Javascript frameworks om unit tests mee uit te kunnen voeren. 
Het belangrijkste framework is naar alle waarschijnlijkheid Mocha. Dit is een framework dat sinds 2011 een speler op de markt van software testing is en daarmee een oude rot in het vak. De grote tegenhanger is sinds relatief korte periode Jest. Dit door Facebook Open Source ontwikkelde framework is een shell van Jasmine met erg veel uitbreidingen en eigen functionaliteiten. Ondanks dat het door Facebook is ontwikkeld hoeft het niet per definitie gebruikt te worden voor React applicaties. Wel werkt het erg fijn samen. Jest bestaat sinds 2015, maar begint pas sinds de herschrijving van vorig jaar echt aan populariteit te winnen. De grootste verschillende zouden liggen bij het gemak qua configuratie, snelheid en de mogelijkheden zonder het gebruik van externe partijen. Hieronder een vergelijking:

**Jest** 
- Nieuw framework, weinig extra libraries en tooling
- Snapshot testing: output vergelijken met een eerdere output
- Maakt gebruikt van Jasmine, een andere testing library (op de roadmap staat wel om dit er uit te halen, omdat het te oud is en niet meer onderhouden wordt)
- Parallel testing waardoor het erg snel aanvoelt i.v.m. andere frameworks. Zo schijnt Jest 6x sneller te zijn dan Mocha met veel en grote tests
- Plaatst tests in een  `__tests__`  map, of een `.spec.js` / `.test.js` extensie
- Build-in tool om uit te lezen hoeveel procent van je code gedekt is door tests

#### Voorbeelden
``` javascript
const sum = require('./sum');
test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
``` // Jest

**Mocha**
- Relatief oud en volwassen framework. Breed en veel gebruikt, grote community
- Flexibel framework, je kunt zelf koppelen wat je nodig hebt. Weinig is ingebouwd
- Eigenlijk onafscheidelijk van de  veelgebruikte assertion library Chai
- Voor stubbing wordt Sinon veel gebruikt, mocking (Sinon of Enzyme) etc.
- Vraagt meer configuratie, het zelf koppelen van libraries en frameworks kost meer tijd
- Zijn erg veel plug-ins voor te vinden met bijv. features die Jest/Jasmine niet hebben

#### Voorbeelden
``` javascript
var assert = require('assert');
describe('Array', function() {
  describe('#indexOf()', function() {
    it('should return -1 when the value is not present', function() {
      assert.equal(-1, [1,2,3].indexOf(4));
    });
  });
});
``` // Mocha

> "In short, if you want to “just get started” or looking for a fast framework for large projects, go with Jest. If you want a very flexible and extendable configuration, go with Mocha."  

### Integration tests (Bron 9, 15 en 16)
Ook voor het schrijven van integration tests zijn er verschillende mogelijkheden. Jest heeft met al zijn ingebouwde mocking functie een goede voorsprong. Maar voor uitgebreide mocking van slimme componenten (redux functionaliteiten) zul je een library als Enzyme moeten gebruiken. Mocha staat daar redelijk gelijk aan, ook hier zul je extra libraries moeten koppelen om dezelfde functionaliteit te krijgen. 

Om bijvoorbeeld gekoppelde componenten op basis van een route te testen, of API requests die via een reducer verlopen te verifiëren zul je libraries als redux-router en redux-thunk kunnen koppelen. Ook hierin verschillen Mocha en Jest zullen weinig, voor beide is extra configuratie nodig. 
Waarin wel een verschil zit bij deze libraries zijn - zoals eerder vernoemd in de vergelijking - de prestaties bij grotere applicaties. In het voorbeeld van AirBnb ging de volledige duur met de omgebouwde Jest  tests meer dan de helft achteruit. Een enorme prestatieverbetering. Van ongeveer 12 minuten met Mocha en Chai naar minder dan 5 minuten met Jest. 

### Functional tests (Bron 11, 17 en 18)
Tot slot hebben we de verschillende tools in de hoek van functional testing. Hier zijn een aantal grote en al lang meedraaiende spelers. CasperJS is een bekende. Geschreven in Python, met een NodeJS shel te installeren via NPM. CasperJS maakt gebruik van PhantomJS, een headless webkit oplossing om browsers te simuleren. Verschillende soorten tests kunnen gebruik maken van PhantomJS om uitgevoerd te worden in een browser, een techniek die voor end-to-end testing nagenoeg essentieel is.  

Een vergelijkbare speler van PhantomJS is Selenium. Beide bootsen een browser na en worden daarom ook wel WebDrivers genoemd. Een belangrijk verschil is dat PhantomJS beperking heeft tot webkit browsers en Selenium kan - met de juiste driver - nagenoeg elke omgeving nabootsen. 

NightwatchJS is een NodeJS testing framework gebaseerd op Selenium. Voordelen van NightwatchJS zijn de leesbare syntax en goede documentatie. Het is wel een relatief nieuw framework waardoor de community support een minpunt is.

Bij een eerder project heb ik mij enigszins verdiept in het gebruik van deze tool in combinatie met een Chrome Driver voor Selenium en Mocha. 
Onderstaand een voorbeeld uit dat project van een pagina die items uit een webshop toont. Nadat de resultaten geladen zijn wordt de pagina automatisch naar beneden gescrollt, waarna er nieuwe projecten ingeladen worden; infinite scrolling. 
``` javascript
module.exports = {
  'before': (browser) => {
    browser
      .resizeWindow(1280, 800)
      .url('http://localhost:6005/results/iphone')
      .waitForElementVisible('body', 1000);
  },

  'check default elements visible': (browser) => {
    browser
      .waitForElementVisible('.results > .result', 2000)
      .expect.element('.result:nth-of-type(10)').to.be.present;
  },

  'scroll to last element': (browser) => {
    browser
      .getLocationInView('.result:nth-of-type(10)', (result) => {
        browser
          .waitForElementVisible('.result:nth-of-type(20)', 2000);
      });
  },

  'after': (browser) => browser.end(),
};
``` // NightwatchJS

Een nadeel van het gebruik van WebDrivers is dat de hoge instapdrempel en configuratie redelijk veel tijd vragen. Dit kan het proces vertragen, maar kan voor grote applicaties wel degelijk waarde bieden. 

Met WebdriverIO zou je hier omheen kunnen. Dit framework wrapped Selenium en biedt enkele basis functionaliteiten. Hiermee zul je enigszins aan flexibiliteit moeten inleveren. TestCafe is een alternatief voor Selenium tools en is makkelijk in gebruik. Dit injecteert in de browser en kan daarmee voor alles browsers - zelfs mobiel - gebruikt worden. 

> "In short, if you want to “just get started” with the most simple set-up, and if you want to test many environments easily, go with TestCafe. If you want to go with the flow and have maximum community support, if you need to be able to write tests not only in JS, Selenium is the way to go."  

Jest heeft standaard een optie genaamd Snapshot testing. Dit kan een redelijke vervanging zijn voor het testen van de UI elementen, maar dit betekend nog niet dat een gebruikerservaring op een Selenium-achtige manier getest kan worden.
De UI uitkomst van snapshots kunnen met elkaar vergeleken worden om zo te verzekeren dat een gebruiker altijd dezelfde interface voorgeschoteld krijgt. Functionaliteit en ervaring kunnen hiermee niet gemeten worden. 

## Waarom (Bron 21, 22, 23, 24 en 25)
Hierboven hebben beschreven wat de verschillende soorten tests doen. Waarom schrijven we dan niet alleen maar functional tests? Deze worden niet voor niets end-to-end tests genoemd? Kunnen we hiermee niet alles testen? 

Zodra iemand een dergelijk onderwerp op tafel legt zal het daarna hoogstwaarschijnlijk over de testing-pyramid gaan. Afgelopen oktober gaf Bart Waardenburg van de ANWB hier over een talk tijdens React Amsterdam. 

![](Automated%20front-end%20testing/Automated%20front-end%20testing/Schermafbeelding%202017-12-09%20om%2018.59.26.png)

Hij geeft in zijn presentatie aan dat user-interface tests vaak niet gemaakt worden als gevolg van moeilijkheidsgraad en slechte prestaties. Ook Martin Fowler - bekend software developer - omschrijft user-interface testing als prijzig en langzaam.  

> "Its essential point is that you should have many more low-level unit tests than high level end-to-end tests running through a GUI."  

Mike Cohn van Mountain Goat Software was de eerste die de pyramide techniek introduceerde in zijn boek *Succeeding with Agile*. In zijn artikel hierover legt hij de verschillen kort en bondig uit. 
Zo worden unit tests vaak geschreven omdat deze een duidelijk bug report geven. Zo schrijft hij "een unit tests geeft specifieke data aan de developer zoals; er is een error op line 47". Mocht je een functional test schrijven moet je het vaak af doen met een error als "Er is een probleem met het ophalen van gebruikers uit de database", wat meer dan honderden regels of code kan bevatten.  Zoals je zult begrijpen is dit een reden om meer unit tests te schrijven. 
Een ander simpel voorbeeld, de bekende rekenmachine. Jouw code bevat een functie  die twee getallen vermenigvuldigt. Om dit met een functional tests te volbrengen moet een webdriver worden opgestart, moeten de inputs gevonden en gevoed worden in de interface en moet er gekeken worden of de juiste wijzigingen worden doorgevoerd aan de interface. Bij een unit test zul je slechts twee getallen in hoeven te vullen en de uitkomst te bekijken. Veel sneller, efficiënter en dus goedkoper. 

Ook Google besteed er op hun blog aandacht aan. Zo omschrijven zij functional tests als onbetrouwbaar. Het kan vaak voorkomen dat een test de ene dag faalt, maar de volgende dag succesvol verloopt. Dit worden "flaky" tests genoemd. Dergelijke problemen kunnen voorkomen door internet connecties, gekoppelde databases etc. Een andere belangrijk argument is dat wanneer er alleen end-to-end tests gebruikt worden daar vaak nog kleinere bugs achter zitten. Zoals zij het omschrijven:

> "A failing or passing test does not directly benefit the user.    
> While this statement seems shocking at first, it is true. If a product works, it works, whether a test says it works or not. If a product is broken, it is broken, whether a test says it is broken or not. So, if failing tests do not benefit the user, then what does benefit the user?"  

Nogmaals samenvattend. Elke tests type speelt dus zijn eigen rol. Snellere en betrouwbare unit tests zorgen er voor dat geïsoleerde delen van je code worden getest op functionaliteit. Het isoleert fouten en geeft duidelijke bug reports. Om die code te testen in combinatie met andere delen zijn er integration tests. Gesimplificeerd pak je twee (of meerdere) van elkaar afhankelijk unit tests en kijk je hoe deze samenwerken. Ook API’s en derde partijen worden hieraan toegevoegd. Toch wil je nog een klein aantal functional tests om het gehele systeem of module te verifiëren. Hoger in de pyramide zijn grotere, maar wel minder aantal tests.

## Implementatie bij Label A (Bron 6, 7, 12, 13 en 19)
Naast dat ik het zelf een erg interessant onderwerp vind kan dit onderzoek voor Label A ook impact hebben. Momenteel wordt er te weinig (in sommige projecten niets) gedaan met testing. Het zou om die reden erg nuttig zijn om een goede uitleg te geven over de implementatie in de huidige workflow. Dit betekend onder andere een integratie met de React-Prime boilerplate van Jorn. Er zullen standaarden moeten komen om iedere developer snel te kunnen laten testen.

Na eerder genoemd onderzoek heb ik besloten mij in eerste instantie te richten op unit en integration testing. Ook komt er met Jest een soort functional testing aan te pas door middel van snapshots. Doel van de implementatie zal zijn om zowel kleine domme React componenten als modulaire Redux ducks (API requests) en slimme componenten te testen. De reden dat ik niet voor functional testing kies is omdat het gebruik van WebDrivers (voor volledige controle en flexibiliteit) nog een extra laag  toegevoegd waar simpelweg niet genoeg tijd voor is. Ook zijn - zoals in de "Waarom sectie uitgelegd" - unit tests de fundatie voor het testen. 

Gebaseerd op het verrichte onderzoek  heb ik - zonder zelf erg veel grote vergelijkingen te doen - gekozen om mij volledig te richten op Jest in plaats van Mocha. Het gemak, en vooral de snelheid van Jest hebben mij doen overtuigen van de kracht. Ook de samenwerking met React is een grotere bijkomstigheid. 

Om testing op te zetten zijn er een aantal vereiste aanpassingen of toevoegingen aan het project. Om te beginnen moet de setup van een project aangepast worden. Het belangrijkste is dat er configuratie voor Jest bij moet komen. Zoals eerder besproken valt dit voor Jest in vergelijking met Mocha enigszins mee, maar je moet - vanzelfsprekend - wel weten wat je precies moet doen en het kostte mij enige tijd om dit door te hebben. De setup ziet er als volgt uit en deze zal ik doornemen:

```javascript
  "jest": {
    "verbose": true,
    "transform": {
      "^.+\\.jsx?$": "./__test__/__mock__/jestPreprocessor.js"
    },
    "moduleDirectories": [
      "node_modules",
      "src/app",
      "src/app/components",
      "src/app/static"
    ],
    "moduleNameMapper": {
      "^-!svg-react-loader.*$": "./__test__/__mock__/fileMock.js"
    },
    "snapshotSerializers": [
      "enzyme-to-json/serializer"
    ],
    "setupFiles": [
      "./__test__/__settings__/shim.js",
      "./__test__/__settings__/setup.js"
    ]
  },
```

1. Verbose: zorgt er voor dat Jest uitgebreide weergave geeft het bugreport
2. Transform: zorgt er voor dat alle `.jsx` bestanden behandeld worden. In dit geval gaat het om `.svg` en `.css` bestanden die door `babel-jest` worden gestuurd
3. moduleDirectories: hierdoor wordt een `import` naar de juiste plek gestuurd
4. moduleNameMapper: behandeld en mockt `.svg` bestanden
5. snapshotSerializers: zorgt er voor dat componenten omgezet worden naar JSON. Dit is handig voor het normaal kunnen lezen van snapshots
6. setupFiles: enkele standaard functies die in ieder test bestand gebruikt worden. Denk aan het polyfillen van ES6/7 functies, globaal beschikbaar maken van variabelen etc.

Daarnaast zijn er een aantal belangrijke aanpassingen die gedaan moeten worden om onze Boilerplate te ondersteunen. 

1. De API helper moet in een mock gezet worden. Omdat we de API url niet kunnen ophalen aan de hand van een variabele moet deze er hardcoded ingezet worden.
2. Er moeten extra `export` statements worden toegevoegd in componenten. Om van een "slim" component "domme" functionaliteit te testen:
``` javascript
// instead of this
class SelectProjects extends React.Component { ... }
// do this
export class SelectProjects extends React.Component { ... }
// and with both still do
export default connect(...)(SelectProjects);
```
3. Binnen een duck moeten API requests een `return` krijgen. Ook zullen de zullen de `initialState` en constants geëxporteerd moeten worden. Op deze manier kun je met de waardes werken wanneer je het bestand importeert in een test:
```javascript
export const getProjectById = id => (dispatch, setState, api) => {
	  // return the API requests
    return api.get({ path: `/projects/${id}` })
        .then(response => dispatch(projectsByIdSuccess(response)))
        .catch((err) => dispatch(projectsFailed(err)));
};
```


## Problemen
- Krijg componenten niet gemocked… 
[Low effort, high value. Integration tests in Redux apps.](https://hackernoon.com/low-effort-high-value-integration-tests-in-redux-apps-d3a590bd9fd5)

- Geen goede mocking van connected components
[Real integration tests with React, Redux and Router](https://medium.freecodecamp.org/real-integration-tests-with-react-redux-and-react-router-417125212638)

- SVG loading (fix below)
[Write jest tests for project with Webpack – Naitro – Medium](https://medium.com/@Yodairish/write-jest-tests-for-webpack-project-823ccda3156)

- Babel Runtime Errors
![](Automated%20front-end%20testing/Automated%20front-end%20testing/Schermafbeelding%202017-12-01%20om%2013.47.45.png)

- Mocken van specifieken elementen zoals de `apiHelper.js`


## Advies en bevindingen (Bron 14)
Aan het einde van vorige jaar ging het artikel "How it feels to learn Javascript in 2016) "viral". Hierin werd de vermoeidheid van alle verschillende frameworks en libraries besproken. Wanneer je met Javascript wilt beginnen kan dit een zwakte zijn van de taal. Waar moet ik beginnen? Welk framework of library kan ik het best gebruiken? Door de bomen zie je het bos niet meer. Tegelijkertijd is dit ook de kracht van Javascript. De taal is hierdoor erg flexibel en breed inzetbaar.  

Ondanks dat ik tegenwoordig mijn weg aardig goed weet te vinden binnen het Javascript landschap is dit wel hoe ik mij opnieuw voelde toen ik begon aan het onderwerp testing. Ten eerste heb je de drie verschillende soorten testing die allemaal weer hun eigen frameworks en best-practices hebben en daarnaast komt nog als fijne bijkomstigheid dat het testen een aantal nieuwigheden met zich mee brengt. Dit heeft onder andere te maken met de denkwijze. Wat zijn belangrijke punten om in mijn applicatie te testen? En ga ik gebruik maken van Test Driven Development; het vooraf bedenken van de tests, deze uitschrijven en dan pas daadwerkelijk gaan bouwen? Door de schaal en de leercurve van het testen heb ik dit nog niet goed kunnen uitproberen, maar ik kan me voorstellen dat dit een manier van werken is weer enige ervaring voor nodig is. Je moet te voren heel duidelijk hebben wat je precies gaat bouwen en welke functionaliteiten er moet komen.

Gaan de weg ben ik er wel achter gekomen wat een goede test is. Duidelijke omschrijvingen is daarbij extreem belangrijk. Uitgeschreven testen zullen vaak overeen komen met de acceptatie criteria van een user story. Daarnaast is het belangrijk duidelijk bug reports te genereren. Bij welke verwachting (`it`) breekt de test, en wat gaat er precies fout? Zo kun je specifieke delen van je code gemakkelijk vinden en snel oplossen. 

Het schrijven van goede tests kan erg nuttig zijn. Het zal er voor zorgen dat veel bugs voorkomen worden, wat erg veel tijd voor het development- en support team kán schelen. Het melden, opzoeken, reproduceren en oplossen van een bug kan zo een aantal uur per geval kosten. 
Ook zul je, wanneer je duidelijke integration tests schrijft de uitkomsten van een API betrouwbaar kunnen weergeven. Hiermee kun je altijd testen of jou API werkt zoals verwacht en kun je de uitkomst vergelijken met verwachtte uitkomsten om er zo voor te zorgen dat de applicatie altijd met de juiste data gevoed wordt. Met goede unit tests kun je daarentegen weer kleinere functies binnen een component testen. Erg nuttig om bijvoorbeeld een filter- of zoekfunctie functie te testen. Doet de functie wat ik verwacht als ik er specifieke data naar toe stuur? 

Vanzelf sprekend heeft het schrijven van tests niet alleen maar voordelen. Eén van de belangrijkste nadelen is dat het tijd kost. Niet alleen in het schrijven van de tests, maar ook in het bijscholen van je personeel. Hoe ver wil je gaan in het testen van je applicatie? Ga je voor elke functie en API requests een test schrijven, of ga je nog verder en ga je ook routes testen. Denk hierbij aan het weergeven van 404’s, of categorie / archief pagina’s. 
Ook kan het het development proces verlangzamen. Als er zware tests worden geschreven die elke keer runnen wanneer je code is aangepast zal het meer tijd kosten voor je applicatie gereed is. Hierbij moeten developers goed oppassen en enkel de snelle en lichte tests direct laten uitvoeren. De grotere functionele- of integration tests zou je alleen tijdens een release naar productie moeten uitvoeren. Een voorbeeld hiervan tijdens met test process was het maken van één API call om de projecten uit Gitlab op te halen. Deze test deed er 3075ms (3 seconden) over. Ter vergelijking, een unit test voor een filter functie deed er slechts 7ms (0.007 seconden) over.  Wanneer je veel API requests hebt om te testen kan dit erg langzaam worden.

Al met al denk ik dat het zeker de moeite waard is om je developers bij te scholen en de basis van testing te leren. Zoek uit wat de beste tools zijn voor jou team, regel één middag vrij en maak een duidelijk gestructureerde workshop met de basis vaardigheden voor het testen. Laat je team experimenteren met het schrijven van tests voor zijn of haar project. Dit zal de professionaliteit, overdraagbaarheid en betrouwbaarheid van je project zeker ten goede komen. 

- - - -

~Bronnen~
1. https://www.sitepoint.com/javascript-testing-unit-functional-integration/
2. [Jest vs. Mocha for React.js Testing – Strengths & Weaknesses](https://spin.atomicobject.com/2017/05/02/react-testing-jest-vs-mocha/)
3. [An Overview of JavaScript Testing in 2017 – powtoon-engineering – Medium](https://medium.com/powtoon-engineering/a-complete-guide-to-testing-javascript-in-2017-a217b4cd5a2a)
4. [Snapshot Testing · Jest](https://facebook.github.io/jest/docs/en/snapshot-testing.html)
5. [JavaScript Unit Testing Performance · Jest](http://facebook.github.io/jest/blog/2016/03/11/javascript-unit-testing-performance.html)
6. [Writing Tests · Redux](https://redux.js.org/docs/recipes/WritingTests.html)
7. [GitHub - facebookincubator/create-react-app: Create React apps with no build configuration.](https://github.com/facebookincubator/create-react-app)
8. [Briefs | React Testing Patterns | 3 Minutes with Kent](https://www.briefs.fm/3-minutes-with-kent/49)
9. [Testing React Applications with Karma, Jest or Mocha – instea.sk](http://instea.sk/2016/08/testing-react-applications-with-karma-jest-or-mocha/)
10. [JavaScript unit testing frameworks: Comparing Jasmine, Mocha, AVA, Tape and Jest: javascript](https://www.reddit.com/r/javascript/comments/6dczux/javascript_unit_testing_frameworks_comparing/)
11. [Rogelio Guzman - Jest Snapshots and Beyond - React Conf 2017 - YouTube](https://www.youtube.com/watch?v=HAuXJVI_bUs)
12. [Testing a React-Redux app using Jest and Enzyme – Netscape – Medium](https://medium.com/netscape/testing-a-react-redux-app-using-jest-and-enzyme-b349324803a9)
13. ["Testing React Applications" with Max Stoiber - YouTube](https://www.youtube.com/watch?v=59Ndb3YkLKA&feature=youtu.be)
14. [How it feels to learn JavaScript in 2016 – Hacker Noon](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f)
15. [Real integration tests with React, Redux and Router](https://medium.freecodecamp.org/real-integration-tests-with-react-redux-and-react-router-417125212638)
16. [Unlocking Test Performance — Migrating from Mocha to Jest](https://medium.com/airbnb-engineering/unlocking-test-performance-migrating-from-mocha-to-jest-2796c508ec50)
17. [Top 5 Most Rated Node.js Frameworks for End-to-End Web Testing](https://medium.com/@adrian_lewis/top-5-most-rated-node-js-frameworks-for-end-to-end-web-testing-f8ebca4e5d44)
18. [google chrome - Casperjs/PhantomJs vs Selenium - Stack Overflow](https://stackoverflow.com/questions/14099770/casperjs-phantomjs-vs-selenium)
19. [GitHub - JBostelaar/react-prime: A starter kit to create comprehensive React apps with Redux and serverside rendering.](https://github.com/JBostelaar/react-prime)
20. [GitHub - erikras/ducks-modular-redux: A proposal for bundling reducers, action types and actions when using Redux](https://github.com/erikras/ducks-modular-redux)
21. [React and the Three Layers of Testing - Bart Waardenburg - YouTube](https://www.youtube.com/watch?v=L2yOoxzXmw8)
22. [That Testing Pyramid](https://wade.be/development/2017/02/20/that-testing-pyramid.html)
23. [The Forgotten Layer of the Test Automation Pyramid](https://www.mountaingoatsoftware.com/blog/the-forgotten-layer-of-the-test-automation-pyramid)
24. [Google Testing Blog: Just Say No to More End-to-End Tests](https://testing.googleblog.com/2015/04/just-say-no-to-more-end-to-end-tests.html)
25. [GTAC 2016: Presentations  |  Google Test Automation Conference       |  Google Developers](https://developers.google.com/google-test-automation-conference/2016/presentations)

~Other handy - not covered - stuff~
- [istanbul-js: A Javascript code coverage tool written in JS](https://gotwarlost.github.io/istanbul/) 
Testen hoeveel procent van je code gedekt is door unit tests.
- https://storybook.js.org/
Jest storybook - The UI Development Environment
- [GitHub - sapegin/jest-cheat-sheet: Jest cheat sheet](https://github.com/sapegin/jest-cheat-sheet)

