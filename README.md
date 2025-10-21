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
