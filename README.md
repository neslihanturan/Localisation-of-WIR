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

