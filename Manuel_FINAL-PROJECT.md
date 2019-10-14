

```python
# First of all, we import REGEX: 
import re

# We open a file containing an Old Souletin text and assign a variable to it:
IP = open('IP.txt').read()

"""One can search for both upper and lower case within the REGEX, 
but it is much more to command a case sensitive replacement of the pattern.
As a starting point, I create 
IP_1 = re.sub('[G|g]e','je',IP)
IP_2 = re.sub('[G|g]i','ji',IP_1)
IP_3 = re.sub('[G|g]ue','ge',IP_2)
IP_4 = re.sub('[G|g]ui','gi',IP_3)
IP_5 = re.sub('[Q|q]ue','ke',IP_4)
IP_6 = re.sub('[Q|q]ui','ki',IP_5)
IP_7 = re.sub('[Ç|ç]','z',IP_6)

Thus, we definitively need a case sensitive replacement function.
"""
```




    "One can search for both upper and lower case within the REGEX, \nbut it is much more to command a case sensitive replacement of the pattern.\nAs a starting point, I create \nIP_1 = re.sub('[G|g]e','je',IP)\nIP_2 = re.sub('[G|g]i','ji',IP_1)\nIP_3 = re.sub('[G|g]ue','ge',IP_2)\nIP_4 = re.sub('[G|g]ui','gi',IP_3)\nIP_5 = re.sub('[Q|q]ue','ke',IP_4)\nIP_6 = re.sub('[Q|q]ui','ki',IP_5)\nIP_7 = re.sub('[Ç|ç]','z',IP_6)\n\nThus, we definitively need a case sensitive replacement function.\n"




```python
# We create a function for case sensitive replacing:
def case_sensitive_substitution(string, old, new):
    """ This function replaces occurrences of old with new, within string
        replacements will match the case of the text it replaces"""
    def repl(match):
        current = match.group()
        result = ''
        all_upper = True
        for i,c in enumerate(current):
            if i >= len(new):
                break
            if c.isupper():
                result += new[i].upper()
            else:
                result += new[i].lower()
                all_upper = False
        # Now we append any remaining characters from new
        if all_upper:
            result += new[i+1:].upper()
        else:
            result += new[i+1:].lower()
        return result
    regex = re.compile(re.escape(old), re.I)
    return regex.sub(repl, string)

# We text the new function:
print(case_sensitive_substitution("This is a STRING whith UPPER and LOWER caseS",'s','б'))
```

    Thiб iб a БTRING whith UPPER and LOWER caбeБ



