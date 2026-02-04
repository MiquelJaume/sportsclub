# Anàlisi de Requeriments i Millora de la Seguretat

## 1. Problemes de Seguretat al Fitxer Original
El fitxer `requirements.txt` original presentava diversos riscos de seguretat i males pràctiques:

* **Manca de Verificació d'Integritat (Sense Hashes):** El fitxer llistava dependències però no incloïa els hashes criptogràfics. Això deixava la instal·lació vulnerable a **atacs Man-in-the-Middle (MitM)** o a repositoris PyPI compromesos. Sense hashes, `pip` no pot verificar si el paquet descarregat ha estat manipulat o conté codi maliciós injectat.
* **Ambigüitat (Input vs. Lock):** El fitxer actuava alhora com a "llista de desitjos" i com a fitxer de bloqueig. Això provoca entorns inconsistents (el clàssic "a la meva màquina funciona") perquè les subdependències podrien resoldre's en versions diferents segons l'ordinador on s'instal·li.

## 2. Solució Implementada
Per resoldre aquests problemes, he implementat una estratègia de **"Pinning with Hashing"** (Fixació amb Hashing) utilitzant l'eina `pip-tools`.

* **Metodologia:** He separat les dependències en dos fitxers diferenciats:
    * **`requirements.in` (Fitxer d'Entrada):** Conté les dependències d'alt nivell que necessit (ex: `Django`), sense restriccions estrictes de versió, per permetre actualitzacions de seguretat futures.
    * **`requirements.txt` (Fitxer de Bloqueig - Lock File):** Un fitxer generat automàticament que fixa estrictament *totes* les versions (incloses les subdependències transitives) i inclou els **hashes SHA256** per garantir la integritat.

* **Execució:**
    He utilitzat la següent comanda per generar el fitxer segur:
    ```bash
    pip-compile --generate-hashes requirements.in --output-file requirements.txt
    ```

## 3. Procediment de Manteniment en Producció
Per mantenir el software al llarg del temps (actualitzacions o canvis de llibreries), el procediment recomanat és:

1.  **Mai editar `requirements.txt` manualment.**
2.  **Editar `requirements.in`:** Si cal afegir, eliminar o actualitzar un paquet, es modifica aquest fitxer.
3.  **Recompilar:** Executar `pip-compile --generate-hashes requirements.in` en un entorn de desenvolupament o CI.
4.  **Testejar:** Verificar que l'aplicació funciona correctament amb les noves versions generades.
5.  **Commit & Deploy:** Pujar el nou `requirements.txt` al repositori per al desplegament.

## 4. Escenaris Previnguts
Amb aquesta nova estructura, estem prevenint:
* **Atacs a la Cadena de Subministrament (Supply Chain Attacks):** Si un atacant substitueix un paquet vàlid a PyPI per una versió compromesa (amb virus), la comprovació del hash fallarà i `pip` bloquejarà la instal·lació automàticament.
* **Desviació de l'Entorn (Environment Drift):** Assegurem que Producció, Staging i Desenvolupament executin exactament el mateix codi, evitant errors (bugs) causats per petites diferències de versió en les subdependències.

Link github: https://github.com/MiquelJaume/sportsclub