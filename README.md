# Flux

## Installation

### flux

<https://fluxcd.io/docs/guides/repository-structure/#repo-per-app>

### telepresence

<https://www.telepresence.io/docs/latest/howtos/intercepts>

### brew

```sh
brew bundle

export GITHUB_TOKEN=<your-token>
```

## Use

### Bootstrap flux

```sh
flux bootstrap github \
  --owner=gitops-remote-dev \
  --repository=flux_telepresence_demo \
  --team=engineers \
  --path=clusters/develop
```

### Resources

```sh
flux get all
kubectl get GitRepository
kubectl get Kustomization # -n flux-system
kubectl get HelmRelease # -n ingress-nginx
```

### GitHub Access

Update `infrastructure/develop/secrets/git-auth.yaml` if necessary:

```sh
echo -n $GITHUB_ACTOR | base64
echo -n $GITHUB_TOKEN | base64
```

### Docker Access

Update `infrastructure/develop/secrets/registry-auth.yaml` if necessary:

```sh
echo "{\"auths\": {\"docker.pkg.github.com\": {\"auth\": \"$(echo "$GITHUB_ACTOR:$GITHUB_TOKEN" | base64)\"}}}" | base64
```

### Intercept traffic

```sh
cd ../blue
telepresence intercept blue --port 80:web --env-file .env.local
docker compose up -d

telepresence list

telepresence leave blue
docker compose down
```
