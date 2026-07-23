# inducks-watch

Surveille [inducks.org](https://inducks.org) et archive le dump `isv.tgz` dès qu'il redevient accessible.

## Fonctionnement

Un workflow GitHub Actions (`.github/workflows/inducks-watch.yml`) s'exécute :

- **toutes les 4 heures** (`cron: '0 */4 * * *'`)
- **à la demande** via le bouton *Run workflow* (`workflow_dispatch`)

À chaque exécution :

1. **Probe** — un `curl` teste `https://inducks.org/inducks/isv.tgz` et récupère le code HTTP.
2. **Download** — si le code est `200`, le dump est téléchargé, sa taille et son `sha256sum` sont affichés, et la liste des premiers fichiers de l'archive est loguée.
3. **Upload** — le dump est publié comme *artifact* (`isv-<run_number>`), conservé **90 jours**.

## Récupérer le dump

Onglet **Actions** → ouvre le run vert le plus récent → section **Artifacts** → télécharge `isv-<numéro>`.

## Conservation

Les artifacts sont gardés au maximum **90 jours**. Pour un archivage durable, il faudrait committer le dump dans le repo ou le pousser vers une *release*.
