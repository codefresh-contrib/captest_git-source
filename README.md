# captest_git-source

This is the Default Git Source repo for a demo Codefresh Software Delivery Platform runtime. While this runtime is no longer installed in my demo environment, I keep this repo around to illustrate which files need to updated for a successful Runtime installation. Some links below use this repo's git history.

The related Runtime GitOps repo is https://github.com/codefresh-contrib/captest.

## Runtume installation process

1. Make sure your cluster has an ingress controller installed. I installed NGINX in mine, via the command below. FYI, this particular version of the NGINX controller requires K8s cluster **version 1.20** or higher.
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/cloud/deploy.yaml
```

2. Start the installation process in the UI by going to [Settings -> Runtimes](https://g.codefresh.io/2.0/account-settings/runtimes), and clicking the **Add Runtime** button. Follow the prompts to start the installation via the Codefresh CLI. While your Runtime installation is in progress, you’ll need to edit several ingress-related manifest files in your 2 gitops repos, starting with…

3. In your runtime’s gitops repo, edit the ingress manifests for app-proxy and workflows - add needed annotations and path
  - App proxy ingress
    - https://github.com/codefresh-contrib/captest/blob/4869aaf113282f5816abf3f26d1a700a10cd18ce/apps/app-proxy/overlays/captest/ingress.yaml#L7-L10
    - https://github.com/codefresh-contrib/captest/blob/4869aaf113282f5816abf3f26d1a700a10cd18ce/apps/app-proxy/overlays/captest/ingress.yaml#L20-L21
  - Workflows ingress
    - https://github.com/codefresh-contrib/captest/blob/4869aaf113282f5816abf3f26d1a700a10cd18ce/apps/workflows/overlays/captest/ingress.yaml#L7-L10
    - https://github.com/codefresh-contrib/captest/blob/4869aaf113282f5816abf3f26d1a700a10cd18ce/apps/workflows/overlays/captest/ingress.yaml#L20-L21

4. In your default Git Source repo, edit the GitHub Event Source and its Ingress
  - https://github.com/codefresh-contrib/captest_git-source/blob/main/resources_captest/eventSource-github.yaml#L30-L31
    - In my case, I obtained the ingress controller’s DNS name via `kubectl get svc ingress-nginx-controller -n ingress-nginx`
    - Also, you’ll probably need to change `http` to `https` here
  - https://github.com/codefresh-contrib/captest_git-source/blob/main/resources_captest/ingress-eventSource-github.yaml#L6-L7
