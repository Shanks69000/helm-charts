# Helm Charts

Charts Helm pour déploiement Kubernetes des projets DevOps personnels.

## Charts disponibles

| Chart | Description | Version |
|---|---|---|
| [infra-health-checker](charts/infra-health-checker) | CronJob de vérification d'infra (HTTP, TLS, TCP, DNS) | 0.1.0 |

## Installation

```bash
helm install health-checker ./charts/infra-health-checker
```

Configuration via `values.yaml` ou `--set` :

```bash
helm install health-checker ./charts/infra-health-checker \
  --set schedule="0 */12 * * *" \
  --set image.tag=v1.0.0
```

## Structure

```bash
charts/
└── infra-health-checker/
│   ├── Chart.yaml
├── values.yaml
  ├── templates/
│   ├── configmap.yml
│   ├── cronjob.yml
│   ├── _helpers.tpl
│   └── NOTES.txt
└── .helmignore
```

## Validation

```bash
helm lint charts/infra-health-checker
helm template test-release charts/infra-health-checker
```

## Troubleshooting

**PermissionError sur `reports/latest.json`** : l'image `infra-health-checker` tourne avec `USER 1000` (non-root, défini dans le Dockerfile source). Le répertoire `reports/` créé au build appartient à root, non accessible en écriture à l'UID 1000 sans volume dédié. Fix : `emptyDir` monté sur `/app/reports` dans le CronJob (voir `templates/cronjob.yml`).

Réf : https://kubernetes.io/docs/tasks/configure-pod-container/security-context/