# hello-app (FastAPI)

Aplicação de exemplo FastAPI para o projeto CI/CD.

Endpoints:
- GET / -> retorna uma mensagem JSON.

Arquivos importantes:
- main.py
- Dockerfile
- requirements.txt

Build local:
```
docker build -t <DOCKER_USERNAME>/hello-app:local .
docker run -p 8080:80 <DOCKER_USERNAME>/hello-app:local
```

Requisitos de secrets no GitHub Actions:
- DOCKER_USERNAME
- DOCKER_PASSWORD
- SSH_PRIVATE_KEY (chave SSH com permissão de push no repo de manifests)
- MANIFESTS_REPO (SSH URL do repo de manifests, ex: git@github.com:usuario/hello-manifests.git)
- MANIFESTS_PAT (token com permissão `repo` para criar Pull Requests no repo de manifests)
## Estrutura dos repositórios

- **Repositório de código:**  
  Contém apenas o código da aplicação FastAPI, Dockerfile e requisitos.
- **Repositório de manifestos:**  
  Contém apenas os arquivos YAML de Deployment e Service do Kubernetes.

---

## Passos realizados

### 1. Criação da API FastAPI

- Estrutura básica (`main.py`):
- ```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}


    - Dockerfile para build da imagem.

---

### 2. Versionamento no GitHub

- Código fonte e Dockerfile ficaram no repositório `meuapp-fastapi`.
- Manifestos Kubernetes (`implantação.yaml`, `serviço.yaml`) ficaram no repositório `meuapp-fastapi-manifestos`.

---

### 3. Manifestos Kubernetes

- **implantação.yaml**
apiVersion: v1
kind: Service
metadata:
name: meuapp-service
spec:
selector:
app: meuapp
ports:
- protocol: TCP
port: 80
targetPort: 80
type: ClusterIP

---

### 4. Deploy automatizado com ArgoCD

- ArgoCD configurado para monitorar o repositório de manifestos.
- Sincronização automática do cluster Kubernetes quando há push no repo.
- Comandos para validar o deploy e os serviços:
kubectl get pods -n default
kubectl get svc -n default

---

### 5. Testando a aplicação FastAPI

- Port-forward para acessar local:
- Acesse no navegador:
 http://localhost:8000
<img width="1917" height="882" alt="Captura de tela 2025-10-21 183140" src="https://github.com/user-attachments/assets/3751071b-4e26-4f60-b9d9-34a1e0377208" />
![projeto](https://github.com/user-attachments/assets/6d055002-53fa-4d3b-9d93-75e0b506b5c6)
