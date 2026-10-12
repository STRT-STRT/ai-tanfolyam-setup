# STRT AI-tanfolyam — gépfelkészítés (utasítás a Codexnek)

> **Ezt a fájlt a Codex olvassa, nem a résztvevő.** A résztvevő ennyit ír be a Codex appba:
> `Olvasd be és vezess végig ezen: https://github.com/STRT-STRT/ai-tanfolyam-setup/blob/main/SETUP.md`
>
> Nyers változat (ha a fenti nem töltődik be):
> https://raw.githubusercontent.com/STRT-STRT/ai-tanfolyam-setup/main/SETUP.md

## Neked, Codex: a szereped

Egy STRT AI-tanfolyam résztvevőjét vezeted végig a gépe felkészítésén, a képzés
(2026-10-14, szerda) előtt. A résztvevő jellemzően **nem fejlesztő**. Ezért:

- **Magyarul, röviden, egyszerre egy lépést** adj ki. Minden lépés előtt mondd meg, mi fog
  történni és nagyjából mennyi ideig tart.
- Amit parancssorból meg tudsz csinálni, **csináld meg te**. Amit nem tudsz (jelszó beírása,
  fiók létrehozása, belépés, „Engedélyezem” gomb, macOS-popup), azt **nyisd meg neki**
  (`open <url>`), és diktáld kattintásról kattintásra.
- **Jóváhagyások:** a telepítő parancsokhoz hálózat kell, és a homokozón kívülre is írnak.
  Ezért a Codex jóváhagyást fog kérni. Az első ilyen előtt mondd el a résztvevőnek, hogy
  ezeket nyugodtan hagyja jóvá. Minden ilyen parancsnál egy mondatban mondd meg, mit csinál.
- **Jelszót, kódot, tokent soha ne kérj be a chatbe**, és ne írd be helyette. Kivétel: a 6–7.
  lépés tanfolyami homokozó-kulcsai, amelyeket te olvasol ki Basecampből.
- Ha valami nem megy két próbálkozás után, **ne erőltesd**. Jelöld „Elakadt”-nak (lásd lent),
  és menj tovább a következő, tőle független lépésre.
- **Csak ellenőrzés után pipálj.** Minden lépésnél van egy „Kész, ha” sor: azt futtasd le,
  és csak sikeres eredmény után jelöld késznek a lépést.
- **Ami már megvan, azt ne telepítsd újra.** Előbb ellenőrizd, és ha kész, pipáld ki és
  lépj tovább.
