# 3-vaktin

Þriðja vaktin er hugræna vinnan við að halda utan um heimilislífið: að muna eftir því að muna. Þessi Claude-skill tekur hluta af henni af foreldrinu.

Skillin les skólapóst, stundatöflu, skóladagatal og Abler-æfingar og setur upp tvö áætluð verkefni:

- **Morgunsamantekt** (virka daga, rétt fyrir kl. 7): hvað barnið á að taka með í dag, frídagar framundan, breytingar á æfingum og það sem þarf að bregðast við úr skólapósti.
- **Kvöldáminning** (sunnudaga til fimmtudaga, um kl. 20): hvað á að pakka í töskuna fyrir morgundaginn.

Verkefnin lesa aðeins póst. Þau senda aldrei, svara, færa eða eyða neinu.

## Það sem þú þarft

- Claude með **Gmail-tengingu** (Settings → Connectors).
- Aðgang að **áætluðum verkefnum** (scheduled tasks) í Claude.
- Stundatöflu og skóladagatal barnsins sem mynd eða PDF.
- Ef barnið er í Abler: kveiktu á **tilkynningum í tölvupósti** í stillingum Abler-appsins. Annars sér Claude ekki þegar æfingar falla niður.

## Uppsetning

### Claude Code (CLI eða Code-flipinn í Claude-appinu)

```
/plugin marketplace add bjarnimaron/3-vaktin
/plugin install 3-vaktin@bjarnimaron
```

Eða úr skipanalínu:

```bash
claude plugin marketplace add bjarnimaron/3-vaktin
claude plugin install 3-vaktin@bjarnimaron
```

### Claude-appið eða claude.ai (sem skill)

1. Sæktu möppuna [`skills/3-vaktin`](skills/3-vaktin) og þjappaðu henni í `.zip` (mappan `3-vaktin` með `SKILL.md` í á að vera efst í zip-skránni).
2. Farðu í **Settings → Capabilities → Skills** og veldu **Upload skill**.
3. Hladdu upp zip-skránni og kveiktu á skillinni.

## Notkun

Skrifaðu t.d. „settu upp 3-vaktin" eða „hjálpaðu mér með þriðju vaktina". Claude spyr um börnin, skólana og hvaðan skólapósturinn kemur, les stundatöfluna og dagatalið, sýnir þér samantekt til að staðfesta og býr svo til verkefnin.

Til að uppfæra seinna (ný stundatafla eftir áramót, nýtt skóladagatal): biddu Claude um að uppfæra 3-vaktin-verkefnin.

## Uppfærslur

Í Claude Code:

```
/plugin marketplace update bjarnimaron
```
