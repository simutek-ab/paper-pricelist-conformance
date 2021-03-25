# 1 Innehållsförteckning
- Revisionshistorik
- Bakgrund
- Syfte
- Krav och antaganden
  - Allmänt
  - Datafält
  - Kommunikation
- Validering

# 2 Revisionshistorik 

| Revision  | Datum | Av | Kommentar |
| ------------- | ------------- | ------------- | ------------- |
| 1.5 | 2021-03-25 | Michael Bergman | Justering kap. 5.2.24 ArticlePrice.Price avseende fasta kolumner i GKS3. |
| 1.4 | 2020-01-20 | Michael Bergman | Flyttat dokument till GitHub. |
| 1.3 | 2016-10-07 | Michael Bergman | Justering kap. 5.2.4. ClientId ska vara en tom sträng om prisfilen ej är kundunik. Förtydligande om WeightPerUnit. |
| 1.2 | 2014-06-19 | Michael Bergman | Justering kap. 5.2.26 till att även omfatta transportenhet. |
| 1.1 | 2014-05-19 | Michael Bergman | Uppdaterad för KEPA EDI version 2.0. |
| 1.0 | 2013-04-02 | Michael Bergman | |
 

# 3 Bakgrund
Organisationen KEPA släppte år 2012 en ny standard för elektroniskt utbyte av artiklar för primärt prisinformation och sekundärt teknisk information. Standarden avser att ersätta en tidigare standard från 1980-tal. Den nya standarden har avsiktligen utformats generell för att inte endast kunna innehålla information omkring finpapper utan även andra typer av artiklar. Standarden omfattar en filstruktur men saknar vidare användningsrestriktioner. 

# 4 Syfte
För att standarden ska vara praktiskt användbar vid utbyte av finpappersartiklar behöver standarden kompletteras med användningsrestriktioner. Med restriktioner för ett visst användningsområde kan det säkerställas att utbyte av information kommer lyckas utan risk för fel – antingen under importen eller senare vid kalkylering och pappersbeställning. Detta spar tid och pengar för alla parter som direkt eller indirekt kommer i kontakt med prisfilerna. 

Följande dokument inkluderar de krav och antaganden som Simutek kompletterat med för att kunna säkerställa att import till affärssystemet GKS ska vara genomförbar. Dokumentet kan komma revideras i det fall att problem identifieras som kan åtgärdas med nya eller förändrade restriktioner. 

# 5 Krav och antaganden
Följande krav eller antaganden har upprättats. 

Vid stora avvikelser från dessa kommer fil vid import att avisas i sin helhet. Vid avvikelser för enskilda artiklar kommer endast dessa artiklar att avisas och övriga artiklar kommer att läsas in. 

## 5.1 Allmänt

### 5.1.1 Teckenuppsättning 
En XML-fil ska innehålla en deklaration som definierar använd teckenuppsättning. Om möjligt rekommenderas teckenuppsättning ”UTF-8. 

### 5.1.2 Frivilliga fält 

Om uppgifter saknas för ett frivilligt fält ska fältet exkluderas. Värdet får inte anta värden såsom ”0”, ”N/A”, ”Saknas” etc. Fältens värde får inte vara ett schablonvärde. 

## 5.2 Datafält 

### 5.2.1 Version 
Värdet måste motsvara exakt den version av schemat som stöds. I dagsläget är detta ”2.0”. 

### 5.2.2 SupplierId 

Värdet måste motsvara pappersleverantörens organisationsnummer. Bindestreck är frivilligt. 

### 5.2.3 SupplierName 

Värdet måste vara ha minst 3 tecken. 

### 5.2.4 ClientId 

Om prisfilen är avsedd för en specifk kund ska värdet motsvara kundens organisationsnummer. Bindestreck är frivilligt. 

I annat fall ska värdet vara blankt (en tom sträng). 

### 5.2.5 ArticlePrice 

Listan måste innehålla åtminstone ett objekt. 

### 5.2.6 ArticlePrice.ArticleNumber 

Artikelnummer måste vara mellan 1-14 tecken samt innehålla tecknen a-z, A-Z eller 0-9. 

Två artikelnummer där endast skiftlägeskänslighet skiljer får inte förekomma, t.ex. ”OP1212A” och ”op1212A”. 

### 5.2.7 ArticlePrice.ArticleDisplayname 

Artikelbeskrivningen måste vara mellan 1-35 tecken, t.ex. ”SuperMaxArt”. 
Artikelbeskrivningen ska inte innehålla övrig information såsom ex. vikt, färg eller format. 

### 5.2.8 ArticlePrice.Grade 

Om angivet, ett heltalsvärde inom intervallet 1-5. 

### 5.2.9 ArticlePrice.Thickness 

Om angivet, ett värde mellan 10-10000. 

### 5.2.10 ArticlePrice.Weight 

Vikt måste vara ett värde större än 0. 

### 5.2.11 ArticlePrice.Width 

