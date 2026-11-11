# Autók fogyasztásának elemzése

**Python-adatelemzési projekt – NumPy, Pandas és Plotly**

## 1. A projekt célja

A projekt célja egy autókat tartalmazó adathalmaz feltáró elemzése, különös tekintettel az üzemanyag-fogyasztásra. Az elemzés öt kutatási kérdésre keres választ:

1. Melyik gyártó modelljei fogyasztanak átlagosan a legkevesebbet?
2. Hogyan függ össze a motor hengerűrtartalma és a fogyasztás?
3. Mennyivel magasabb a városi fogyasztás az országútinál?
4. Hogyan változik a fogyasztás a hengerek számával?
5. Melyik autókategória a legtakarékosabb?

Az elemzés egy Jupyter Notebookban található, az eredményeket táblázatok és interaktív Plotly-diagramok szemléltetik.

## 2. A projekt felépítése

```text
Autok_adatelemzes_beadando/
├── Autok_fogyasztasanak_elemzese.ipynb
├── README.md
├── requirements.txt
└── adatok/
    └── mpg.csv
```

| Fájl | Leírás |
|---|---|
| `Autok_fogyasztasanak_elemzese.ipynb` | Az adatok betöltését, előkészítését, elemzését és ábrázolását tartalmazó notebook. |
| `adatok/mpg.csv` | A vizsgálathoz használt CSV-adatállomány. |
| `requirements.txt` | A futtatáshoz szükséges Python-csomagok listája. |
| `README.md` | Rövid projektismertető és futtatási útmutató. |

## 3. Felhasznált technológiák

- **Python:** az elemzés programozási nyelve.
- **Pandas:** CSV-adatok beolvasása, kezelése, csoportosítása és összesítése.
- **NumPy:** numerikus számítások, átlagok és korreláció meghatározása.
- **Plotly Express:** interaktív oszlop- és pontdiagramok készítése.
- **Jupyter Notebook:** a kód, a magyarázatok és az eredmények együttes bemutatása.

A szükséges csomagok a `requirements.txt` fájlban találhatók.

## 4. Telepítés és futtatás

### Előfeltételek

- Python 3 telepítése
- Jupyter Notebook vagy JupyterLab
- A projekt ZIP-fájljának kicsomagolása

### Csomagok telepítése

Terminálban, a projekt mappájában futtasd:

```bash
pip install -r requirements.txt
```

A csomagok külön is telepíthetők:

```bash
pip install pandas numpy plotly
```

### A notebook indítása

1. Csomagold ki a projektet úgy, hogy a mappaszerkezet ne változzon.
2. Nyiss terminált a projekt főkönyvtárában.
3. Indítsd el a Jupyter Notebookot:

   ```bash
   jupyter notebook
   ```

4. Nyisd meg az `Autok_fogyasztasanak_elemzese.ipynb` fájlt.
5. Válaszd a **Run All** lehetőséget, vagy futtasd egymás után a cellákat.

**Fontos:** a notebook a CSV-fájlt az `adatok/mpg.csv` relatív útvonalon keresi, ezért a futtatást a projekt főkönyvtárából indítsd, és őrizd meg az eredeti mappaszerkezetet.

## 5. Az adathalmaz bemutatása

Az adatállomány a ggplot2 `mpg` adatkészletén alapul. Összesen **234 autómegfigyelést** tartalmaz, 1999-es és 2008-as modellévekből. Az adatok nem a teljes autópiacot, és nem a jelenlegi autókínálatot reprezentálják.

### A felhasznált oszlopok

| Oszlop | Jelentés | Mértékegység / érték |
|---|---|---|
| `manufacturer` | Gyártó | szöveg |
| `model` | Modell neve | szöveg |
| `displ` | Motor hengerűrtartalma | liter |
| `cyl` | Hengerek száma | darab |
| `cty` | Városi üzemanyag-hatékonyság | amerikai mérföld/gallon (MPG) |
| `hwy` | Országúti üzemanyag-hatékonyság | amerikai mérföld/gallon (MPG) |
| `class` | Autókategória | kategória |

Az eredeti adatokban az MPG nagyobb értéke jelent kedvezőbb fogyasztást. Az összehasonlíthatóság érdekében a notebook az értékeket liter/100 kilométerre (l/100 km) alakítja át. Ebben a mértékegységben a **kisebb érték a kedvezőbb**.

## 6. Adatelőkészítés és számítások

A CSV-fájl beolvasása Pandas segítségével történik:

```python
df = pd.read_csv("adatok/mpg.csv")
```

A notebook ellenőrzi az adatok méretét, a hiányzó értékeket, és megjeleníti az első néhány sort.

### MPG átváltása l/100 km-re

Az amerikai MPG-értékek átváltásához használt képlet:

\[
\text{l/100 km} = \frac{235.214583}{\text{MPG}}
\]

A notebook három új oszlopot hoz létre:

```python
df["varosi_l_100km"] = 235.214583 / df["cty"]
df["orszaguti_l_100km"] = 235.214583 / df["hwy"]
df["kulonbseg_l_100km"] = (
    df["varosi_l_100km"] - df["orszaguti_l_100km"]
)
```

