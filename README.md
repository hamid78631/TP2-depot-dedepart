# TP2 - Conteneuriser un modèle et optimiser une image

Cours Cloud Computing & déploiement IA - Série TP 2.

Classifieur de revenus (jeu Adult Income) : entraînement (`src/train.py`) et inférence (`src/predict.py`) conteneurisés avec Docker.

## Lancer le projet

```bash
python data/download.py                      # récupère data/adult.csv et data/sample.csv
docker build -f Dockerfile.train -t adult-train:naif .
docker run --rm -v "$(pwd)/artifacts:/artifacts" adult-train:naif
docker build -f Dockerfile.serve.opt -t adult-serve:opt .
docker run --rm -v "$(pwd)/artifacts:/artifacts" adult-serve:opt
```

## Mesures (`bash benchmark.sh <etiquette> <dockerfile>`)

| # | Optimisation | Taille | Build à froid | Rebuild après modif. |
|---|---|---|---|---|
| 0 | Version naïve (référence) | 735 Mo | 156 s | 147 s |
| 1 | Base python:3.11-slim | 375 Mo | 135 s | 157 s |
| 2 | COPY requirements.txt avant COPY src/ | 263 Mo | 133 s | 3 s |
| 3 | .dockerignore complet | | | |
| 4 | pip install --no-cache-dir | | | |
| 5 | Construction multi-étapes | | | |
| 6 | Utilisateur non-root + HEALTHCHECK | | | |
