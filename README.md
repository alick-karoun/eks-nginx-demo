# eks-nginx-demo
![Deploy](https://github.com/alick-karoun/eks-nginx-demo/actions/workflows/deploy.yaml/badge.svg)





## Architecture

```mermaid
flowchart LR
    Dev[Developer<br/>VS Code] -->|git push| Repo[GitHub repo]
    Repo --> GHA[GitHub Actions<br/>deploy.yaml]
    GHA -->|OIDC token| IAM[AWS IAM role<br/>GitHubActionsEKSDeploy]
    GHA -->|kubectl apply| EKS

    User[Visitor browser] -->|HTTP :80| IGW[Internet gateway]

    subgraph AWS[AWS cloud · eu-north-1]
        IAM
        IGW --> LB[Load balancer<br/>public, port 80]

        subgraph VPC[VPC · public subnets]
            subgraph EKS[EKS cluster nginx-demo]
                CM[ConfigMap<br/>index.html] --> Pods[nginx pods x2]
                Node[Node group nginx-nodes<br/>2 x t3.small]
            end
        end

        LB --> Pods
    end
```

## Screenshots
nginx running on EKS
![Nginx running on EKS](IMG_0023.png)

the cluster
![Cluster](IMG_0030.jpeg)

nodes
![nodes](IMG_0033.jpeg)

IAM entries in the cluster
![IAM entries in the cluster](IMG_0031.png)

roles made for EKS
![all roles made for eks](IMG_0029.jpeg)

trust relationship
![trust relationship](IMG_0034.png)

