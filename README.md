# Localisation-of-WIR
This repository is a guideline to create automated WiR [Women in Red](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_Women_in_Red/Redlist_index) lists in your wiki

WiR is an international project among Wikipedia's to fight against gender gap in digital areas. The main idea is turning red links (which means no one created the article for the linked person) to blue ones (means an article exists) with the help of red lists (consist of recognized women with no article yet). 

## Steps
1. The data to create lists will be taken from [Wikidata](https://wikidata.org) by querying with [SPARQL](https://en.wikipedia.org/wiki/SPARQL).
    1. Querying list of categories (I used occupations as to categorize)
    2. Querying list of women under this category 
2. Lists of women under a category will be created (and updated automatically, periodically) by using [Listeria bot](https://www.wikidata.org/wiki/Wikidata:Listeria)

3. Turning files include wikitext to Wikipedia pages by using [PyWikiBot]() page from file script. (To prevent vandalism, please proceed with this step only if you know what you are doing. This will make a real edit on the Wikipedia you prefer.)

### 1. Get data
#### i. Get occupation list
Check out queryOccupationsExampleSPARQL.txt file or simply try the query on wikidata from [here](https://w.wiki/zt4)
#### ii. Get women list of an occupation 
Check out queryWomenByOccupationExampleSPARQL.txt file or simply try the query on wikidata from [here](https://w.wiki/ztA)
```
...
    ?item wdt:P31 wd:Q5 . # what to query? people
    ?item wdt:P21 wd:Q6581072 . # whose gender women
    ?item wdt:P106 wd:Q901 . # whose occupation is scientist
...
  FILTER (?linkcount >= 1) .       # only include items with 1 or more sitelinks (linked in other wikipedias)
...
    ?article schema:inLanguage "tr" . # not exists in Turkish Vikipedi (this should be changes according to your local Wikipeda language code)
...
ORDER BY DESC(?linkcount) #Ordered by number of links in other wikipedias, this can give an idea about the fame of women
```
### 2. Listeria bot usage
Now, all the lists we have created at Wikidata should be carried on Wikipedia. Listeria helps to use these queries to define a list usable in Wikipedia. Here is the [documentation](https://www.wikidata.org/wiki/Template:Wikidata_list) of the tool.

Here is the wikitext form of my implementation in Turkish Vikipedi:
```
{{Wikidata list|sparql=PREFIX schema: <http://schema.org/>
SELECT ?item ?itemLabel ?linkcount WHERE {
    ?item wdt:P31 wd:Q5 .
    ?item wdt:P21 wd:Q6581072 .
    ?item wdt:P106 wd:Q3400985 .
    ?item wikibase:sitelinks ?linkcount .
  FILTER (?linkcount >= 1) .       # only include items with 1 or more sitelinks
  FILTER NOT EXISTS {
    ?article schema:about ?item .
    ?article schema:inLanguage "tr" .
    ?article schema:isPartOf <https://tr.wikipedia.org/>
  }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "en,de,es,ar,fr" }
}
ORDER BY DESC(?linkcount)
LIMIT 300
|columns=number:#,label:Ad,P18,description:açıklama,P27,item:wikidata maddesi,?linkcount:site bağlantıları
|section=131
|min_section=3
|links=red
|thumb=128
|autolist=fallback
}}

{{Wikidata list end}}
```
### 3. PyWikiBot usage to create multiple files
PyWikiBot is a rich tool to make several operations on wikis. We will be using "pagefromfile" tool. Here is the [documentation](https://www.mediawiki.org/wiki/Manual:Pywikibot) (you may be needed to set your configuration settings first according to your account information and the language code of the Wikipedia you will be editing)

For out specific purpose create input files in form of:
```
{{-start-}}
'''WikiProject:WomenInRedInYourLanguage/RedListInYourLanguage/Academician''' and here the content of file (ie. list of women who is academician) should be added. You can use exampleWikitextUsingListeria.txt file as a template for this content.
{{-stop-}}
{{-start-}}
'''WikiProject:WomenInRedInYourLanguage/RedListInYourLanguage/Musician''' and here the content of file (ie. list of women who is academician) should be added. You can use exampleWikitextUsingListeria.txt file as a template for this content.
{{-stop-}}
...
...
```
Where everything between {{-start-}} and {{-stop-}} will be a single page content which should be written according to wikitext format. The first item between triple quotes ( \'\'\'  \'\'\' ) will be the name of the page. To run the script you simply run this command from terminal:
```
python pwb.py pagefromfile -showdiff -file:inputFile.txt
```