- `varosi_l_100km`: városi fogyasztás l/100 km-ben.
- `orszaguti_l_100km`: országúti fogyasztás l/100 km-ben.
- `kulonbseg_l_100km`: a városi és országúti fogyasztás különbsége.

## 7. Az elemzés menete

### 7.1. Gyártók összehasonlítása

A program gyártónként csoportosítja az adatokat, majd kiszámítja az országúti fogyasztás átlagát. Az eredményeket növekvő sorrendbe rendezi, és oszlopdiagramon jeleníti meg.

A vizsgált mintában a legalacsonyabb átlagos országúti fogyasztású gyártó: **Honda** (kb. 7,27 l/100 km).

### 7.2. Hengerűrtartalom és városi fogyasztás kapcsolata

A motor hengerűrtartalma (`displ`) és a városi fogyasztás (`varosi_l_100km`) kapcsolatát pontdiagram mutatja be. A pontok színe az autókategóriát jelöli.

A lineáris kapcsolat erősségét a Pearson-féle korrelációs együtthatóval számítja ki a program:

```python
korrelacio = np.corrcoef(
    df["displ"],
    df["varosi_l_100km"]
)[0, 1]
```

A vizsgált adatokban a korreláció körülbelül **0,804**, ami erős pozitív lineáris kapcsolatra utal: a nagyobb hengerűrtartalom általában magasabb városi fogyasztással jár együtt. A korreláció önmagában nem bizonyít ok-okozati kapcsolatot.

### 7.3. Városi és országúti fogyasztás összehasonlítása

A program kiszámítja a városi és országúti fogyasztás átlagát, valamint az autómegfigyelésenkénti különbség átlagát.

A mintában kapott értékek:

| Mutató | Átlag |
|---|---:|
| Városi fogyasztás | kb. 14,84 l/100 km |
| Országúti fogyasztás | kb. 10,73 l/100 km |
| A két érték átlagos különbsége | kb. 4,11 l/100 km |

Az eredményeket oszlopdiagram szemlélteti. A vizsgált mintában a városi átlagfogyasztás magasabb az országútinál.

### 7.4. Hengerek száma és városi fogyasztás

A program hengerek száma szerint csoportosítja az autókat, majd kiszámítja a városi fogyasztás átlagát.

| Hengerek száma | Átlagos városi fogyasztás |
|---:|---:|
| 4 | kb. 11,46 l/100 km |
| 5 | kb. 11,48 l/100 km |
| 6 | kb. 14,69 l/100 km |
| 8 | kb. 19,11 l/100 km |

A mintában a nyolc hengeres autók csoportjának a legmagasabb az átlagos városi fogyasztása.

### 7.5. Autókategóriák összehasonlítása

Az autókat kategóriánként csoportosítja a program, majd kiszámítja az országúti fogyasztás átlagát. Az eredményeket növekvő sorrendben jeleníti meg.

A vizsgált mintában a **compact** kategória a legtakarékosabb az országúti átlagfogyasztás alapján (kb. 8,43 l/100 km).

## 8. Eredmények összefoglalása

A 234 megfigyelésből álló mintán végzett elemzés főbb eredményei:

1. A legalacsonyabb átlagos országúti fogyasztású gyártó a Honda.
2. A motor hengerűrtartalma és a városi fogyasztás között erős pozitív korreláció figyelhető meg (r ≈ 0,804).
3. A városi átlagfogyasztás körülbelül 4,11 l/100 km-rel magasabb az országútinál.
4. A nyolc hengeres autók csoportjának a legmagasabb az átlagos városi fogyasztása.
5. Az országúti fogyasztás alapján a compact kategória a legtakarékosabb.

Az értékek a mellékelt CSV-adatállományból számított, kerekített eredmények.

## 9. Korlátok és értelmezés

- Az adathalmaz mindössze 234 megfigyelést tartalmaz.
- Az adatok 1999-es és 2008-as autóváltozatokra vonatkoznak, ezért nem tekinthetők a mai autópiac reprezentatív leírásának.
- Az MPG-értékek amerikai mérési rendszerből származnak; az átváltás nem WLTP-mérés.
- A gyártók és kategóriák összehasonlítása kizárólag az adathalmazban szereplő modellekre vonatkozik.
- Az átlagok elfedhetik az egyes modellek közötti különbségeket.
- A korreláció együttjárást mutat, nem bizonyít ok-okozati összefüggést.

## 10. Adatforrások

- **ggplot2 – mpg adatkészlet:** https://ggplot2.tidyverse.org/reference/mpg.html
- **plotnine GitHub-tárhely:** https://github.com/has2k1/plotnine

## 11. További fejlesztési lehetőségek

A projekt továbbfejleszthető például az alábbiakkal:

- modellévek szerinti összehasonlítás;
- váltótípus és hajtás szerinti elemzés;
- medián, szórás és kvartilisek vizsgálata;
- szélsőértékek és eloszlások elemzése;
- további interaktív diagramok és szűrők készítése.
