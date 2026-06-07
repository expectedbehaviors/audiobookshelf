# Audiobookshelf Helm Chart

Baseline Helm chart for [Audiobookshelf](https://www.audiobookshelf.org) (self-hosted audiobook and podcast server). Uses the [bjw-s app-template](https://github.com/bjw-s/helm-charts).

## Subcharts

| Subchart | Source | Values prefix | Description |
|----------|--------|---------------|-------------|
| **audiobookshelf** (app-template) | [bjw-s helm-charts](https://github.com/bjw-s/helm-charts) | `audiobookshelf.*` | Deployment, multi-volume persistence, ingress. |
| **onepassworditem** | [expectedbehaviors/OnePasswordItem-helm](https://github.com/expectedbehaviors/OnePasswordItem-helm) | `onepassworditem.*` | Optional secrets sync (e.g. `TOKEN_SECRET`). |

## Secrets required

Create a Kubernetes Secret named **`audiobookshelf-secrets`** with at least:

- **`TOKEN_SECRET`** — required by Audiobookshelf for session signing.

Alternatively, enable `onepassworditem` and map a 1Password item to a Secret referenced in `envFrom`.

## Key values

| Area | Where | What to set |
|------|--------|-------------|
| Ingress | `audiobookshelf.ingress.main.hosts` | Your domain and TLS secret. |
| Media paths | `audiobookshelf.persistence.audiobooks/podcasts` | PVC size, storageClass, or `existingClaim`. |
| Config/metadata | `audiobookshelf.persistence.config/metadata` | PVC or `existingClaim`. |
| Secrets | `onepassworditem.items` or manual Secret | `TOKEN_SECRET` for the app. |

## Install

```bash
helm dependency update .
helm install audiobookshelf . -f my-values.yaml -n audiobookshelf --create-namespace
```

**From Helm repo (expectedbehaviors):**

```bash
helm repo add audiobookshelf https://expectedbehaviors.github.io/audiobookshelf
helm install audiobookshelf audiobookshelf/audiobookshelf -f my-values.yaml -n audiobookshelf --create-namespace
```

## Render & validation

```bash
helm dependency update . && helm template audiobookshelf . -f values.yaml -n audiobookshelf
```

## Support this project

I build tools to get the best homelab experience I can from what's available and to grow as a programmer along the way. If you'd like to contribute, donations go toward homelab operating costs and subscriptions that keep this tooling maintained. Optional and appreciated.

[![Donate with PayPal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/donate/?business=9RHVW92WMWQNL&no_recurring=0&item_name=Optional+donations+help+support+Expected+Behaviors%E2%80%99+open+source+work.+Thank+you.&currency_code=USD)
