# Site do Processo Vivo no GitHub — passo a passo (kit v1.0.2)

Objetivo: o site www.processovivo.com.br passa a morar num repositório do GitHub. Para atualizar, você sobe o arquivo novo no GitHub (pelo navegador ou pelo GitHub Desktop) e o site se publica sozinho no servidor, em cerca de 1 minuto.

---

## Parte 1 — Colocar o site atual (v1.3.0) no repositório (uma vez só)

1. No seu computador, descompacte o `processovivo-site-v1.3.0.zip`.
2. No GitHub, no repositório `webmedula/processovivo-site`, clique em **Add file > Upload files**.
3. Arraste os 8 arquivos do pacote: `index.html`, `favicon.svg`, `favicon-32.png`, `apple-touch-icon.png`, `og-image.png`, `robots.txt`, `sitemap.xml` e `LEIA-ME.md`.
4. Em "Commit changes", escreva `Site v1.3.0 (versão no ar)` e clique em **Commit changes**.

Os Termos e a Privacidade ficam só no servidor e não são apagados pela publicação.

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

No GitHub: aba **Actions > Publicar site > Run workflow**. Em cerca de 1 minuto deve aparecer o sinal verde. Abra www.processovivo.com.br, force a atualização (Ctrl+F5) e confira se continua tudo igual (é a mesma v1.3.0 que já está no ar).

Se der vermelho, clique na execução para ver o erro. Os mais comuns:
- `Permission denied (publickey)`: a chave pública não entrou no `authorized_keys` ou o `SSH_KEY` foi colado incompleto.
- `Connection timed out`: o firewall do servidor (CyberPanel > Security > Firewall) está bloqueando a porta do SSH para fora.

---

## No dia a dia: como atualizar o site

1. Receba o novo `index.html` (ex.: `site-vendas-v1.4.0-index.html`) e renomeie para `index.html`.
2. No GitHub: **Add file > Upload files**, arraste o arquivo, resumo `Site v1.4.0` > **Commit changes**. (Pelo GitHub Desktop também serve: commit e **Push origin**.)
3. Pronto: o site se publica sozinho. Abra o site com Ctrl+F5 para conferir.

**Voltar uma versão:** no GitHub, abra o histórico do arquivo (**History**), baixe a versão anterior e suba de novo com Upload files. No GitHub Desktop: aba **History**, botão direito no commit com problema > **Revert changes in commit** > **Push origin**.

**Observação:** a publicação não apaga arquivos do servidor. Se você remover um arquivo do repositório e quiser tirá-lo do ar, apague também pelo File Manager do CyberPanel.
