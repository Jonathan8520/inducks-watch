# inducks-watch

Surveille [inducks.org](https://inducks.org) et archive le dump `isv.tgz` dès qu'il redevient accessible, puis **ouvre une issue** pour te prévenir.

## Fonctionnement

Un workflow GitHub Actions (`.github/workflows/inducks-watch.yml`) s'exécute :

- **toutes les 4 heures** (`cron: '0 */4 * * *'`)
- **à la demande** via le bouton *Run workflow* (`workflow_dispatch`)

À chaque exécution :

1. **Probe** — un `curl` teste `https://inducks.org/inducks/isv.tgz` et récupère le code HTTP.
2. **Download** — si le code est `200`, le dump est téléchargé ; sa taille et son `sha256sum` sont calculés, et les premiers fichiers de l'archive sont logués.
3. **Upload** — le dump est publié comme *artifact* (`isv-<run_number>`), conservé **90 jours**.
4. **Issue** — une issue est ouverte automatiquement pour te prévenir (avec taille, sha256 et liens). Un garde-fou anti-doublon évite d'en créer une nouvelle à chaque run tant qu'une issue est déjà ouverte.

> Tant que l'issue reste ouverte, aucune nouvelle issue n'est créée. **Ferme-la une fois le dump récupéré** pour réarmer la notification.

## Notifications

GitHub t'envoie un e-mail / une notif dès qu'une issue est ouverte sur le repo (selon tes réglages *Watch*). Assure-toi de *watcher* le repo (éil en haut → *All Activity* ou *Issues*).

## Récupérer le dump

Onglet **Actions** → ouvre le run vert le plus récent → section **Artifacts** → télécharge `isv-<numéro>`.

## Conservation

Les artifacts sont gardés au maximum **90 jours**. Pour un archivage durable, il faudrait committer le dump dans le repo ou le pousser vers une *release*.
