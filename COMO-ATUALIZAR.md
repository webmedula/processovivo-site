# Site do Processo Vivo no GitHub — passo a passo (kit v1.0.1)

Objetivo: o site www.processovivo.com.br passa a morar num repositório do GitHub. Para atualizar, você troca o arquivo no GitHub Desktop, clica em **Push** e o site se publica sozinho no servidor, em cerca de 1 minuto.

---

## Parte 1 — Repositório (uma vez só)

> O repositório `webmedula/processovivo-site` já foi criado e já recebeu a publicação automática, o README e este guia. No GitHub Desktop, use **File > Clone repository** e escolha `webmedula/processovivo-site`. Depois, coloque na pasta clonada o `index.html` (renomeie o `site-vendas-v2.2.0-index.html`) e siga a partir do item 3 abaixo.

### Se fosse criar do zero

1. No GitHub Desktop: **File > New repository**.
   - Name: `processovivo-site`
   - Local path: a pasta onde você guarda os projetos
   - Marque **Initialize with a README**: não (o kit já tem um README)
2. Copie para dentro da pasta criada **tudo** o que veio no kit (`index.html`, `README.md`, `COMO-ATUALIZAR.md`, `.gitignore` e a pasta `.github`).
   - A pasta `.github` começa com ponto e pode ficar escondida no Windows: em "Exibir", marque "Itens ocultos".
3. Copie do servidor os arquivos que já estão no ar e ainda não estão no kit:
   - No CyberPanel: **Websites > List Websites > processovivo.com.br > File Manager**, pasta `public_html`.
   - Baixe e coloque na pasta do repositório: a pasta `img/`, a pasta `fontes/`, `termos.html`, `privacidade.html` e qualquer outro arquivo do site (ex.: `robots.txt`, `favicon.ico`).
   - **Não** copie o `index.html` do servidor: o do kit (v2.2.0) é o novo.
4. No GitHub Desktop: escreva o resumo `Site v2.2.0` e clique em **Commit to main**.
5. Clique em **Publish repository**. Pode deixar **privado** (Keep this code private): o site continua público, só o código fica fechado.

## Parte 2 — Ligar a publicação automática (uma vez só)

### 2.1 Criar uma chave SSH só para o GitHub

No terminal do servidor (CyberPanel > **Terminal**, ou SSH):

```bash
ssh-keygen -t ed25519 -C "github-deploy-site" -f ~/.ssh/github_deploy_site -N ""
cat ~/.ssh/github_deploy_site.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
cat ~/.ssh/github_deploy_site
```

O último comando mostra a **chave privada** (começa em `-----BEGIN OPENSSH PRIVATE KEY-----`). Copie tudo, incluindo a primeira e a última linha.

### 2.2 Descobrir a pasta e o dono dos arquivos do site

```bash
ls -ld /home/processovivo.com.br/public_html
```

O resultado mostra o dono, por exemplo `drwxr-x--x 5 proce1234 nobody ... public_html`. O dono aqui é `proce1234`. Anote.

### 2.3 Cadastrar os segredos no GitHub

No site do GitHub, no repositório `processovivo-site`: **Settings > Secrets and variables > Actions > New repository secret**. Crie um por um:

| Nome | Valor |
|---|---|
| `SSH_HOST` | IP do servidor |
| `SSH_PORT` | `22` (ou a porta do seu SSH) |
| `SSH_USER` | `root` |
| `SSH_KEY` | a chave privada copiada no 2.1 |
| `SITE_DIR` | `/home/processovivo.com.br/public_html` |
| `SITE_OWNER` | o dono anotado no 2.2 (ex.: `proce1234`) |

### 2.4 Testar

No GitHub: aba **Actions > Publicar site > Run workflow**. Em cerca de 1 minuto deve aparecer o sinal verde. Abra www.processovivo.com.br e confira no rodapé: **Site v2.2.0**.

Se der vermelho, clique na execução para ver o erro. Os mais comuns:
- `Permission denied (publickey)`: a chave pública não entrou no `authorized_keys` ou o `SSH_KEY` foi colado incompleto.
- `Connection timed out`: o firewall do servidor (CyberPanel > Security > Firewall) está bloqueando a porta do SSH para fora.

---

## No dia a dia: como atualizar o site

1. Receba o novo `index.html` (ex.: `site-vendas-v2.3.0-index.html`), renomeie para `index.html` e substitua o da pasta do repositório.
2. GitHub Desktop: resumo `Site v2.3.0` > **Commit to main** > **Push origin**.
3. Pronto: o site se publica sozinho. Confira a versão no rodapé.

**Voltar uma versão:** no GitHub Desktop, aba **History**, clique com o botão direito no commit com problema > **Revert changes in commit** > **Push origin**.

**Observação:** a publicação não apaga arquivos do servidor. Se você remover um arquivo do repositório e quiser tirá-lo do ar, apague também pelo File Manager do CyberPanel.