```python
# Once created the function for case sensitive replacement, we can use it for command a number of substitutions 
# that perform changes from the spelling system of Old Souletin Basque into the spelling of present-day standard Basque.
# It is crucial to bear in mind the order of the rules that we want to introduce in the code.

# The first pair of rules must be executed before the second pair; otherwise one pair invalidates the other pair:
IP_rule1 = (case_sensitive_substitution(IP,'ge','je'))
IP_rule2 = (case_sensitive_substitution(IP_rule1,'gi','ji'))
IP_rule3 = (case_sensitive_substitution(IP_rule2,'gue','ge'))
IP_rule4 = (case_sensitive_substitution(IP_rule3,'gui','gi'))

# The following 3 rules do not pause such problem:
IP_rule5 = (case_sensitive_substitution(IP_rule4,'que','ke'))
IP_rule6 = (case_sensitive_substitution(IP_rule5,'qui','ki'))
IP_rule7 = (case_sensitive_substitution(IP_rule6,'ç','z'))

# Rules 8 & 9 must be executed in this very order, otherwise 9 cancels 8.
# Moreover, rules 8 & 9 must be previous to 10:
IP_rule8 = (case_sensitive_substitution(IP_rule7,'x','ts'))
IP_rule9 = (case_sensitive_substitution(IP_rule8,'tch','tx'))
IP_rule10 = (case_sensitive_substitution(IP_rule9,'ch','x'))

# Similarly, rules 11 & 12 must be permormed before rule 13:
IP_rule11 = (case_sensitive_substitution(IP_rule10,'ce','ze'))
IP_rule12 = (case_sensitive_substitution(IP_rule11,'ci','zi'))
IP_rule13 = (case_sensitive_substitution(IP_rule12,'c','k'))

# We add more rules updating spelling:
IP_rule14 = (case_sensitive_substitution(IP_rule13,'gn','ñ'))
IP_rule15 = (case_sensitive_substitution(IP_rule14,'y','i'))
IP_rule16 = (case_sensitive_substitution(IP_rule15,'v','b'))
IP_rule17 = (case_sensitive_substitution(IP_rule16,'mb','nb'))
IP_rule18 = (case_sensitive_substitution(IP_rule17,'mp','np'))
IP_rule19 = (case_sensitive_substitution(IP_rule18,'ss','s'))
IP_rule20 = (case_sensitive_substitution(IP_rule19,'é','e'))

# Still we need to establish two rules which are crucial to reflect the phonological appearence of the Souletin dialect:
# u > ü, BUT ou > u. 
# After having explored many different and more complex ways, I propose this one, which is quite simple:

# First we establish the change from U to ü:
IP_rule21 = (case_sensitive_substitution(IP_rule20,'u','ü'))
# Then we change the previously non-existent OÜ group to U:
IP_rule22 = (case_sensitive_substitution(IP_rule21,'oü','u'))

# Now we can test how is the general spelling update for Old Souletin texts:
print(IP_rule22)
```

    ﻿IGANTEZTAKO PRONUA, ETA HILEN PRONUA.
    PAÜBEN, G. DÜGÜE ETA J. DESBARATZ, Beithan Muldezko Leteretan ezarria.
    
    < 1 > IGANTEZTAKO PRONUA.
    
    Aitaren eta Semiaren eta Espiritü Saintiaren izenian. Hala biz.
    
    Popülü fidela, zunbat ere gure menteko egün oroz Jinko gure kreazaliaren zerbütxatzera obligatü beikira, halere igantia Jaünaren egüna deithü izan da zeren eta egün saintü huntan lan thenporalak oro eitzi behar beitütügü, amurekatik gure Jaün ezinago handi denaren laidatzen, 
    eta uhuratzen enplega dezagün, eta haren leialki zerbütxatzeko moianen ikhasten.
    
    Hartakoz lekhü saintü huntara bildü izan gira, nun present gaüdialarik mezako sakrifizio saintian, zuñtan Jesü Kristek bere büria guregatik aphezen eskietzaz bera aitari oferitzen beitü ogiaren eta arduaren üdüriaren pian, lehenik Jinkua adoratzen dügü, < 2 > eta hari zor zaion ororen gañetiko uhuria emaiten: bigerrenki haren huntarzün dibinuari eskerrak deitzogü, egün orok largoki hareganik ükheiten dütügün hunkietzaz: herenki arrakiritzen dügü gure bekhatiak pharka ditzagün: laürgerrenki aldiz galthatzen deiogü, gük eta gure anaiek mengua dütügün graziak oro, eta berheziki haren aste huntan leialki zerbütxatzeko, eta haren manü saintien bidian ebilteko behar dütügünak.
    
    Bena nula gure bekhatiek Jinkuaganik hürrünt erazirik haren grazien errezebitzeko photeria idokitzen beiteiküie sar gitian gihaür beithan, hari ümilki pharkamentü esketzeko galtatzen deitzogülarik, hetarik. egiazko penitenzia batez xahatü izateko mengua dütügün sokhorriak.
    
    Lüharrastürik zure aitzinian, ô ene, Jinko huna baguatzü gure faltá orotzaz kofesione jeneral baten egitera, zuri hetzaz ezinago ümilki parkhamentü galthatzeko, arrakiritzen zütügü Jesü Krist zure semiaren odol khürütxen gañen ixuri izan denaren ororen othoizez eta mereximentiez, plazer deitzküzün pharkatü, eta zure ez haboro ofentsatzeko grazia eman.
    
    < 3 > Kofesatzen nüzü Jinko photere oro dianari Maria bethi birjina dohatsiari Jondane iñeli Arxanjeliari, Jondane Johane Bitisatri Phetiri eta Paüle Apostolü saintier, saintü orori, eta zuri, aita ezpiritüala, zeren sobera bekhatü egin beitüt phensamentüz hitzez eta obraz ene faltaz, ene faltaz, ene ezinago falta handiaz, hartakoz othoitzen düt Maria bethi Birjina dohatsia Jondane iñeli Arkanjelia Jondane Johane Batista Phetiri eta Paüle Apostülü saintiak, beste saintiak oro eta zü aita ezpiritüala, othoi dezazün enegatik gure Jinko Jaüna.
    
    Eta nula ezin egürükiten ahal beitügü gure falten pharkamentürik, gihaürk guri ogen egiler pharkatzez baizik, hitzeman dezagün goratik Jinko miserikordien Aitari < 4 > bihotzez pharkatzen dügüla gure etsaier, eta galtha diazogün gure protsimuaren kuntre hartü ahal güntükian hastiotarzünezko, eta hügünkeriazko sendimentiak gure bihotzetarik khentzen dütialarik, betha ditzagün amurioz eta karitatez, Jesü Krist gure gehienak khürütxen gañen etsenplü hañ konplitü eman deikünaren araür.
    
    Ezinago aphaltürik, eta gure bihotzak karitatiaren oropiluaz algarreki jüntatürik eskentüren dütügü Jinkuari gure botuak, eta othoitziak. Elizagatik, amurekatik Jinkuaren huntarzün handiak plazer dian han irañerazi bakia, eta jünto izatia; othoitüren dügü fede katolika, Apostolika romanuaren emendatziagatik, heresien osoki gal erazitziagatik, sxismen hil erazitziagatik, infidel eta heretikuen fediala ützülziagatik, eta jeneralki Elizaren komünionetik berhezi izan diren orogatik; amurekatik haren barniala berriz sartzen direlarik Jinkua lekhü orotan bardinzki izan dadin ezagütü zerbütxatü eta maithatü.
    
    Eskentüren dütügü gisa berian Jinkuari gure boto eta othoitze ezinago ümilak aphezküpü eta artzañ bere elizaren gobernia eitzi jeien orogatik, berheziki gure Aita saintiagatik gure Jaün aphezküpiagatik, haren kargüdün prinzipalegatik, eta jeneralki erretor eta artzañ arima kargü dienegatik, amurekatik < 5 > bere argiez argi ditzan, eta bere amuriuaz gort, bere bürien eta bere ardien salbamentüzko bidian gidatzeko.
    
    Othoitüren dügü prinze khiristi, direnen alkharreki jünto eta enthelegü hunian izatiagatik, bakiagatik, erresoma hunen destorbü gabe izatiagatik: bena prinzipalki gure errege ezinago khiristi denaren persona sakratiagatik, erregiñagatik, jaün daüfiagatik, eta erregeren familia orogatik, parropia huntako patruagatik, gure gobernia dien Gehienegatik, amurekatik hen gehienguaren pian bakiak irañ dezan, biziua den gaztigatü, berthütia unhetsi eta süstengatü, eta praübiak bere miserian diren sokhorritü.
    
    Eskentüren dütügü gisa berian Jinkuari gure othoitziak eliza huni hunki egilegatik, egün ogi benedikatia oberendatü dienegatik, gure askazi eta adixkidegatik, eri, praübe, alhargün, haürr zürtx, presoner, bidejant, emazte izorra, eta jeneralki aflijitü diren orogatik; amurekatik plazer dütian bere huntarzün handiaz soferitzen dütien gaitzetarik libratü, edo her eman grazia, eta mengua dien pazentzia, hen khiristi hun antzo igursteko.
    
    Othoitüren dügü orano jüstuegatik, Jinkuari, zuñek eman beiteie jüsto izatia, galthatzeko, hartan berian irañ erazi diazen, < 6 > eta bekhatoregatik, amurekatik ezitzan orozbat eitz estatü hartan; bena bere aitzinetiko graziez hunki ditzan eta hunialat ützül erazi.
    
    Eta nula Jinkuari lejitimoki galthatü behar dütügün gaizak oro edireiten beitira Jaünaren orazionian: entzün ezazü Jinko Jaüna Jesü-Krist zure semiak berak hitzez hitz erakutsi nahi ükhen deikün othoitzia baguatzü erraitera gure ezpiritiak elizarenareki jüntatzen dütügülarik.
    
    Gure Aita zelietan zirena, erabil bedi saintüki zure izena: jin bekigü zure erresoma: konpli bedi zure boronthatia zelian bezala, lürrian ere: emagüzü egün gure egün orozko ogia: eta pharka itzagützü gure bekhatiak hala nula gük pharkatzen beitütügü guri ogen egiler, eta, ezkitzazüla eitz tentazionialá erortera: bena libra gitzazü gaizkitik hala biz.
    
    < 7 > Eta Maria Birjina ejinago saintaren lagüngua püxanta gihaürganat erakhar dezagün amurekatik, erranen dügü Aingüriaren salütazionia, arrakiritzen dügülarik, nahi deizün eskentü, ô ene Jinkua! elizareki hel erazitzen deiogün orazionia.
    
    Salütatzen zütüt Maria graziaz bethia; Jaüna düzü zureki: benedikatü zira emazte ororen gañetik: eta benedikatü düzü zure sabeleko frütia Jesüs. Maria sainta Jinkuaren ama othoi egizü guregatik bekhatore girelakoz orai eta gure hiltzeko orenian hala biz.
    
    Eta zeren fedia beita gure salbamentiaren fundamena, eta gure othoitzek eta egitatek orok, Jinkuaren aitzinian hun izateko egiazko feden gañen bermatü behar beitie, hari hura gabe eztenen gañen posible plazerik egitia, fede hartan girela orori büriak gorarik erakutsiren dügü, zuñ edireiten beita Apostolien sinboluan, eta zuñtan bizi eta hil nahi girela fermoki segürtatzen beitügü.
    
    < 8 > Sinhesten düt Jinko Aita photere oro dianian, zeliaren eta lürraren Kreazalian, eta Jesü-Krist haren seme bakhoitz gure Jaünian zuñ konzebitü izan beita espiritü saintiaz, sorthü Maria Birjinaganik, soferitü Ponzio Pilatüsen pian, krüzifikatü, hil, eta ehortzi, jaitsi ifernietara, heren egünian hiletarik phiztü zelietarat igañ, jarririk dago Jinko Aita photere oro dianaren althe esküñian, hantik jinen-da bizien eta hilen jüjatzera sinhesten düt espiritü saintian, eliza bekhatien pharkamentia aragiaren phiztia, bethi irañen dian bizitzia hala biz.
    
    Nula khiristi baten bizitzia ezpeitago solamentz < 9 > Jinkuak bere elizari ezagüt erazi deion sinhestian; bena haren zerbütxatzian, uhuratzian, eta gaiza ororen gañetik maithatzian, berak bere legian manü emaiten deikün bezala: arrakiritzen zütügü, ô ene Jinkua, plazer deiküzün eman zure grazia, zure eta elizaren zure espusa saintaren manien leialki begiratzeko: baguatzü hen irakurtera, amurekatik begien aitzinian thaik gabe etxekiten dütügülarik, hen konplitzeko goguati izan gitian.
    
    JINKUAREN MANIAK.
    
    I. Jinko bakhoitz bat adora eta maitha ezak gaiza orotan perfeitki,
    II. Jinkuaren izena jüra eztezala ez beste gaikarik banoki,
    III. Igantiak begira itzak Jinkua zerbütxatzen dialarik debotki.
    IB. Aita eta ama uhura itzak lürrian bizi adin lüzazki,
    B. Gizon erhaile ez izala obraz ez gogoz bolontarioki,
    BI. Lütsürius ez izala gogoz ez khorpitzez baimentuareki.
    BII. Besteren hunak ebats eztitzala ez dakialarik edüki,
    BIII. Jakilegua falsürik erran eztezala ez gezürrik ihuri,
    < 10 > ITS. Aragiazko obra desira eztezala, nun ezten ezkuntziareki,
    TS. Besteren hunak inbidia eztitzala edükiteko gaxtoki.
    
    ELIZAREN MANIAK.
    
    I. Igantez Meza entzün ezak eta besta manhatiez leialki,
    II. Bekhatiak kofesa itzak mendrenetik urthian behin xüxenki,
    III. Ore kreazalia errezebi ezak aments bazko thenporan ümilki,
    IB. Besta manhatü izanen zaitzaianak igarenen dütük saintüki,
    B. Laür thenporak bijiliak eta gorexema barur itzak osoki,
    BI. Ostiralez ez neskanegünez eztük janen aragirik manentki.
    
    Eta fedezko artikülü bat denen gañen pürgatoriuan soferitzen dien arimak sokhorritü izaten ahal direla gure orazionetzaz, Elizak, karitate oso batez heki jüntatürik daguelarik manü emaiten deikü, gure othoitziak hen arhinmentxatako egin ditzagün eskentzen deitzügü, ô ene Jinknua, gure orazione ezinago ümilak arima sainta hegatik, berheziki eliza hunen fundazale eta huni hunki egilenenegatik, gure aita, ama, anaie, < 11 > arreba, askazi, eta adixkidenenegatik, eliza huntan, edo hunen ilherrian khorpitz ehortzi izan diren arimegatik, eta jeneralki pürgatoriuan soferitzen dien orogatik; amurekatik plazer dütüzün ô ene Jinkua bere phenetan solajatü, eta zure pharadüsü saintiaren gloriaz gozerazi; hori dela kaüsa jüntatüren dütüzie, ene anaie maitiak, zien orazioniak gurenekila eta erranen dügü alkhareki.
    
    < 12 > Etskümükatü direla diogü heretikuak, simonianuak, konfidenziariak, Majizienak, belhagiliak, eta aztiak oro, kostüma zaharraren araür detxemak phakatü nahi eztütienak, elizako hunak eta titüliak züzenik gabe beretzen, edo etxekiten dütienak, aphezen edo elizagizonen, bere estatiaren araür bizitzen direlarik, zaflazaliak, izentatü horik oro etskümükatürik egonen dira penitentia egin, eta bere gaizki eginaren absolüzionia ükhen artio: eta nula gaiza saintietan ezpeitie phartelant izan behar hurak merexitzen eztütienek, hartakoz manü emaiten dügü etskümükatü orori, orai berian hebentik jalki ditian, eta misterio saintietan eztitian present ediren.
    
    < 13 > PRONO HITZ GÜTIAGOZ eginen dena.
    Zuñek ezbeitü irakurtü izan behar etsortatione zunbait egin behar datekianian baizik.
    
    Popülü khiristia igantiaren egün saintü huntan bildü gira alkhargana gure ama eliza saintaren manüz, amurekatik gure Jinko Jaün gehienari, zor zaitzon zerbütxü eta uhuria eman diatzogün, haren leialki zerbütxatzeko moianak ikhas ditzagün, eta hari galtha diatzogün, hanbat gihaürentako, nula gure anaie khiristientako gure bekhatien pharkamentia, haren manü saintien begiratzeko grazia, haren süstengia aste huntan eta haren amuriuan iraitia.
    
    Hori dela kaüsa, ene anaie maitiak Jesü-Krist gure Jaünaren khorpitz eta odolaren sakrifizio ez odolstatia oferitzen dügün thenporan bihotzak eta ezpiritiak algarreki jüntatzen dütügülarik: haren mereximentietan fidatzen girelarik othoi dezagün Jinkua eliza sainta katolikaren begira eta goratziagatik, gure Aita saintiagatik, Jaün aphezküpiagatik eta jeneralki eliza gizon orogatik.
    
    Galthatüren dügü Jinkuari bakia prinze khiristi direnen arteko othoitüren dügü erresoma hunen destorbü gabe izatiagaitk gure < 14 > errege ezinago khiristi denagatik, erregiñagatik, Jaün daüfiagatik, eta erregeren familia orogatik parropia huntako patruagatik eliza huni hunki egilegatik, ogi benedikatia oberendatü dienegatik ere, eta aflijitü diren ororen konsolazioniagatik, hitz batez gure anaie katoliko diren orogatik, orotan gainti parropia hontako ekhoiliarregatik.
    
    Othoi dezagün orano elizaren fedian hil izan diren ororentako eta prinzipalki aldiz zuñen ere khorpitzak eliza huntan edo hunen ilherrian ehortzirik beitaüde hen arimentako.
    
    Jinkuari plazer haboruenik egiten deion othoitzia da Jaünaren orazionia, zuñtan beitaüde hitz aphürretan gük hari galthatü behar dütügün gaizak, eta ezinago hun da Aingüriaren salütazioniaren hari jüntatzia, amurekatik xikiago entzünak izan gitian Maria Birjina ezinago saintaren arartekoguaz, baguatzü latiz eta üskaraz hen erraitera.
    
    Prono handian bezala.
    
    < 15 >
    
    Nula bildü izan beikira orano lekhü saintü huntara gure salbamentiaren egiteko moianen ikhasteko, eta nula salbatü izateko, sinhetsi behar beitira Jinkuak bere manü eman deitzkünak konplitü, baguatzü apostolien sinboluaren erraitera zuñtan hitz aphürretan beitaüde gük sinhetsi behar dütügün gaizak, eta Jinkuaren eta elizaren maniak, zuñek erakusten beiteizkie egin behar dütügünak.
    
    Sinhestendüt, &k.
    Jinko bakhoitz bat adora, &k.
    Igantez meza entzün ezak, &k.
    Prono handian bezala.
    
    Horietarik landa aphezak erranen dütü asteko bestak, barur egünak, obitak, kridae, edo beste gaiza pronotik jakin erazitzera kostümatü direnak, eta gero akabiren dü dioialarik.
    
    Etskümükatü direla diogü heretikuak, shismatikuak, majizienak, belhagiliak eta aztiak, elizagizonen ikthuski zaflazaliak, düelian phapakatzen direnak, elizaren hunen berezaliak edo etxekizaliak haren jüstiziaren < 16 > kuntre duatzanak, hari so daüdian titülü, paper, eta borogantzen gordazaliak, bazterzaliak edo etxekiliak, legat piusen gordazaliak edo konplitü nahi bagiak, holakuak oro egonen dira maradikatürik eta etskümükatürik, penitenzia egin, eta bere gaixtokerien absolüzionia ükhen dezen artio.
    
    Aphezak gero eginen-dü bere etsortazionia, hartarik landa pheredikagütie jaiki gabe erranen dü.
    
    Baguatza deprofündisaren erraitera, hil ororen eta berheziki parropia huntakuen solajamentütan.
    
    Popülia belhaürikatüren da aphezak aldiz altharialat begithartez ützültzen delarik azkarki erranen dü.
    
    Prono handian bezala.
    
    < 17 > HILEN PRONUA.
    
    Aitaren eta Semiaren eta Espiritü Saintiaren izenian. Hala biz.
    
    Arima debotak, bildü ziradeie lekhü saintü huntara eliza ama saintaren egitate hunaren araür lehenik Jinkuaren adoratzera, eta egin deitzien huntarzün eta grazia orotzaz eskerren emaitera, zien bekhatiaz pharkamentü eskatzera eta arimen salbatzeko behar dütüzien gaizen hari ümilki galthatzera ararteko ezarten deriozielarik Birjina Ama sainta, zien aingürü begirazale, patru saintü, eta beste saintiak oro.
    
    Bigerrenian huna bildü ziradeize Pürgatoriko phenetan diren arimen ürgaiztera eta berheziki N. zenaren arimaren zuñentako egün erraiten beitira meza saintiak eliza huntan, eta ohereskiak egiten, eretxeki ditzakogün oro bere bekhatian satisfaktionetan, othoitzen dügülarik gure Jinkua plazer dian phena orotarik idokirik pharadüsüko gloria saintian ezarri; hartakoz erranen düzie Trinitate saintian uhuretan hiruretan Pater < 18 > abe Maria eta gloria, berhala Jesü-Krist gure salbazaliaren bost zaüri khariuen uhuretan bostetan Pater abe Maria eta Rekiem, eta arima hura ezpada beharretan; huntarzün hoiek oro ditzagün Jinkuari eskent haren askazi edo beste arima beharretan direnegatik.
    
    Eta zeren Pürgatoriako phenetan diren arimek sokhorritü behar beitie meza saintiez orotan gainti, gero othoitze, barur, amuina, ohereskü, eta beste obra hunez gomendatzen deritziet karitate hoietzaz ahalik hobekiena ürgaitz ditzazien, sinhesten düzielarik, eta esparantxa hartzen bere phenetarik soltatürik Jinkuaren gloriaz gozatüren direnian izanen direla ordari othoitze egile, eta Jinkuak hobeki entzün zitzen adela ziteie zien arimen haren grazian ezartera bekhatü orotzaz pharkamentü ümilki galthatzen deriozielarik eta osoki hitz emaiten etziradiela haboro bekhatiala eroriren; igarenez dolümen haür erakuts ezozie.
    
    Ene Jinko huna pharkamentü eske naüzü ümilki ene bekhatü orotzaz dolü eta bihotz min dizüt zure amurekatik zeren oro hun zirelarik ogen egin beiterizüt; hartzen dizüt boronthate osobat zure graziareki batian, ez haboro bekhatürik egiteko, berhala hitz emaiten deizüt ene bekhatü orotzaz kofesatüren nizala ahalik sarriena, emanen zaitadan penitenziaren egiteko hala biz. 
    



