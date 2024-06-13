

# Metamerism
[versione live](https://larobernasconi.github.io/metamerism/)

**Autore: Lara Bernasconi**  
SUPSI 2023-234  
Corso d’interaction design, CV428.01  
Docenti: A. Gysin, G. Profeta  

## Introduzione e tema

Metamerism è un archivio digitale interattivo che esplora la percezione del colore attraverso una collezione di immagini suddivise per tonalità. L'obiettivo del progetto è permettere agli utenti di scoprire la varietà cromatica e le differenze nella percezione del colore, mettendo in risalto come i colori possono variare sotto diverse condizioni di illuminazione. Attraverso questo archivio, invito gli utenti a esplorare la ricchezza visiva del colore, scoprendo le sfumature e le diverse percezioni cromatiche.

## Riferimenti progettuali

Il principale riferimento progettuale di Metamerism è il concetto di archivio digitale visivo. Mi sono ispirata a piattaforme che valorizzano la percezione del colore e la diversità visiva, come il progetto ["Color Leap"](https://www.colorleap.app/), che esplora i colori attraverso la storia. Ho voluto creare un'esperienza utente coinvolgente che permetta di scoprire la varietà dei colori in un modo visivamente attraente e interattivo.

## Design dell’interfaccia e modalità di interazione

L'interfaccia di Metamerism è progettata per essere intuitiva e coinvolgente. Al centro della schermata, le immagini sono disposte in categorie basate sulla tonalità di colore. Gli utenti possono cliccare su una categoria di colore per visualizzare le immagini associate a quella tonalità. Passando il mouse sopra un'immagine, si visualizzano ulteriori dettagli sulla percezione del colore sotto diverse condizioni di illuminazione.

### Categorie di Colore

Le categorie di colore permettono agli utenti di filtrare le immagini in base alla tonalità. Questo è gestito tramite elementi `<span>` che vengono collegati a un event listener in JavaScript per filtrare le immagini in base alla tonalità selezionata.

#### HTML per le Categorie

```html
<div class="categories">
    <span class="category" data-hue="0-30">orange</span>
    <span class="category" data-hue="30-90">yellow</span>
    <span class="category" data-hue="90-150">green</span>
    <span class="category" data-hue="150-210">cyan</span>
    <span class="category" data-hue="210-270">blue</span>
    <span class="category" data-hue="270-330">magenta</span>
    <span class="category" data-hue="330-360">red</span>
</div>
```

#### JavaScript per Gestire le Categorie

Quando l'utente clicca su una categoria di colore, l'evento `click` viene ascoltato e la funzione di callback `filterByHueRange` aggiorna la visualizzazione delle immagini in base alla tonalità selezionata.

```javascript
document.querySelectorAll('.category').forEach(category => {
    category.addEventListener('click', (e) => filterByHueRange(e.target.getAttribute('data-hue')));
});
```

### Visualizzazione delle Immagini

Le immagini sono visualizzate nella sezione principale della pagina. Quando l'utente passa il mouse sopra un'immagine, appare un tooltip che mostra i dettagli sulla percezione del colore.

#### HTML per la Sezione Principale

```html
<main></main>
<div class="tooltip" id="tooltip"></div>
```

#### CSS per il Tooltip

Il CSS definisce lo stile per il tooltip, rendendolo visibile sopra il contenuto principale quando viene attivato.

```css
/* tooltip.css */
.tooltip {
    position: absolute;
    display: none;
    background-color: white;
    border: 1px solid black;
    padding: 10px;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
```

#### JavaScript per Gestire il Tooltip

Le funzioni `showTooltip` e `hideTooltip` gestiscono la visualizzazione del tooltip. Quando l'utente passa il mouse sopra un'immagine, `showTooltip` mostra i dettagli sulla percezione del colore.

```javascript
function showTooltip(event, fileName, h, s, l) {
    const tooltip = document.getElementById('tooltip');
    tooltip.innerHTML = `hue: ${h}<br>saturation: ${s}%<br>lightness: ${l}%`;
    tooltip.style.display = 'block';
    tooltip.style.left = `${event.pageX + 10}px`;
    tooltip.style.top = `${event.pageY + 10}px`;
}

function hideTooltip() {
    const tooltip = document.getElementById('tooltip');
    tooltip.style.display = 'none';
}
```

### Integrazione delle Funzionalità

Quando i dati delle immagini vengono caricati e le immagini sono disposte sullo schermo, ogni immagine è associata agli eventi `onmouseover` e `onmouseout` che chiamano rispettivamente `showTooltip` e `hideTooltip` con i dettagli dell'immagine.

## Tecnologia usata

Per sviluppare Metamerism, ho utilizzato diverse tecnologie. La struttura HTML definisce la struttura base della pagina web, mentre CSS è utilizzato per lo stile e la presentazione visiva. Il cuore dell'interattività è gestito tramite JavaScript. L'uso di fetch API permette di caricare i dati delle immagini da file JSON esterni, rendendo l'archivio facilmente aggiornabile e manutenibile.

### Funzione filterByHueRange

La funzione `filterByHueRange` è fondamentale per filtrare le immagini in base alla tonalità di colore selezionata. Essa utilizza JavaScript per confrontare le tonalità HSL delle immagini con la tonalità selezionata dall'utente e visualizzare solo le immagini corrispondenti.

```javascript
function filterByHueRange(hueRange) {
    const [minHue, maxHue] = hueRange.split('-').map(Number);
    const filteredData = exifData.filter(item => {
        const HSL = colorDict[item.FileName][0];
        return HSL.h >= minHue && HSL.h <= maxHue;
    });

    displayFilteredData(filteredData);
}
```

### Funzione displayFilteredData

La funzione `displayFilteredData` è responsabile di visualizzare le immagini filtrate nella sezione principale della pagina. Essa aggiorna dinamicamente il contenuto HTML con le nuove immagini filtrate.

```javascript
function displayFilteredData(filteredData) {
    let output = "";
    for (let i = 0; i < filteredData.length; i++) {
        const fileName = filteredData[i].FileName;
        const HSL = colorDict[fileName][0];

        output += `
            <div class="image-info" onmouseover="showTooltip(event, '${fileName}', ${HSL.h}, ${HSL.s}, ${HSL.l})" onmouseout="hideTooltip()">
                <span class="color-box" style="background-color: hsl(${HSL.h}, ${HSL.s}%, ${HSL.l}%);"></span>
            </div>
        `;
    }
    document.querySelector('main').innerHTML = output;
}
```

## Target e contesto d’uso

Metamerism è rivolto a chiunque sia interessato alla percezione del colore, alla fotografia, al design e all'esplorazione delle diverse sfumature cromatiche. Questo include studenti, ricercatori, artisti, designer e chiunque sia curioso di scoprire nuove percezioni e interpretazioni dei colori. Il contesto d'uso principale è un ambiente educativo o culturale, dove gli utenti possono interagire con l'archivio digitale sia per scopi di apprendimento che di esplorazione personale.
