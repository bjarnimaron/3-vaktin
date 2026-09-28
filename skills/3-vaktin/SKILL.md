---
name: "3-vaktin"
description: "Tekur við hluta af þriðju vaktinni: setur upp daglega morgunsamantekt og kvöldáminningu fyrir foreldra úr skólapósti, stundatöflu, skóladagatali og Abler-æfingum."
---

# 3-vaktin

Þriðja vaktin er hugræna vinnan við að halda utan um heimilislífið: að muna eftir því að muna. Hún felur meðal annars í sér að fylgjast með skilaboðum frá skólum, muna hvaða dag á að pakka sundfötum og vita hvenær skipulagsdagar eru. Þessi skill tekur hluta af því álagi af foreldrinu.

Skillin setur upp tvö áætluð verkefni (scheduled tasks):

1. **Morgunsamantekt** (virka daga, rétt fyrir kl. 7): hvað barnið á að taka með í dag, frídagar framundan, breytingar á æfingum dagsins og það sem þarf að bregðast við úr skólapósti.
2. **Kvöldáminning** (sunnudaga til fimmtudaga, um kl. 20): hvað á að pakka í töskuna fyrir morgundaginn, eða að enginn skóli sé á morgun.

Talaðu við notandann á íslensku, nema hann skrifi á öðru máli. Vertu stuttorður og hlýlegur.

## Skref 1: Athuga tengingar

- Athugaðu hvort Gmail-tól séu tiltæk (t.d. með ToolSearch að "gmail search"). Ef ekki, biddu notandann að tengja Gmail undir **Stillingar → Tengingar** (Settings → Connectors) og kveikja á því fyrir samtalið. Ekki halda áfram fyrr en Gmail virkar.
- Hladdu inn tólum fyrir áætluð verkefni (create_trigger, update_trigger, list_triggers) með ToolSearch.
- Athugaðu með list_triggers hvort notandinn sé þegar með svipuð verkefni, svo þú búir ekki til tvítekningar.

## Skref 2: Safna upplýsingum

Biddu um eftirfarandi. Það má spyrja um allt í einu skilaboði.

1. **Upplýsingar um barn eða börn.** Fyrir hvert barn: nafn, aldur, skóli eða leikskóli, bekkur eða deild (eins og það birtist í pósti, t.d. "3.HJÓ"), og nöfn umsjónarkennara ef þau eru þekkt. Einnig hvaða íþróttir eða tómstundir barnið stundar og hjá hvaða félagi.
2. **Afrit af stundatöflu** hvers skólabarns (mynd eða PDF).
3. **Afrit af skóladagatali** skólans og leikskólans fyrir skólaárið (mynd eða PDF, oftast á vefsíðu skólans).
4. **Hvaðan skólapóstur kemur.** T.d. Mentor/InfoMentor (noreply@mentor.is), Karellen, eða netfang skólans. Ef notandinn veit það ekki, leitaðu í Gmail að nýlegum pósti sem nefnir skólann og finndu sendandann sjálfur.

Minntu notandann alltaf á þetta:

> **Abler:** Til að ég sjái þegar æfingar falla niður eða breytast þarftu að kveikja á tilkynningum í tölvupósti í stillingum Abler-appsins. Annars berast þær bara sem tilkynningar í appinu og ég sé þær ekki.

Ef notandinn notar annað kerfi fyrir æfingar skaltu biðja um sendanda þeirra pósta.

## Skref 3: Lesa stundatöflu og dagatal

**Stundatafla.** Fyrir hvern virkan dag skaltu finna tíma sem krefjast þess að eitthvað sé tekið með: íþróttir (íþróttaföt), sund (sundföt og handklæði), útikennsla (útiföt, regnföt), heimilisfræði, o.s.frv. Skráðu líka hvenær skóla lýkur ef það er óvenjulegt (t.d. styttri föstudagar).

**Skóladagatal.** Skiptu dögum í tvo flokka:
- **Enginn skóli:** skipulagsdagar, vetrarfrí, jólafrí, páskafrí, rauðir dagar og aðrir frídagar.
- **Sérstakir dagar:** viðtalsdagar, litlu jól, öskudagur, vordagar, skólaslit, þemadagar og svipað.

Taktu aðeins með daga frá deginum í dag til loka skólaársins. Slepptu helgum.

Athugaðu vikudaga með kóða (t.d. Python `datetime`) áður en þú skrifar "mán.", "þri." o.s.frv. Ef dagatal sýnir talningu skóladaga á mánuði skaltu nota hana til að staðfesta óskýra daga. Segðu notandanum hvaða dagar voru óskýrir.

**Staðfestu með notandanum.** Sýndu stuttan lista yfir: hvað þarf að taka með hvern dag, frídaga og sérstaka daga. Biddu hann að leiðrétta ef eitthvað er rangt. Ekki búa til verkefnin fyrr en hann hefur samþykkt.

## Skref 4: Búa til verkefnin

Notaðu create_trigger með `initiation: "human_request"` og `notifications: {"push": true}`. Notaðu tímabelti notandans (sjálfgefið `Atlantic/Reykjavik`) í `CRON_TZ=`.

- **Morgunsamantekt:** virka daga, nokkrum mínútum fyrir kl. 7, t.d. `CRON_TZ=Atlantic/Reykjavik 48 6 * * 1-5`.
- **Kvöldáminning:** sunnudaga til fimmtudaga, nokkrum mínútum fyrir kl. 20, t.d. `CRON_TZ=Atlantic/Reykjavik 46 19 * * 0-4`.

Hver keyrsla byrjar í nýju samtali án minnis, svo hver prompt verður að innihalda allt: börnin, stundatöfluna, dagatalið, sendendur og reglur. Notaðu sniðmátin hér að neðan og fylltu inn í [HORNKLOFA].