```python
# For evaluation purposes, we can also count the number replacements.
# To that end we can use the function len() and REGEX, in a syntax as follows:
# count_changes = len(re.findall(pattern,string))

# To obtain the number of replacements operated by the each rule, 
# we can subtract the number of occurrences of the OLD PATTERN in the text issue from the application of this rule (IP_ruleX)
# to the number of occurrences of the old pattern in previous version of the text (IP_rule(X-1):
# count_ruleX = len(re.findall(pattern,IP)) - len(re.findall(pattern,IP_ruleX))

# We test the number of matches with function print()
count_rule1 = len(re.findall('ge',IP)) - len(re.findall('ge',IP_rule1))
print('Rule 1 operated ',count_rule1,' changes.')
```

    Rule 1 operated  9  changes.



```python
# Now we do the same for each replacing rule:

count_rule2 = len(re.findall('gi',IP_rule1)) - len(re.findall('gi',IP_rule2))
count_rule3 = len(re.findall('gue',IP_rule2)) - len(re.findall('gue',IP_rule3))
count_rule4 = len(re.findall('gui',IP_rule3)) - len(re.findall('gui',IP_rule4))
count_rule5 = len(re.findall('que',IP_rule4)) - len(re.findall('que',IP_rule5))
count_rule6 = len(re.findall('qui',IP_rule5)) - len(re.findall('qui',IP_rule6))
count_rule7 = len(re.findall('ç',IP_rule6)) - len(re.findall('ç',IP_rule7))
count_rule8 = len(re.findall('x',IP_rule7)) - len(re.findall('x',IP_rule8))
count_rule9 = len(re.findall('tch',IP_rule8)) - len(re.findall('tch',IP_rule9))
count_rule10 = len(re.findall('ch',IP_rule9)) - len(re.findall('ch',IP_rule10))
count_rule11 = len(re.findall('ce',IP_rule10)) - len(re.findall('ce',IP_rule11))
count_rule12 = len(re.findall('ci',IP_rule11)) - len(re.findall('ci',IP_rule12))
count_rule13 = len(re.findall('c',IP_rule12)) - len(re.findall('c',IP_rule13))
count_rule14 = len(re.findall('gn',IP_rule13)) - len(re.findall('gn',IP_rule14))
count_rule15 = len(re.findall('y',IP_rule14)) - len(re.findall('y',IP_rule15))
count_rule16 = len(re.findall('v',IP_rule15)) - len(re.findall('v',IP_rule16))
count_rule17 = len(re.findall('mb',IP_rule16)) - len(re.findall('mb',IP_rule17))
count_rule18 = len(re.findall('mp',IP_rule17)) - len(re.findall('mp',IP_rule18))
count_rule19 = len(re.findall('ss',IP_rule18)) - len(re.findall('ss',IP_rule19))
count_rule20 = len(re.findall('é',IP_rule19)) - len(re.findall('é',IP_rule20))
count_rule21 = len(re.findall('u',IP_rule20)) - len(re.findall('u',IP_rule21))
count_rule22 = len(re.findall('oü',IP_rule21)) - len(re.findall('oü',IP_rule22))

print('Rule 1 operated ',count_rule1,' changes.')
print('Rule 2 operated ',count_rule2,' changes.')
print('Rule 3 operated ',count_rule3,' changes.')
print('Rule 4 operated ',count_rule4,' changes.')
print('Rule 5 operated ',count_rule5,' changes.')
print('Rule 6 operated ',count_rule6,' changes.')
print('Rule 7 operated ',count_rule7,' changes.')
print('Rule 8 operated ',count_rule8,' changes.')
print('Rule 9 operated ',count_rule9,' changes.')
print('Rule 10 operated ',count_rule10,' changes.')
print('Rule 11 operated ',count_rule11,' changes.')
print('Rule 12 operated ',count_rule12,' changes.')
print('Rule 13 operated ',count_rule13,' changes.')
print('Rule 14 operated ',count_rule14,' changes.')
print('Rule 15 operated ',count_rule15,' changes.')
print('Rule 16 operated ',count_rule16,' changes.')
print('Rule 17 operated ',count_rule17,' changes.')
print('Rule 18 operated ',count_rule18,' changes.')
print('Rule 19 operated ',count_rule19,' changes.')
print('Rule 20 operated ',count_rule20,' changes.')
print('Rule 21 operated ',count_rule21,' changes.')
print('Rule 22 operated ',count_rule22,' changes.')
```

    Rule 1 operated  9  changes.
    Rule 2 operated  13  changes.
    Rule 3 operated  17  changes.
    Rule 4 operated  72  changes.
    Rule 5 operated  0  changes.
    Rule 6 operated  1  changes.
    Rule 7 operated  395  changes.
    Rule 8 operated  22  changes.
    Rule 9 operated  19  changes.
    Rule 10 operated  16  changes.
    Rule 11 operated  117  changes.
    Rule 12 operated  125  changes.
    Rule 13 operated  480  changes.
    Rule 14 operated  32  changes.
    Rule 15 operated  30  changes.
    Rule 16 operated  3  changes.
    Rule 17 operated  6  changes.
    Rule 18 operated  11  changes.
    Rule 19 operated  12  changes.
    Rule 20 operated  4  changes.
    Rule 21 operated  782  changes.
    Rule 22 operated  267  changes.



```python
# REMARKS:

# Although the result is highly satisfactory, it is needed a philological revision of the output.
# For example, the [u > ü] rule may change family names that are not Basque (Dugué > Dügüé).
# More importantly, some diphtongs that are no subject to this rule have been changed too (PAUBEN > PAÜBEN).
# Another problem is that of ethymological spelling: the syllable <chan> in the word "Archangel" is pronounced /kan/, 
# so its spelling must NOT be changed into <Arxanjel> (as rule 10 establishes), but into <Arkanjel>.
# As a final remark, this conversor of spelling is not "universal" for Old Souletin Basque, since variation in spelling is important in old texts. 
# However, the main guidelines of this graphical updating are supposed to work with most of Old Souletin texts:
# we always can adapt this code by introducing new rules or change existent ones.
```
