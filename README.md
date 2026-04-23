# platform-helm-values

Helm `values.yaml` for **non-Istio** platform charts (cert-manager, External Secrets Operator, Kyverno install, observability, and so on). Istio values live in the sibling `istio-helm-values` repository.

**Observability** (Grafana Helm charts: `alloy`, `loki`, `mimir-distributed`, `tempo`, `grafana`) uses `clusters/<cluster>/{alloy,loki,mimir-distributed,tempo,grafana}/values.yaml`. Defaults are placeholders; set retention, storage classes, and Grafana admin credentials via secrets before production.

**Controllers** consumed by `argocd-applications` include `cert-manager`, `external-secrets`, `kyverno` (install chart only), `argo-rollouts`, and observability charts. Pin chart `targetRevision` in each `Application` to match the upstream Helm index before syncing.

## Layout

```text
clusters/<management|test|prod>/<chart-name>/values.yaml
```

Add optional overlays such as `values-prod.yaml` if you split concerns later.

## Assumptions

- Argo CD Applications in `argocd-applications` reference this repository via multi-source `$values/...` paths.
- Use `devendradk` in Argo CD manifest repository URLs, not in this repository.

## Related repositories

| Repository | Role |
|------------|------|
| `argocd-applications` | `Application` / `AppProject` |
| `istio-helm-values` | Istio-only values |