### Sniðmát: Morgunsamantekt

```
Farðu yfir Gmail-pósthólfið mitt með Gmail-tólunum og taktu saman póst frá [SKÓLAR OG LEIKSKÓLAR] og [ÆFINGAKERFI, t.d. Abler]. Byrjaðu á áminningu um hvað börnin þurfa að taka með í dag.

BÖRNIN:
[Fyrir hvert barn: nafn, aldur, skóli/leikskóli, bekkur/deild eins og hún birtist í pósti, kennarar, íþróttir og félag.]

STUNDATAFLA [NAFN BARNS]:
- Mánudagur: [t.d. Íþróttir kl. 8:20-9:00. ÍÞRÓTTAFÖT.]
- Þriðjudagur: [...]
- Miðvikudagur: [...]
- Fimmtudagur: [...]
- Föstudagur: [...]

SKÓLADAGATAL [SKÓLI] [SKÓLAÁR]:
Enginn skóli:
- [dagur. dags.: lýsing]
Sérstakir dagar:
- [dagur. dags.: lýsing]

SENDENDUR:
- Skólapóstur: [t.d. noreply@mentor.is]. Taktu aðeins með póst sendan á bekk/árgang barnsins eða allan skólann.
- Æfingar: [t.d. noreply@abler.io]

1) "Taka með í dag" (alltaf efst): Finndu dagsetningu og vikudag í dag. Ef dagurinn er frídagur samkvæmt dagatalinu skaltu segja það í fyrstu línu. Annars skaltu segja hvað á að taka með samkvæmt stundatöflunni. Nefndu sérstaka daga. Ef póstur breytir þessu skaltu treysta póstinum.

2) "Framundan í skólanum": Nefndu frídaga og sérstaka daga á næstu 7 dögum, með dagsetningu og vikudegi. Slepptu liðnum ef ekkert er framundan.

3) "Æfingar í dag": Æfingar sem falla niður, breyttur tími eða staður, og nýir viðburðir í dag. Nefndu barn, félag, flokk, tíma og ástæðu. Ef engar breytingar eru, segðu það í einni línu.

4) Annað úr pósti síðasta sólarhrings (á mánudögum frá föstudagsmorgni), raðað eftir dagsetningu: AÐEINS heimanám, hlutir sem á að taka með, sérstakir dagar, frídagar, breyttir skólatímar, fundir og frestir. Slepptu matseðlum, almennum fréttum, auglýsingum og öllu sem á við aðra bekki eða deildir.

Leitarfyrirspurn til að byrja með: newer_than:4d ([from:SENDANDI OR from:SENDANDI OR skólanafn ...]). Síaðu eftir dagsetningu. Lestu hvern viðeigandi þráð að fullu með get_thread (PLAIN_TEXT).

Skrifaðu á íslensku, stutt og skýrt. Merktu hvert atriði með nafni barnsins. Settu hlekki á upprunapósta neðst undir "Heimildir".

Aðeins lesa. Ekki senda, svara, merkja, færa eða eyða neinum pósti.
```

### Sniðmát: Kvöldáminning

```
Skrifaðu stutta kvöldáminningu á íslensku um hvað börnin þurfa að taka með Á MORGUN, svo hægt sé að pakka í kvöld.

BÖRNIN: [sama og að ofan]
STUNDATAFLA: [sama og að ofan]
SKÓLADAGATAL: [sama og að ofan]
SENDENDUR: [sama og að ofan]

1) Finndu dagsetningu og vikudag á morgun. Ef á morgun er frídagur skaltu segja það í fyrstu línu og hvenær skóli hefst aftur. Annars skaltu segja hvað á að pakka samkvæmt stundatöflunni. Ef frídagur er á næstu 3 dögum eftir morgundaginn skaltu bæta við einni línu um það.

2) Athugaðu Gmail (newer_than:4d, sömu sendendur) hvort póstur breyti morgundeginum: frídagur, breyttur tími, tími sem fellur niður, vettvangsferð, eitthvað sem á að taka með, eða æfing á morgun sem fellur niður eða breytist. Ef póstur stangast á við dagatalið skaltu treysta póstinum.

Snið: Í mesta lagi 3-4 stuttar línur. Fyrsta línan segir hvað á að pakka, t.d. "Á morgun (þriðjudag): Sundföt og handklæði í töskuna." Settu hlekk á póst ef upplýsingar koma úr pósti.

Aðeins lesa. Ekki senda, svara, merkja, færa eða eyða neinum pósti.
```

## Skref 5: Ljúka

Segðu notandanum í fáum setningum:
- Hvenær verkefnin keyra og hvenær fyrsta keyrsla er.
- Að hann fái tilkynningu í símann.
- Ef `derived_state.permission_mode` er ekki `auto` í svarinu: að hann þurfi að kveikja á **"Automatically approve"** í stillingum hvors verkefnis, annars stoppa keyrslurnar og bíða eftir samþykki áður en þær lesa Gmail.
- Minntu aftur á tölvupósttilkynningar í Abler-appinu ef hann hefur ekki staðfest það.
- Að hann geti beðið um breytingar hvenær sem er, t.d. nýja stundatöflu eftir áramót eða nýtt dagatal næsta skólaár. Notaðu þá update_trigger með heilum nýjum prompt, ekki búa til nýtt verkefni.

## Reglur

- Verkefnin mega aðeins lesa póst. Aldrei senda, svara, færa, merkja eða eyða.
- Ekki setja netföng, nöfn eða upplýsingar annarra fjölskyldna inn í verkefni notandans.
- Ef dagatal eða stundatafla er óskýr skaltu spyrja frekar en að giska.