# Devis Pro

Générateur de devis : formulaire client, lignes de prestations, aperçu
imprimable en PDF et tableau de bord avec graphiques.

Application locale, sans serveur ni compte.

## Lancer l'application

Aucune dépendance, aucune étape de build.

```bash
python -m http.server 5600
```

Puis ouvrir http://127.0.0.1:5600 (ou utiliser Live Server dans VS Code).

Au premier lancement, l'onglet **Réglages** s'ouvre : renseignez-y le nom de
votre structure, vos coordonnées, votre logo et votre devise. Ces informations
apparaîtront sur chaque devis.

## Votre identité n'est pas dans le code

Le nom, l'e-mail, le représentant et le logo sont saisis dans les réglages et
conservés dans le `localStorage` de votre navigateur. Ils ne figurent nulle
part dans ce dépôt.

C'est délibéré : une personne qui récupère ce fichier obtient un générateur de
devis **vierge**, et non un générateur de devis à votre nom. Sans cela,
n'importe qui pouvait produire un document commercial d'apparence authentique
portant votre identité.

Le corollaire : votre identité ne suit pas d'un navigateur à l'autre. Elle est
incluse dans le fichier de sauvegarde (voir ci-dessous), ce qui permet de la
retrouver sur une autre machine.

## Où sont stockés les devis

Dans le `localStorage`, sous la clé `uptech_devis` (les réglages sous
`uptech_agence`). Trois conséquences :

1. **Rien ne quitte la machine.** Aucun serveur, aucun compte.
2. **Vider les données de navigation efface tout.** D'où les boutons
   **⬇ Sauvegarder** et **⬆ Restaurer** du tableau de bord : le fichier JSON
   exporté contient les devis *et* les réglages, et c'est votre seule copie.
3. **Le stockage est lié à l'adresse.** Les devis créés sur `localhost:5600`
   ne suivent pas vers une version en ligne, et inversement. Pour changer
   d'adresse : exporter depuis l'ancienne, importer sur la nouvelle.

La restauration ne détruit rien : un devis déjà présent est conservé tel quel,
seuls les devis absents sont ajoutés, et les réglages ne sont repris que si
aucune identité n'est encore saisie.

## Stack

- HTML/CSS/JS en un seul fichier, sans framework
- [Chart.js](https://www.chartjs.org) 4.4.1 via CDN, pour les deux graphiques
- Impression via `window.print()` et une feuille de style `@media print`
- Logo redimensionné à 256 px par `<canvas>` avant stockage, pour ne pas
  saturer le `localStorage`

## Mise en ligne

L'application fonctionne en local sans rien installer, et c'est l'usage prévu.

Si vous la déployez malgré tout, protégez l'accès (sur Vercel :
Settings → Deployment Protection). Une instance publique reste un générateur
vierge — l'identité n'est pas dans le code — mais rien n'oblige à la laisser
ouverte à tous.

## Reste à faire

- **Sauvegarde automatique.** L'export est manuel : il faut y penser.
- **Modifier un devis existant.** L'historique permet de consulter et de
  changer le statut, pas de rouvrir un devis pour le corriger.
- **Mentions légales du devis.** Aucun champ ne prévoit le numéro
  d'immatriculation ni les conditions générales, souvent attendus sur un
  document commercial.
