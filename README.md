
# Table of Contents

1.  [Sobre aquest repositori](#org6960f42)
2.  [Format i eines emprades](#org392d0ac)
    1.  [Gestor bibliogràfic: Zotero](#org50e0c4e)
    2.  [Emacs](#orgade2f96)
        1.  [Paquets adicionals](#orgc897e7d)
3.  [Organització de la informació](#org46d9c6a)
4.  [Publicació de una web](#orgdcb16ac)
5.  [Notes finals](#org4fdf3b7)
6.  [Referències](#org5e4c171)



<a id="org6960f42"></a>

# Sobre aquest repositori

He creat aquest repositori per tindre centralitzada tota la informació relativa al Màster que cursaré el curs 2023-2024 i per poder realitzar un seguiment del meu treball i els coneixements que vaig adquirint.


<a id="org392d0ac"></a>

# Format i eines emprades

Empraré [git](https://git-scm.com/), [Zotero](https://www.zotero.org) i [Emacs](https://www.gnu.org/software/emacs/), així comm els paquets [org-mode](https://orgmode.org/), [org-roam](https://www.orgroam.com/) i [org-noter](https://github.com/org-noter/org-noter) d'aquest, per construir el meu sistema de gestió del coneixement (Nagel, Jennifer, 2014) personal ([PKMS)](https://www.reddit.com/r/PKMS/), i emprar-lo profusament durant la realització del màster.

La idea és emprar programes lliures, gratuïts i tindre'ls instal·lats localment (bé siga al portatil o a la meua xarxa local, evitant els serveis *en el nuvol*). Evidentment, estic referint-me al meu entorn personal, per al treball grupal hauré d'adaptar-me al que acorde el grup, així com entregar les tarees en el format indicat pels professors del Màster.

Altres alternatives que he mirat són fer ús de markdown i programes com [obsidian](https://obsidian.md/), [logseq](https://logseq.com/), &#x2026; però la versatilitat i absoluta personalització del combo Emacs/org-mode/org-roam/&#x2026; m'ha fet decantar-me per esta última opció.


<a id="org50e0c4e"></a>

## TODO Gestor bibliogràfic: Zotero

Com a gestor bibliogràfic empre [Zotero](https://www.zotero.org),

![img](/home/cavicas/Pictures/ScreenShots/Screenshot_20230924_091828.png)

conjuntament amb:

-   L'[extensió per al navegador web](https://www.zotero.org/download/) ([Firefox](https://www.mozilla.org/en-US/firefox/) en el meu cas). Facilita molt baixar els documents des d'Internet, o si el que estem llegint és una pàgina web, realitzar una captura local d'aquesta.

-   El plugin [Better Bibtex,](https://retorque.re/zotero-better-bibtex/) que exporta la bibliografia de Zotero a format bibtex, per emprar-la en la creació de documents en els formats org-mode o $\LaTeX$, per exemple.

Ara bé, la lectura de la documentació, així com les notes de la mateixa, intentaré realitzar-les des del propi Emacs, emprant org-noter i pdf-tools.


<a id="orgade2f96"></a>

## Emacs

La raó per emprar Emacs és que vull emprar el mode org, i org-roam, per construir el meu sistema de gestió del coneixement.

Per a les cites i referències dins d'Emacs empraré el paquet [citar](https://github.com/emacs-citar/citar). En la següent captura estic inserint una referència:

![img](/home/cavicas/Pictures/ScreenShots/Screenshot_20230923_083923.png)

Una vegada inserida la referència, puc obrir el document associat a la mateixa (que sempre intente tindre com adjunt en Zotero): 

![img](/home/cavicas/Pictures/ScreenShots/Screenshot_20230923_084003.png)

Per llegir i anotar els documents també empraré Emacs, fent ús [org-noter](https://github.com/weirdNox/org-noter) i [pdf-tools](https://github.com/vedang/pdf-tools):

![img](/home/cavicas/Pictures/ScreenShots/Screenshot_20230923_141504.png)

Totes les notes associades a una referència bibliogràfica estaran dins d'un mateix fitxer *.org* que també constituirà un node d'*org-roam*, de manera que en qualsevol moment puc buscar un node d'org-roam i si este fa referència a qualsevol document puc obrir el *pdf* i continuar prenent anotacions.

Emacs és un *editor de text* completament personalitzable mitjançant [Lisp](https://en.wikipedia.org/wiki/Emacs_Lisp), així que he decidit crear (amb l'ajuda de [phind.com](https://www.phind.com), ja que apenes conec Lisp) algunes funcions per facilitar-me certes tasques repetitives. Així, m'he creat funcions per realitzar traduccions amb [Argos Translate](https://github.com/argosopentech/argos-translate), per llegir en veu alta el text sel·leccionat (en aquest cas emprant [piper](https://github.com/rhasspy/piper)) i  per transcriure fonèticament el text seleccionat (mitjançant [espeak-ng](https://github.com/espeak-ng/espeak-ng)) -sí, per ara, són totes per facilitar-me la comprensió de documents escrits en anglés-.

A continuació mostre un exemple de les funcions creades:

    (defun speak-selected-text ()
      "Speak the text selected."
      (interactive)
      (let* ((text (buffer-substring-no-properties (region-beginning) (region-end)))
    	 (escaped-text (replace-regexp-in-string "'" "'\\\\''" text))
    	 (final-text (replace-regexp-in-string "\\*" "" escaped-text)))
        (shell-command (format "echo '%s' | piper --data-dir
        ~/.local/share/piper-voices/
      --model ~/.local/share/piper-voices/en_GB-alba-medium.onnx
    --output-raw  | aplay -r 22050 -f S16_LE -t raw" final-text))))

Totes les altres funcions són petites variacions d'aquesta, ja que són funcions que passen el text seleccionat a un programa extern.


<a id="orgc897e7d"></a>

### Paquets adicionals

Alguns dels paquets que he instal·lat en Emacs són corfu, vertico, embark, orderless, marginalia, languagetool, &#x2026;

És possible que més endavant puje a este repositori l'arxiu *.emacs* amb la meua configuració.


<a id="org46d9c6a"></a>

# Organització de la informació

Donat que empraré org-roam no necessite guardar i classificar la informació seguint la típica estructura de directoris i subdirectoris. El que tinc pensat és crear un node per a cadascuna de les assignatures del Màster, de manera que per a cada assignatura creare un sol fitxer *.org* que estarà al directori org-roam. Aquest fitxers estaràn també enllaçats al meu repositori *git* local del Màster.


<a id="orgdcb16ac"></a>

# Publicació de una web

Publicar tot el que escriga en format org-mode a html és realment directe, de manera que segurament publique una web amb el contingut elaborat per mi, i que em facilite, per exemple, accedir als apunts i treballs directament des del movil. La idea és mantindre la web emprant [github pages](https://pages.github.com/), tal com faig fer amb el [Treball de fi de Grau](https://casimirvictoria.github.io/TFG-Semiconductores_2D/), si bé en aquella ocasió no vaig acabar la web, ja que la vaig començar massa tard, i ara tinc pensat realitzar tot el treball en org-mode.

Hi ha moltes opcions que podem configurar quan volem exportar a html, però en principi sols modificaré l'estil *css* de la web creada, i per ara estic per decidir-me entre el de [worg](https://orgmode.org/worg/) i el de [tufte](https://edwardtufte.github.io/tufte-css/) (m'agrada molt l'estil senzill i elegant de worg.css, així com que la taula de continguts estiga plegada i siga flotant).


<a id="org4fdf3b7"></a>

# Notes finals

Per finalitzar volia fer notar que encara estic començant a vore com treballar comodament amb org-roam, però amb les poques proves que he realitzat sembla que sí que l'adoptare per gestionar tota la meua informació, no sols la relativa al màster. El fet de treballar amb text pla i buscar i relacionar la informació mitjançant una base de dades, tot baix una interfaç molt senzilla m'agrada molt. A més, el fet de poder personalitzar completament l'entorn al meu gust i necessitats és una passada.


<a id="org5e4c171"></a>

# Referències

Nagel, Jennifer (2014). *Knowledge: {{A}} Very Short Introduction*, {OUP Oxford}.