Bredd måste vara ett värde större än 0, samt mindre än 10000. 

### 5.2.12 ArticlePrice.Length 

Längd måste vara ett värde större än 0 om arkpapper. 

Om längd har utelämnats tolkas detta som ett rullpapper. 

### 5.2.13 ArticlePrice.WeightPerUnit 

Om angivet, måste vara ett värde mellan 0.001-100. 
”Per unit” tolkas som ”Per sales unit”. 

### 5.2.14 ArticlePrice.SalesUnit 

Försäljningsenhet (SalesUnit) ska vara ”KG” om enheten avser Kilo, ”ARK” om det avser styckenhet eller ”M2” om det avser kvadratmeter. 
 
Om något annat än dessa enheter anges kommer dessa artiklar att läsas in som ”ARK”. 

Värdet måste ha en längd större än 1 tecken. 

### 5.2.15 ArticlePrice.MinimumSalesUnit 

Måste vara ett värde större än 0. 

### 5.2.16 ArticlePrice.SalesMultiple 

Måste vara ett värde större än 0. 

Om ej Breakable, måste Packaging.SalesUnitPerPackagingUnit vara jämnt delbart med SalesMultiple. 

### 5.2.17 ArticlePrice.TransportPackagingUnit 

Om angivet, värdet måste ha en längd större än 1 tecken. 

Värdet måste vara unikt bland definierade enheter. 

### 5.2.18 ArticlePrice.SalesUnitPerTransportUnit 

Om angivet, ska vara ett heltal och ett värde större än 0. 

### 5.2.19 ArticlePrice.Packaging 

Listan måste innehålla exakt ett objekt. 

### 5.2.20 ArticlePrice.Packaging.PackagingUnit 

Förpackningsenhet ska vara ”PKT” om det avser paket. I annat fall bör förpackningsenheten utskrivas ”RIM” för rismarkerat. Om någon annan än dessa enheter anges kommer de artiklarna att läsas in som ”RIM”. 

Värdet måste ha en längd större än 1 tecken. 

Värdet måste vara unikt bland definierade enheter. 

Endast en förpackningsenhet får anges per artikel, trots att standarden möjliggör för att lägga in fler. 

### 5.2.21 ArticlePrice.Packaging.SalesUnitPerPackagingUnit 

Ett värde större än 0. 

### 5.2.22 ArticlePrice.Breakable 

Om ja, kommer exakt kvantitet i försäljningsenhet att beställas. 

Om nej, kommer endast multiplar av SalesUnitPerPackagingUnit att beställas.  

### 5.2.23 ArticlePrice.Currency 

Valuta måste vara ”SEK” för den svenska marknaden och ”NOK” för den norska marknaden. 

Olika valutor får inte förekomma i en och samma fil. 

### 5.2.24 ArticlePrice.Price 

Listan måste innehålla åtminstone ett objekt.

I GKS3 defineras fasta priskolumner (stafflade priser) via _Papper > Leverantörer_. Av denna anledning kräver importen att alla artilkar har exakt lika många Price-element. _Scale_ och _ScaleUnit_ kommer vidare ignoreras till fördel för de gränser som finns definerade i GKS3.

### 5.2.25 ArticlePrice.Price.Amount 

Måste vara ett värde större eller lika med 0.  

### 5.2.26 ArticlePrice.Price.Unit 

Som prisenhet accepteras endast för artikeln angiven försäljningsenhet, förpackningsenhet, transportenhet eller ”KG”. 

Värdet måste ha en längd större än 1 tecken. 

### 5.2.27 ArticlePrice.Price.Factor 

Värdet måste vara ett heltal större än 0. 

### 5.2.28 ArticlePrice.Price.Scale 

Måste vara ett värde större eller lika med 0. 

Prisgränserna ska tolkas som ”Från och med”. En angiven gräns ”250 ARK” med pris ”100 kr” innebär att detta pris gäller vid köp 250 styck eller mer, dock endast upp till eventuell nästkommande gräns. 

Den första angivna gränsen per ScaleUnit (normalpriset) bör vara angiven till gräns ”0 ARK”. I annat fall kommer den första angivna gränsen att tolkas som detta. 

### 5.2.29 ArticlePrice.Price.ScaleUnit 

För ScaleUnit gäller samma antaganden som för kapitel 5.2.26. 

### 5.2.30 Color 

Om angivet, ett värde mellan 3 – 20 tecken. 

 

## 6 Kommunikation 

Standarden omfattar ingen bestämmelse hur prisinformationsutbytet ska kommuniceras. Simutek kommer att stödja följande tillvägagångssätt: 

Import av fil sparad på lokal dator 

Import av fil tillgänglig via en publik URL, t.ex. ”http://www.papplev.se/pricelists/gks/customer02/pricelist-latest.xml” 

## 7 Validering 

För att förenkla för leverantörer som avser att ta fram prisfiler enligt detta dokument har Simutek tagit fram valideringsverktyg.
https://validator.simutek.se

 
