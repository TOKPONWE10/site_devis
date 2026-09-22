# UpTech — Devis Pro

Générateur de devis pour UpTech : formulaire client, lignes de prestations,
aperçu imprimable en PDF et tableau de bord avec graphiques.

Outil interne, pas un site vitrine.

## Lancer l'application

Aucune dépendance, aucune étape de build.

```bash
python -m http.server 5600
```

Puis ouvrir http://127.0.0.1:5600 (ou utiliser Live Server dans VS Code).

## Où sont stockés les devis

Les devis vivent dans le `localStorage` du navigateur, sous la clé
`uptech_devis`. Cela a trois conséquences à connaître :

1. **Ils ne quittent jamais la machine.** Aucun serveur, aucun compte.
2. **Vider les données de navigation les efface tous.** D'où les boutons
   **⬇ Sauvegarder** et **⬆ Restaurer** du tableau de bord : exportez
   régulièrement, le fichier JSON obtenu est votre seule copie.
3. **Le stockage est lié à l'adresse.** Les devis créés sur
   `localhost:5600` ne suivent pas vers une version en ligne, et
   inversement. Pour changer d'adresse : exporter depuis l'ancienne,
   importer sur la nouvelle.

La restauration fusionne sans écraser : un devis déjà présent est conservé
tel quel, seuls les devis absents sont ajoutés.

## Changer de devise

Tout le formatage monétaire — champs de saisie, total, devis imprimé,
graphiques — découle d'une seule déclaration, en tête du script :

```js
var DEVISE = { symbole: 'FCFA', decimales: 0 };
```

## Stack

- HTML/CSS/JS en un seul fichier, sans framework
- [Chart.js](https://www.chartjs.org) 4.4.1 via CDN, pour les deux graphiques
- Impression via `window.print()` et une feuille de style `@media print`

## Reste à faire

- **Sauvegarde automatique.** L'export est manuel : il faut y penser. Un
  rappel périodique, ou une synchronisation vers un fichier, éviterait
  l'oubli.
- **Modifier un devis existant.** L'historique permet de consulter et de
  changer le statut, pas de rouvrir un devis pour le corriger.
- **Coordonnées de l'agence en dur.** Le nom, l'e-mail et le pied de page du
  devis imprimé sont écrits dans `buildPreview()`. Les regrouper dans une
  constante, comme `DEVISE`, faciliterait leur mise à jour.
- **Logo.** `logo.jpeg` fait 1024 × 1024 pour un affichage en 38 px. Le
  réduire allégerait le chargement.
