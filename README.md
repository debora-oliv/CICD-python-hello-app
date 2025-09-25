# Objetivo
Automatizar o ciclo completo de desenvolvimento, build, deploy e execução de uma aplicação FastAPI simples, usando GitHub Actions para CI/CD, Docker Hub como registry, e ArgoCD para entrega contínua em Kubernetes local com Rancher Desktop.

# Pré-requisitos

- [ ] Conta no GitHub com repositório público
- [ ] Conta no Docker Hub com token de acesso
- [ ] Rancher Desktop com Kubernetes habilitado
- [ ] kubectl configurado corretamente
- [ ] ArgoCD instalado no cluster local
- [ ] Python 3, Docker e Git instalados

# Deployment

**1. Fazer um fork desse repositório**

**2. Criar um repositório para os manifestos**

**3. Criar um arquivo service.yaml e deployment.yaml no repositório dos manifestos**

**4. Configurar o Argo CD no cluster local**

- Criar um namespace
    ```sh
    kubectl create namespace argocd
    ```
    
- Instalar Argo CD no namespace criado
    ```sh
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```
    
**5. Instalar o Argo CD CLI para acesso via linha de comando**
 
- Instalar em ambiente **Windows**
    
    ```sh
    choco install argocd-cli
    ```
    
- Instalar em ambiente **Linux**
    
    ```sh
    curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64

    sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd

    rm argocd-linux-amd64
    ```

**6. Logar no Argo CD via linha de comando**

- Pegar senha inicial do Argo CD em ambiente **Windows**
    
    ```sh
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($(kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}")))
    ```
  
- Pegar senha inicial do Argo CD em ambiente **Linux**
    
    ```sh
    kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
    ```
    
- Port-forward para acessar o ArgoCD
  
    ```sh
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```

- Logar no Argo CD
  
    ```sh
    argocd login localhost:8080
    ```
    > Usuário: `admin`
    > 
    > Senha: `sua_senha_inicial`

**8. Acessar a aplicação**

No seu browser: `localhost:8080`

**9. Acessar a interface do Argo CD**

No seu browser: `localhost:30080`
