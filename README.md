# Inspection Planner

PWA mono-fichier pour planifier et suivre des visites d'intervention périodiques sur un parc de sites.

L'app fonctionne entièrement hors-ligne après le premier chargement, et propose une synchronisation cloud optionnelle pour partager les données entre plusieurs appareils.

## Fonctionnalités

- **Priorités** : liste des sites triés par urgence (fenêtre de visite dépassée, en cours, à venir)
- **Planning** : calendrier 16 semaines avec drag & drop, jours fériés, congés, événements
- **Contrats** : suivi des contrats en cours et terminés, préparation des renouvellements
- **Fiche site** : détail complet (interventions, dates, historique, photos, notes d'accès)
- **Catalogue** : bibliothèque personnelle de phrases réutilisables pour la génération assistée
- **Transmis** : sites délégués à un collègue, masqués des autres vues
- **Sauvegarde cloud** : sync automatique via [JSONBin.io](https://jsonbin.io) (optionnel)
- **Photos** : capture caméra ou galerie, compression auto à 1024px
- **Thème sombre / gris** : basculable depuis l'app
- **Import / export** : JSON ou CSV
- **PWA** : installable sur iOS et Android comme une app native

## Architecture

- **Mono-fichier** : tout (HTML, CSS, JS, React, données initiales) est dans `index.html`
- **React 19.2.5** : embarqué inline, pas de build, pas de dépendances externes
- **Stockage local** : `localStorage` avec clé `ispk-all`
- **Stockage de session** : `window.name` comme fallback
- **Sync cloud** : `fetch()` vers l'API JSONBin v3 (Master Key utilisateur)
- **Aucun tracker, aucune télémétrie**

## Utilisation

### En local

Ouvrir `index.html` dans un navigateur moderne (Chrome, Safari, Firefox récents). C'est tout.

### Sur iPhone / Android (installation PWA)

1. Héberger `index.html` (GitHub Pages, Netlify, Vercel, ou n'importe quel serveur HTTPS)
2. Ouvrir l'URL dans Safari (iOS) ou Chrome (Android)
3. *Partager → Ajouter à l'écran d'accueil*
4. L'icône apparaît sur l'écran d'accueil et l'app s'ouvre en plein écran

### Configurer la synchronisation cloud (optionnel)

1. Créer un compte gratuit sur [jsonbin.io](https://jsonbin.io)
2. Copier la *Master Key* depuis le dashboard
3. Dans l'app : ouvrir le menu de sync, coller la clé, *Connecter*
4. Les données se synchronisent automatiquement après chaque modification
5. Sur un autre appareil : utiliser la même Master Key + le Bin ID affiché dans l'app

## Données

Les données affichées au premier lancement ne sont que des **exemples génériques** (`Site 01`, `Site 02`, etc.). Elles sont remplacées dès qu'un utilisateur réel commence à saisir ses propres sites — et n'écrasent jamais les données existantes en `localStorage` ou en cloud.

## Modèle d'un site

```js
{
  id: "s01",
  name: "Site 01",
  ref: "REF001 PRJ001",       // numéro site + numéro projet (séparés par espace)
  loc: "Ville 01",
  type: "SPK_Q1",             // type d'intervention principal
  visits: ["2025-11-12"],     // historique des visites passées (plus récente d'abord)
  planned: "2026-06-02",      // date planifiée principale
  extraPlanned: [],           // dates planifiées supplémentaires
  clientStatus: "confirmed",  // confirmed | proposed | planned
  visitsPerYear: 2,           // fréquence (optionnel, sinon défaut du type)
  visitNum: "V1-2",           // numéro de visite courant
  contractStart: "2025-01-01",
  contractEnd: "2025-12-31",
  contractHours: 24,
  contractComplete: false,
  notes: "",
  // ... champs optionnels : siteComment, siteNotes, sitePhotos[],
  // commissionYear, standard, pendingRenewal, transferredTo, etc.
}
```

## Types d'intervention

| Code         | Couleur | Périodicité nominale | Ponctuel |
|--------------|---------|----------------------|----------|
| `SPK_Q1`     | bleu    | 183 j                | non      |
| `SPK_DT`     | violet  | 183 j                | non      |
| `SPK_TRIM`   | vert    | 91 j                 | non      |
| `SPK_DT_VA`  | cyan    | 91 j                 | non      |
| `RIA`        | ambre   | 365 j                | non      |
| `HYD`        | orange  | 365 j                | non      |
| `PCF`        | vert clair | 365 j             | non      |
| `GES`        | turquoise | 365 j              | oui      |
| `FORMATION`  | rose    | 365 j                | oui      |
| `OTHER`      | gris    | 365 j                | non      |

## Compatibilité

- Safari iOS 15+
- Chrome / Edge 100+
- Firefox 100+

## Licence

À définir (MIT recommandé pour usage personnel libre).

## Crédits

Construit avec React, déployable sur n'importe quel hébergement statique.
