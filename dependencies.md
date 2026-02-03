# Anàlisi de Vulnerabilitats en Dependències

## 1. Eina Nativa de GitHub: Dependabot

### Què és?
L'eina inclosa a GitHub és **Dependabot**. És una eina automatitzada d'escaneig de dependències que monitoritza els fitxers del projecte (com `requirements.txt`). Compara les versions utilitzades amb la **GitHub Advisory Database**, que conté una llista de vulnerabilitats de seguretat conegudes (CVEs).

### Passos per activar-la
Per activar Dependabot en aquest repositori, he seguit aquests passos:
1.  He creat un directori de configuració específic: `.github/`.
2.  He creat el fitxer de configuració `dependabot.yml`.
3.  He definit l'ecosistema d'actualització (`pip`), el directori (`/`) i la freqüència (`weekly`) per minimitzar el soroll mantenint la seguretat.
4.  He fet commit i push del fitxer al repositori.

### Resultats
Com que he generat un `requirements.txt` nou utilitzant `pip-tools` amb les últimes versions disponibles, Dependabot actualment no hauria de reportar **cap vulnerabilitat crítica**. Si es troba una vulnerabilitat en el futur, Dependabot obrirà automàticament una Pull Request suggerint l'actualització a una versió segura.

---

## 2. Alternatives per a Núvol Privat (Anàlisi de Mercat)

Si allotgéssim aquest codi en un GitLab privat, Bitbucket o un servidor on-premise sense les funcions de GitHub, necessitaríem eines externes. A continuació, presento una comparativa de 3 eines rellevants basada en capacitats Núvol/Local, Llicència i idoneïtat per a CI/CD.

### Alternativa A: OWASP Dependency-Check
* **Descripció:** Una eina clàssica d'Anàlisi de Composició de Software (SCA) mantinguda per la fundació OWASP.
* **Codi Obert vs. Pagament:** 100% **Codi Obert (Open Source)** i gratuïta.
* **Núvol vs. Local:** Dissenyada principalment per a execució **Local** o en servidors on-premise. No requereix enviar codi a un núvol de tercers.
* **Idoneïtat per a CI:** Alta. Té un plugin natiu per a **Jenkins**, sent l'estàndard per a pipelines tradicionals. Genera informes HTML/XML complets.

### Alternativa B: Snyk
* **Descripció:** Una plataforma moderna de seguretat per a desenvolupadors que troba i corregeix vulnerabilitats en codi i contenidors.
* **Codi Obert vs. Pagament:** Producte de **Pagament (Comercial)** per a ús empresarial, tot i que ofereix un nivell gratuït limitat per a projectes individuals de codi obert.
* **Núvol vs. Local:** Basada en **Núvol (SaaS)**. Generalment requereix connectar el repositori als servidors de Snyk per a l'anàlisi, la qual cosa pot ser un inconvenient per a entorns privats molt estrictes.
* **Idoneïtat per a CI:** Excel·lent. S'integra perfectament amb gairebé totes les eines CI/CD (GitLab CI, CircleCI, Azure DevOps) i ofereix Pull Requests de correcció automàtica, similar a Dependabot.

### Alternativa C: Trivy (de Aqua Security)
* **Descripció:** Un escàner de seguretat "tot en un" conegut per la seva velocitat i versatilitat (escaneja repositoris, sistemes de fitxers i imatges docker).
* **Codi Obert vs. Pagament:** **Codi Obert** (Llicència Apache 2.0).
* **Núvol vs. Local:** **Local**. És un binari únic que s'executa totalment a la màquina on es llança. No s'envien dades a cap núvol extern, fent-lo perfecte per a núvols privats aïllats (air-gapped).
* **Idoneïtat per a CI:** Molt Alta (DevSecOps). Com que és una eina de línia de comandes (CLI) simple i extremadament ràpida, és molt fàcil d'afegir com a pas en qualsevol pipeline (GitLab CI, GitHub Actions) sense alentir el procés de build.

---

## 3. Execució Local d'una Alternativa

He triat **`pip-audit`** per provar la seguretat localment. És similar a Trivy/OWASP en filosofia (Codi Obert + Local) però especialitzada per a Python.

### Instal·lació
He instal·lat l'eina al meu entorn virtual:
```bash
pip install pip-audit