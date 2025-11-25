# Testing Artifactory Image Blocking Policy

## 1. Install Gatekeeper
helm repo add gatekeeper https://open-policy-agent.github.io/gatekeeper/charts
helm install gatekeeper gatekeeper/gatekeeper --namespace gatekeeper-system --create-namespace

## 2. Apply OPA Rules
kubectl apply -f ct-forbid-image-prefix.yaml
kubectl apply -f constraint-forbid-image-prefix.yaml

## 3. Test Fixtures
Apply the manifests in fixtures:
cd .\fixtures\block-yaml-files

kubectl apply -f deployment-multi-container-should-block.yaml
kubectl apply -f deployment-multi-container-should-permit.yaml
kubectl apply -f block-deploy.yaml
kubectl apply -f block-cronjob.yaml
kubectl apply -f block-initcontainer.yaml
kubectl apply -f block-job.yaml
kubectl apply -f block-statefulset.yaml
...

## Expected
- Resources using images from `af.switchmedia.asia` should be BLOCKED
- Resources using only docker.io or other registries should be PERMITTED
