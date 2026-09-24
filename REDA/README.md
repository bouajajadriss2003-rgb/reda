# 💧 Assistant SRM — Chatbot service client

Assistant virtuel pour une **Société Régionale Multiservices** (eau potable,
électricité, assainissement liquide, éclairage public).

Construit **uniquement** en HTML, CSS et JavaScript : aucun framework, aucune
bibliothèque externe, aucune étape de compilation.

---

## ⚠️ À FAIRE AVANT TOUTE UTILISATION RÉELLE

Le chatbot contient des **valeurs à remplacer**. Ouvrez `script.js` et modifiez
le bloc tout en haut du fichier :

```javascript
const SRM = {
  region:    '[NOM DE LA RÉGION]',
  telephone: '[NUMÉRO DU CENTRE D\'APPEL]',
  urgence:   '[NUMÉRO D\'URGENCE 24h/24]',
  site:      '[SITE WEB]',
  horaires:  '[HORAIRES DES AGENCES]'
};
```

Ces cinq valeurs sont utilisées **partout** : prompt de l'IA, réponses du mode
démo, messages de contact. Une seule modification suffit.

> 🔴 **Les réponses du mode démo décrivent des démarches générales.** Elles ne
> contiennent volontairement **aucun tarif, numéro, adresse ni délai inventé**,
> mais elles doivent être **relues et validées par la SRM** avant toute mise à
> disposition du public. Complétez-les avec les procédures officielles.

---

## 📁 Fichiers du projet

| Fichier | Rôle |
|---|---|
| `index.html` | Structure : barre latérale, fil de discussion, saisie, paramètres |
| `style.css` | Design complet : thème sombre/clair, animations, responsive mobile |
| `script.js` | Configuration SRM, prompt de l'IA, base de connaissances, appel API |
| `README.md` | Ce document |

---

## 🚀 Lancer le projet

### Mode démo (immédiat, sans internet)

Double-cliquez sur **`index.html`**. L'assistant répond à partir de sa base
locale de **23 sujets SRM**. Idéal pour une démonstration ou une soutenance.

### Avec la véritable IA

1. Ouvrez `script.js` et renseignez la clé :

```javascript
const API_KEY = 'sk-ant-...';   // vide = mode démo
```

2. L'appel API exige un **serveur local** (le protocole `file://` est bloqué
   par le navigateur). Dans le dossier du projet :

```bash
python -m http.server 8000      # ou :  npx serve .
```

3. Ouvrez **http://localhost:8000**

> 💡 Avec **VS Code**, l'extension *Live Server* fait la même chose.

### ⚠️ Sécurité de la clé

La clé est écrite dans `script.js`, donc **visible par tout visiteur** du site.

- ❌ Ne publiez **jamais** ce site en ligne avec une clé à l'intérieur
- ❌ Ne mettez pas `script.js` sur GitHub avec une clé
- ✅ En production, la clé doit rester sur un **serveur intermédiaire** qui
  relaie les appels : la page ne doit jamais la connaître

---

## 🛡️ Comment l'assistant reste dans son rôle

L'assistant est cadré **à deux niveaux**, pour qu'il fonctionne avec ou sans IA.

### 1. Le prompt système (quand l'IA est connectée)

La constante `SYSTEM_PROMPT` dans `script.js` impose les règles : périmètre
limité à la SRM, refus poli hors sujet, interdiction d'inventer des
informations, jamais de mot de passe ni de coordonnées bancaires, réponse dans
la langue de l'utilisateur, résistance aux tentatives de contournement.

### 2. Le routage du mode démo (sans IA)

Chaque question est classée dans l'une de trois catégories :

| Cas | Comportement |
|---|---|
| Sujet connu | Réponse détaillée avec étapes numérotées |
| Question SRM sans réponse préenregistrée | Orientation vers le centre d'appel — **aucune invention** |
| Question hors sujet | Refus poli, reprenant le message officiel |

Le refus hors sujet est exactement :

> Désolé, je suis l'assistant de la SRM [RÉGION] et je peux seulement répondre
> aux questions concernant nos services : eau, électricité, assainissement,
> factures et démarches clients. Comment puis-je vous aider ?

---

## 📚 Les 23 sujets du mode démo

**Factures** — consulter, payer, facture élevée, duplicata et attestations

**Démarches** — souscrire, résilier, changement de nom, branchement,
déménagement, réclamation

**Incidents** — coupure d'électricité, coupure d'eau, fuite, problème de
compteur, assainissement, éclairage public, remise en service après impayé,
relevé d'index, urgence et sécurité

**Général** — accueil, liste des sujets (`aide`), contact et agences,
remerciements

Tapez **`aide`** dans le chat pour afficher la liste complète.

### Langues reconnues

La recherche ignore les accents et la ponctuation, et reconnaît le français
ainsi que des mots de darija : `salam`, `choukran`, `lma` (eau), `daw` /
`trisiti` (électricité), `fatoura` (facture), `qta3` (coupure), `wakala`
(agence), `khalas` (payer).

---

## ✨ Fonctionnalités de l'interface

- Réponses en **streaming** (texte mot à mot) avec bouton **stop**
- Mémoire des **20 derniers messages** de la conversation
- Historique **sauvegardé dans le navigateur**, plusieurs conversations
- **Export** d'une conversation au format Markdown
- Thème **sombre / clair**, **responsive** mobile
- Rendu **Markdown** : titres, listes numérotées, citations, gras
- Affichage optionnel du **raisonnement** de l'IA

**Raccourcis clavier**

| Touche | Action |
|---|---|
| `Entrée` | Envoyer |
| `Maj` + `Entrée` | Nouvelle ligne |
| `Ctrl` + `K` | Nouvelle conversation |
| `Échap` | Fermer la modale ou le menu |

---

## 🎨 Personnaliser

**Ajouter un sujet au mode démo** — complétez le tableau `DEMO_ANSWERS` dans
`script.js` :

```javascript
{
  sujet: 'Compteur intelligent',
  keys: ['compteur intelligent', 'smart meter', 'telereleve'],
  text: `Explication de la démarche…\n\n` + CONTACT
}
```

La constante `CONTACT` ajoute automatiquement le bloc téléphone / site /
agence. Ajoutez aussi les mots nouveaux à `MOTS_DOMAINE` pour qu'une question
proche soit orientée plutôt que refusée.

**Changer les couleurs** — au début de `style.css` :

```css
:root {
  --accent:   #0e7fc1;   /* bleu principal */
  --accent-2: #3fc1c9;   /* cyan secondaire */
}
```

---

## ❓ Problèmes fréquents

| Symptôme | Cause et solution |
|---|---|
| Les réponses affichent `[NOM DE LA RÉGION]` | Normal : remplissez le bloc `SRM` en haut de `script.js`. |
| L'assistant refuse une question pourtant liée à la SRM | Ajoutez le mot-clé au sujet concerné, et à `MOTS_DOMAINE`. |
| « Connexion impossible » | La page est ouverte en `file://`. Lancez un serveur local. |
| « Clé API refusée » | Clé incorrecte ou expirée dans `script.js`. |
| Mes modifications n'apparaissent pas | Cache du navigateur. Les fichiers portent un numéro de version (`style.css?v=5`) dans `index.html` : **augmentez-le** après chaque modification, ou faites `Ctrl` + `F5`. |
| L'historique a disparu | Le `localStorage` a été vidé. |

---

*Projet pédagogique — HTML, CSS et JavaScript natif.*
