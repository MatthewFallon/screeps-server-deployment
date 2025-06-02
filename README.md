# Screeps-Launcher Kubernetes Deployment

This is serving as the configuration for my private screeps server with friends, but can also serve as a good starting point for others to copy from for creating their own deployments, happy to take suggestions and may put this up to the screeps-launcher project in time as a good template to look to, or convert to helm if people want to maintain a distributable version, but as this is niche it is mainly for my own purposes.

This includes everything except for the final public port opening, I didn't want to include that here as that can be really custom. You can obviously load balance out, but I **heavily** reccomend using an ingress controller (I will be using traefick on a k3s cluster) to create an ipallowlist. Screeps is incredibly outdated and the nodejs it runs on has multiple cves associated, you can generally think of this thing like a ticking time bomb I would tightly control who can access it unless you know what you're doing like the larger community servers.

## How to use this

Fork this repo, and modify the config.yml to meet your needs. 

You will need 2 resources to exist in your repo in advance:
- A namspace, my default is screeps you can change this in the root kustomization.yaml and run the below (substituting your namespace if modified):
```bash
kubectl create namespace screeps
``` 
- Your [steam API key](https://steamcommunity.com/dev/apikey) as a secret in the namespace 
```bash
kubectl create secret generic steam-key --from-literal=STEAM_KEY=<insert_your_steam_key_here>
```

Then you can run:
```bash
kubectl apply -k base/
```
to apply this config, you can utilize this config with argocd or others pretty easily to allow editing the config and redeploying as needed.