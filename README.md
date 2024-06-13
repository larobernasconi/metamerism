

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

L'interfaccia di Metamerism è stata progetta pensando ad uno scroll pseudo infinito orizzontale. Al centro della schermata, i colori sono disposti inizialmente senza una categoria ma i bottoni con le categorie di colore ne permettono un sorting. Terminando passando il mouse sopra ogni colore, si visualizzano i dettagli di Hue, Saturation e Lightness (HSL).


## Tecnologia usata

Per sviluppare Metamerism, ho utilizzato due script, uno per permettermi di estrarre i metadati dalle immagini e il secondo per calcolarne il colore medio.

### JSON colore  

```
{
        "FileName": "1170eef4-9f45-48c9-8512-7272df7771b1",
        "FileExtension": ".jpg",
        "Colors": [
            "#ca8184"
        ]
    },
    {
        "FileName": "11EB0DF2-0A81-40AE-BD27-5E80C9942D84",
        "FileExtension": ".jpg",
        "Colors": [
            "#9b9488"
        ]
    },
    {
        "FileName": "126CF0FF-CE1A-4D5B-A03A-0DFD34333864",
        "FileExtension": ".jpg",
        "Colors": [
            "#6c6c64"
        ]
    },
    {
        "FileName": "13F28300-E23C-4333-B89A-B22173342320",
        "FileExtension": ".jpg",
        "Colors": [
            "#b2a99d"
        ]
    },


```

### Conversione da HEX a HSL

Per convertire i codici HEX a HSL ho utilizzato una funzione javascript di 
James Milner – [HEX to HSL](https://www.jameslmilner.com/posts/converting-rgb-hex-hsl-colors/)

```
 function HexToHSL(hex) {
            const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
            if (!result) {
                throw new Error("Could not parse Hex Color");
            }

            const rHex = parseInt(result[1], 16);
            const gHex = parseInt(result[2], 16);
            const bHex = parseInt(result[3], 16);

            const r = rHex / 255;
            const g = gHex / 255;
            const b = bHex / 255;

            const max = Math.max(r, g, b);
            const min = Math.min(r, g, b);

            let h = (max + min) / 2;
            let s = h;
            let l = h;

            if (max === min) {
                // Achromatic
                return { h: 0, s: 0, l };
            }

            const d = max - min;
            s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
            switch (max) {
                case r:
                    h = (g - b) / d + (g < b ? 6 : 0);
                    break;
                case g:
                    h = (b - r) / d + 2;
                    break;
                case b:
                    h = (r - g) / d + 4;
                    break;
            }
            h /= 6;

            s = s * 100;
            s = Math.round(s);
            l = l * 100;
            l = Math.round(l);
            h = Math.round(360 * h);

            return { h, s, l };
        }

```

## Target e contesto d’uso

Metamerism è rivolto a studenti, ricercatori, artisti, designer e chiunque sia curioso di scoprire la palette colori del proprio rullino fotografico. Il contesto d'uso principale è fondamentalmente una ricerca/esplorazione personale.
