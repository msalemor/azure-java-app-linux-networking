# Azure Java App Linux App Networking

## Some scenarios in order of improved security

### App Services Java App with PostgreSQL (Deploy code)

```mermaid
graph LR;
  A((Internet))--Access Restrictions<br>ie IP Filter-->a2;
  subgraph "Linux App Service Plan"
    a2[Web App]
  end  
  a2--Service Tags-->D(Azure<br/>PostgreSQL);
  classDef unsafe fill:red,color:black;
  class A,B,C unsafe;
  classDef semisafe fill:darkorange,color:black;
  class D,a2 semisafe;
```

Code/container deployment:
- From VS Code or VS Studio
- From Azure CLI
- Recommended: Automated CI/CD pipeline (i.e. Git actions/ADO)

Suitable scenarios:
- Dev, Test, POC

### App Services Container Java App with PostgreSQL (Deploy a Docker container)

```mermaid
graph LR;
  A((Internet))--Access Restrictions<br>ie IP Filter-->a2;
  subgraph "Linux App Service Plan"
    a2[Web App<br/>For Containers]
  end  
  a2--Service Tags-->D(Azure<br/>PostgreSQL);
  classDef unsafe fill:red,color:black;
  class A,B,C unsafe;
  classDef semisafe fill:darkorange,color:black;
  class D,a2 semisafe;
```

JAR or WAR Code deployment:
- From VS Code
- From Azure CLI
- Recommended: Automated CI/CD pipeline (i.e. Git actions/ADO)

Suitable scenarios:
- Dev, Test, POC

### AppGW and Private Enpoint App Services App (code or container)

```mermaid
graph LR;
  A((Internet))-->H((Public<br/>IP));
  H-->F[AppGw<br/>WAF]
  F--Subnet<br/>Restriction-->a2;
  subgraph "Linux App Service Plan"
    a2(Web App<br/>or Web App<br/>For Containers)
  end  
  a2-->D(Private<br/>Endpoint);
  D-->G(Azure<br/>PostgreSQL);
  classDef unsafe fill:red,color:black;
  class A,B,C unsafe;
  classDef semisafe fill:darkorange,color:black;  
  classDef safe fill:darkgreen,color:black;
  class D,F,a2,G safe;
```

Security at this level:
- Public IP can be protected by DDOS
- TLS by default
- AppGW deployed to subnet and can do SSL offloading
- Traffic flows from internet to the application via Application Gateway
- The application is protected against common attacks via the AppGw WAF
- Traffic flows from App to PostgreSQL via VNET integration, the backend subnet and the PostgreSQL Private Endpoint
- All traffic stays within the Azure backbone

JAR or WAR Code deployment:
- From VS Code
- From Azure CLI
- Recommended: Automated CI/CD pipeline (i.e. Git actions/ADO)

Suitable scenarios:
- QA and production security sensitive workloads

### Front Door Standard App Services Java App (code or containers)

### Front Door Premium App Services

### Azure Container Apps External Java App

### Azure Kubernetes Service External Ingress Java App
