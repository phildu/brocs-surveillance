# Sonde externe des sites Brocs

Ce dépôt ne contient pas de code métier : il vérifie depuis les serveurs de
GitHub que les deux hôtes de production répondent.

- Sondes (toutes attendues en 200) :
  - `https://brocs.arrimage.com/health.php`
  - `https://brocs.arrimage.com/portail/`
  - `https://brocs.arrimage.com/la-friperie-de-naty/` (boutique témoin)
  - `https://brocante.arrimage.com/health.php`
  - `https://brocante.arrimage.com/`
- Fréquence : toutes les 5 minutes (`*/5 * * * *`)
- Tout autre code que 200 (503 = base, disque ou quota IA en défaut) ouvre une
  issue « Surveillance : site indisponible », assignée au propriétaire du dépôt.
- Au retour à 200, l'issue est refermée automatiquement.

`/health.php` répond 200 avec `{"ok":true}` quand la base s'ouvre en écriture,
l'espace disque dépasse 500 Mo et aucune erreur fatale ni refus de quota IA
n'est survenu dans la fenêtre surveillée.

Documentation côté site : `includes/watchdog.php` et `health.php`.

## Limites connues

- GitHub désactive les tâches planifiées d'un dépôt public après 60 jours sans
  activité : faire un commit (même vide) relance le compte à rebours.
- Les tâches planifiées de GitHub peuvent avoir plusieurs minutes de retard.
