# Anàlisi de Vulnerabilitats en Dependències

## 1. Eina Nativa de GitHub: Dependabot

### Què és?
Dependabot és l'eina nativa de GitHub per a l'escaneig automatitzat de dependències (SCA). Monitoritza fitxers com `requirements.txt` i els compara amb la GitHub Advisory Database.

### Passos per activar-la
1.  Crear directori `.github/`.
2.  Crear fitxer `dependabot.yml` configurant l'ecosistema `pip` amb freqüència setmanal.
3.  Pujar-ho al repositori (GitHub Actions ho detecta automàticament).

### Resultats
En executar-se sobre el meu *fork*, Dependabot no ha trobat vulnerabilitats crítiques actualment, ja que he generat els requirements recentment amb les últimes versions segures ("Dependency Hardening").

---

## 2. Alternatives per a Núvol Privat (Market Analysis)

Aquí comparo 3 alternatives basant-me en els criteris de l'assignació: **Cloud vs. Local**, **Open Source vs. Paid** i **CI Suitability**.

### Alternativa A: OWASP Dependency-Check
* **Open Source vs. Paid:** És 100% **Open Source** i gratuïta.
* **Cloud vs. Local:** Execució **Local**. No requereix pujar codi al núvol, ideal per a servidors privats.
* **CI Suitability:** Alta. Genera informes molt detallats (HTML/XML), però és una eina una mica pesada (Java based).

### Alternativa B: Snyk
* **Open Source vs. Paid:** És un producte **Paid (Comercial)** (amb versió free tier). Està pensat per a empreses.
* **Cloud vs. Local:** Principalment **Cloud (SaaS)**. Requereix connectar el repo als servidors de Snyk.
* **CI Suitability:** Excel·lent. S'integra nativament amb GitHub/GitLab i ofereix solucions automàtiques (Auto PRs).

### Alternativa C: Trivy (o pip-audit)
* **Open Source vs. Paid:** Completament **Open Source**.
* **Cloud vs. Local:** Funcionament **Local**. És un binari lleuger o paquet Python que no envia dades fora.
* **CI Suitability:** Molt Alta. Perfecte per integrar al pipeline perquè és ràpid i bloqueja el build si troba errors.

---

## 3. Execució Tècnica i Integració

### Execució Local
He triat una eina local equivalent, **`pip-audit`**, per demostrar la seguretat en aquest entorn Python.

**Comanda:**
```bash
pip install pip-audit
pip-audit

Link github: https://github.com/MiquelJaume/sportsclub