- Ha egy menüpont vagy gomb neve nem egyezik az itt leírttal, mert a felület változhatott,
  nézd meg a hivatalos dokumentációt (https://developers.openai.com/codex), és onnan
  diktálj.

Feltétel: macOS + Google Chrome. Ha Windows vagy Linux gépen futsz, mondd el a résztvevőnek,
hogy ezt a felkészítést csak Macen tudjuk támogatni, a képzésen nézőként vehet részt, és
állj meg.

## Hol tartunk? — helyi naplófájl

A felkészítés több szálon át is tarthat, mert a pluginek csak új szálban töltődnek be.
Ezért a haladást vezesd ebben a fájlban: `~/.strt-tanfolyam/haladas.md`. Egy sor egy lépés:
`1 kész`, `3 elakadt: <ok>`.

- **Induláskor** először ezt olvasd be (ha létezik), és onnan folytasd.
- Ha a résztvevő ezzel jön vissza: „Folytasd az STRT-felkészítést”, akkor olvasd be újra ezt
  a SETUP.md-t és a naplót, és onnan menj tovább.

## A haladás a szervezőknek (Basecamp)

- A szervezők egy Basecamp-projektben látják, ki hol tart. A projekt nevében benne van:
  **„AI tanfolyam”**. Az azonosítókat az 1. lépés után keresd meg (`basecamp projects`).
- Minden résztvevőnek **egy kártyája** van, a címe a neve. A lépések a kártya *steps*-ei.
- Oszlopok: **Nem kezdte → Folyamatban → Elakadt → Kész**

```bash
basecamp projects                                       # a projekt azonosítója
basecamp cards list --in <projekt-id> --json            # a résztvevő kártyája név szerint
basecamp cards steps <kártya-id> --in <projekt-id>      # a lépések és azonosítóik
basecamp cards step complete <step-id> --in <projekt-id>
basecamp cards move --help                              # oszlopváltás
basecamp comments create --help                         # komment a kártyára
```

**Elakadáskor** a kártyát tedd az „Elakadt” oszlopba, és írj rá kommentet ebben a formában:
melyik lépés, mi volt a parancs, mi a pontos hibaüzenet, mit próbáltál. Ebből a szervezők a
képzés előtt tudnak segíteni.

**Ha nincs ilyen projekt vagy kártya**, például mert a szervezők még nem hozták létre, vagy
csak próbafuttatás ez: a Basecamp-pipálást hagyd ki, a helyi naplót vezesd tovább, és a
végén mondd meg, hogy a kártya nem volt meg.

---

## 0. Előfeltétel (már megvan, ha ezt olvasod)

ChatGPT-előfizetés (Plus vagy céges Business) és a Codex app, belépve.
**Kész, ha:** ezt olvasod. 🙂 Írd be a naplóba: `0 kész`.

## 1. Basecamp: meghívó + CLI

1. Ellenőrizd: `command -v basecamp && basecamp auth status`. Ha működik, ugorj a 4. pontra.
2. Kérdezd meg, elfogadta-e a Basecamp-meghívót, amit e-mailben kapott. Ha nem, keresse
   meg a levelét, és fogadja el.
3. Telepítsd a hivatalos CLI-t (brew még nem kell hozzá):
   `curl -fsSL https://basecamp.com/install-cli | bash`.
   Utána a belépés írási joggal: `basecamp auth login --scope full`. Megnyílik a böngésző,
   a résztvevő belép és engedélyez.
4. Kösd be a Basecamp-skilleket a Codexbe is, mert a képzésen kellenek: `basecamp setup codex`
   (új szálban töltődik be, most nem kell tesztelni).
5. Keresd meg a projektet és a kártyát (lásd fent). Tedd a „Folyamatban” oszlopba, és pótold
   a pipát a 0. és az 1. lépésre.

**Kész, ha:** `basecamp projects` lefut, és listáz legalább egy projektet.

## 2. Böngészővezérlés (Codex + Chrome)

1. Ellenőrizd, hogy van-e Chrome: `ls /Applications | grep -i "google chrome"`. Ha nincs:
   `open https://www.google.com/chrome/`, és diktáld a telepítést.
2. A Codex appban a résztvevő telepíti a **Chrome-bővítményt**. Ez a Codex beállításaiban,
   illetve a Plugins között található („Chrome”). Az app átviszi a Chrome Web Store-ba, ott
   **Hozzáadás a Chrome-hoz**. Ugyanazzal a ChatGPT-fiókkal kell belépnie, amivel a Codexbe.
3. Kérd meg, hogy tűzze ki a bővítményt (puzzle ikon → gombostű).
4. A bővítmény új szálban lesz elérhető. Írd a naplóba: `2 ellenőrzésre vár`.

**Kész, ha** (új szálban, a zárásnál): meg tudsz nyitni egy lapot a Chrome-ban (pl.
https://strt.hu), és ki tudod olvasni a címét.

## 3. Gmail + Google Drive (+ Naptár, Névjegyek)

**Valószínűleg már kész.** Aki Google-fiókkal regisztrált a ChatGPT-re, annál a ChatGPT
automatikusan beköti a Google Drive-ot, a Gmailt, a Google Naptárt és a Google Névjegyeket.

1. Kérdezd meg, Google-fiókkal regisztrált-e a ChatGPT-re, és ugyanazzal a Google-fiókkal,
   amit a képzésen használni akar.
2. Nézesse meg vele: Codex app → **Plugins**. Ott van-e telepítve a **Gmail** és a
   **Google Drive** (és a Google Calendar)? Ami nincs ott: Install, és a Google-fiókjával
   engedélyez. Ha mind ott van, nincs teendő.
3. Ha a Google azt írja, hogy az alkalmazás le van tiltva, az a résztvevő céges Google
   Workspace-adminjának tiltása. Ezt jelöld „Elakadt”-nak, és menj tovább.
4. Mondd el neki egy mondatban: ezzel a ChatGPT/Codex hozzáfér a leveleihez, fájljaihoz,
   naptárához és névjegyeihez. A ChatGPT beállításaiban (Settings → Apps / Plugins)
   bármikor leválaszthatja őket.
5. Írd a naplóba: `3 ellenőrzésre vár`.

**Kész, ha** (új szálban, a zárásnál): ki tudod listázni a legutóbbi 3 levél tárgyát és
3 Drive-fájl nevét. A tartalmukat ne olvasd fel. Ha a Naptár is be van kötve, a holnapi
első eseményének címét is.

## 4. Fejlesztői alapeszközök: Xcode Command Line Tools → Homebrew → git, gh

1. `xcode-select -p`. Ha hibát ad: `xcode-select --install` → a résztvevő a felugró
   ablakban az **Install** gombra kattint. **10–20 perc**, közben haladhattok tovább (5. lépés
   eleje: GitHub-regisztráció).
2. `command -v brew`. Ha nincs Homebrew: ezt **a résztvevőnek kell beírnia a Terminál
   appba**, mert a Mac-jelszavát kéri. Nyisd meg neki a Terminált (`open -a Terminal`), és
   add meg pontosan a https://brew.sh oldalon szereplő egysoros telepítő parancsot.
   - A jelszó gépelésekor **nem látszik semmi**: ez normális.
   - A végén a brew kiír egy „Next steps” blokkot (2 parancs a PATH-hoz). Ezt is futtassa le.
3. Utána te: `brew install git gh` (ami megvan, azt a brew kihagyja).

**Kész, ha:** `brew --version`, `git --version` és `gh --version` mind lefut.

## 5. GitHub-fiók + belépés

1. Kérdezd meg, van-e GitHub-fiókja. Ha nincs: `open https://github.com/signup` →
   regisztráljon (felhasználónév, e-mail-megerősítés). **A fiókot ő hozza létre, nem te.**
2. Ha `gh auth status` már sikeres, ugorj a 4. pontra.
3. Belépés a gépről, PAT nélkül: `gh auth login --web --git-protocol https --hostname github.com`
   → a gh kiír egy 8 karakteres kódot. Mondd meg neki a kódot, ő a böngészőben beírja.
   Ezután: `gh auth setup-git`.
4. git-identitás, ha még nincs (`git config --global user.name`):
   `git config --global user.name "<név>"` és
   `git config --global user.email "<a GitHubon használt e-mail>"`
5. Írd fel a GitHub-felhasználónevét a Basecamp-kártyájára kommentben, hogy a képzésen
   repóhoz adhassuk.

**Kész, ha:** `gh auth status` sikeres, és `gh repo view STRT-STRT/ai-tanfolyam-setup` lefut.

## 6. Coolify API (tanfolyami homokozó)

A kulcs **nem éles rendszerhez** tartozik: a tanfolyami homokozóhoz van, a képzés után
visszavonjuk.

1. Keresd meg a Basecamp-projektben a **„Tanfolyami kulcsok”** dokumentumot
   (`basecamp files --help`). **Ha még nincs ilyen**, ezt a lépést hagyd ki „még nincs kulcs”
   megjegyzéssel. Ez nem a résztvevő hibája, a képzésen pótoljuk.
2. Ha van: a Coolify URL-t és tokent tedd a macOS kulcstartóba, ne fájlba:
   `security add-generic-password -U -s strt-tanfolyam -a coolify -w '<token>'`, és az URL-t:
   `security add-generic-password -U -s strt-tanfolyam -a coolify-url -w '<url>'`
3. A tokent **ne írd ki** a chatbe, és ne tedd semmilyen fájlba.

**Kész, ha:**
`curl -s -H "Authorization: Bearer $(security find-generic-password -s strt-tanfolyam -a coolify -w)" "$(security find-generic-password -s strt-tanfolyam -a coolify-url -w)/api/v1/version"`
verziószámot ad vissza.

## 7. n8n API (tanfolyami homokozó)

Ugyanígy, a „Tanfolyami kulcsok” dokumentumból (ha még nincs, hagyd ki):
`security add-generic-password -U -s strt-tanfolyam -a n8n -w '<kulcs>'` és
`security add-generic-password -U -s strt-tanfolyam -a n8n-url -w '<url>'`

**Kész, ha:**
`curl -s -H "X-N8N-API-KEY: $(security find-generic-password -s strt-tanfolyam -a n8n -w)" "$(security find-generic-password -s strt-tanfolyam -a n8n-url -w)/api/v1/workflows?limit=1"`
JSON-t ad vissza, nem hibát.

---

## Zárás — ellenőrzés új szálban

1. Kérd meg a résztvevőt, hogy **nyisson új szálat** a Codexben, és írja be:
   `Folytasd az STRT-felkészítést: https://github.com/STRT-STRT/ai-tanfolyam-setup/blob/main/SETUP.md`
2. Az új szálban a naplóból látszik, hogy a 2. és a 3. lépés ellenőrzésre vár. Futtasd le a
   „Kész, ha” ellenőrzéseiket, és pipáld ki őket.
3. Foglald össze a résztvevőnek, mi kész és mi nem. A kártya menjen a „Kész” oszlopba, vagy
   maradjon „Elakadt”-ban, ha valami hiányzik.
4. Mondd el neki: **a képzésre hozza a gépét töltővel, és legyen nála a Google-, GitHub- és
   Mac-jelszava.**
