# Autók fogyasztásának elemzése

Kezdő szintű Python-adatelemzési projekt, 5 kutatási kérdéssel, Pandas-, NumPy- és Plotly-használattal.

## Fájlok
- `Autok_fogyasztasanak_elemzese.ipynb` – teljes Jupyter Notebook
- `adatok/mpg.csv` – 234 soros valós adatállomány
- `requirements.txt` – telepítendő csomagok

## Futtatás
1. A ZIP-et csomagold ki, a mappaszerkezetet tartsd meg.
2. Nyisd meg a notebookot Jupyterben.
3. Szükség esetén futtasd egy külön cellában: `%pip install pandas numpy plotly`
4. A **Run All** paranccsal futtasd a cellákat.

## Kérdések
1. Melyik gyártó modelljei a legtakarékosabbak a mintában?
2. Hogyan függ össze a hengerűrtartalom és a fogyasztás?
3. Mennyivel magasabb a városi fogyasztás az országútinál?
4. Hogyan függ össze a hengerszám és a fogyasztás?
5. Melyik autókategória a legtakarékosabb?

## Adatforrás
A ggplot2 `mpg` adatkészlete, a plotnine Python-csomagban található CSV-ből. Dokumentáció: https://ggplot2.tidyverse.org/reference/mpg.html
A 234 megfigyelés 1999-es és 2008-as autóváltozatokat tartalmaz; nem teljes körű és nem mai autópiaci adatok.
Az amerikai MPG-t a `235.214583 / mpg` képlettel alakítjuk l/100 km-re.

## GitHub
A teljes mappát töltsd fel egy új repositoryba. A `README.md` automatikusan megjelenik a projekt főoldalán.
