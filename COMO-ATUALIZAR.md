# Site do Processo Vivo no GitHub — passo a passo (kit v1.0.4)

Objetivo: o site www.processovivo.com.br passa a morar num repositório do GitHub. Para atualizar, você sobe o arquivo novo no GitHub (pelo navegador ou pelo GitHub Desktop) e o site se publica sozinho no servidor, em cerca de 1 minuto.

---

## Parte 1 — Colocar o site atual (v1.3.0) no repositório (uma vez só)

1. No seu computador, descompacte o `processovivo-site-v1.3.0.zip`.
2. No GitHub, no repositório `webmedula/processovivo-site`, clique em **Add file > Upload files**.
3. Arraste os 8 arquivos do pacote: `index.html`, `favicon.svg`, `favicon-32.png`, `apple-touch-icon.png`, `og-image.png`, `robots.txt`, `sitemap.xml` e `LEIA-ME.md`.
4. Em "Commit changes", escreva `Site v1.3.0 (versão no ar)` e clique em **Commit changes**.

Os Termos e a Privacidade ficam só no servidor e não são apagados pela publicação.

## Parte 2 — Ligar a publicação automática (uma vez só)

### 2.1 Criar um acesso só para o site (não o root)

O GitHub entra no servidor com o **usuário do próprio site** (o mesmo que o CyberPanel criou para o processovivo.com.br). Esse usuário só mexe nos arquivos do site: não enxerga os outros sites nem administra a VPS. A senha e a chave do root nunca vão para o GitHub.

Rode uma vez, como root, no terminal do servidor (Hostinger: hPanel > VPS > Gerenciar > **Terminal do navegador**):

```bash
SITEUSER=$(stat -c %U /home/processovivo.com.br/public_html)
SITEHOME=$(getent passwd "$SITEUSER" | cut -d: -f6)
echo "Usuário do site: $SITEUSER  |  pasta: $SITEHOME"
usermod -s /bin/bash "$SITEUSER"
mkdir -p "$SITEHOME/.ssh"
ssh-keygen -t ed25519 -C "github-deploy-site" -f /root/github_deploy_site -N ""
echo "no-port-forwarding,no-agent-forwarding,no-X11-forwarding,no-pty $(cat /root/github_deploy_site.pub)" >> "$SITEHOME/.ssh/authorized_keys"
chown -R "$SITEUSER:$SITEUSER" "$SITEHOME/.ssh"
chmod 700 "$SITEHOME/.ssh"
chmod 600 "$SITEHOME/.ssh/authorized_keys"
base64 -w0 /root/github_deploy_site; echo
```

- A primeira linha que aparece mostra o **usuário do site** (ex.: `proce1234`). Anote.
- A última linha é a **chave privada em base64**: um texto comprido, numa linha só, sem espaços (normalmente começa com `LS0tLS1CRUdJTi`). Copie a linha inteira. Assim a cópia pelo terminal do navegador não quebra a chave. Não envie essa chave para ninguém.

Depois de cadastrar a chave no GitHub (2.2), apague a cópia do servidor:

```bash
rm -f /root/github_deploy_site /root/github_deploy_site.pub
```

### 2.2 Cadastrar os segredos no GitHub

No site do GitHub, no repositório `processovivo-site`: **Settings > Secrets and variables > Actions > New repository secret**. Crie um por um:

| Nome | Valor |
|---|---|
| `SSH_HOST` | `145.223.92.68` |
| `SSH_PORT` | `22` (ou a porta do seu SSH) |
| `SSH_USER` | o usuário do site anotado no 2.1 (ex.: `proce1234`) — **não use root** |
| `SSH_KEY` | a chave em base64 copiada no 2.1 (uma linha só) |
| `SITE_DIR` | `/home/processovivo.com.br/public_html` |

Não crie o `SITE_OWNER`: como o envio já é feito pelo dono dos arquivos, ele não é necessário.

**Para cortar o acesso a qualquer momento:** apague a linha `github-deploy-site` do arquivo `/home/processovivo.com.br/.ssh/authorized_keys` (ou o arquivo inteiro). O GitHub perde o acesso na hora.

### 2.3 Testar

No GitHub: aba **Actions > Publicar site > Run workflow**. Em cerca de 1 minuto deve aparecer o sinal verde. Abra www.processovivo.com.br, force a atualização (Ctrl+F5) e confira se continua tudo igual (é a mesma v1.3.0 que já está no ar).

Se der vermelho, clique na execução para ver o erro. Os mais comuns:
- `A chave em SSH_KEY está incompleta` ou `error in libcrypto`: a chave foi colada quebrada. Gere a versão em base64 (2.1) e edite o segredo `SSH_KEY`.
- `Permission denied (publickey)`: a chave pública não entrou no `authorized_keys` do usuário do site, ou o `SSH_USER` está errado.
- `Connection timed out`: o firewall do servidor (CyberPanel > Security > Firewall) está bloqueando a porta do SSH para fora.

---

## No dia a dia: como atualizar o site

1. Receba o novo `index.html` (ex.: `site-vendas-v1.4.0-index.html`) e renomeie para `index.html`.
2. No GitHub: **Add file > Upload files**, arraste o arquivo, resumo `Site v1.4.0` > **Commit changes**. (Pelo GitHub Desktop também serve: commit e **Push origin**.)
3. Pronto: o site se publica sozinho. Abra o site com Ctrl+F5 para conferir.

**Voltar uma versão:** no GitHub, abra o histórico do arquivo (**History**), baixe a versão anterior e suba de novo com Upload files. No GitHub Desktop: aba **History**, botão direito no commit com problema > **Revert changes in commit** > **Push origin**.

**Observação:** a publicação não apaga arquivos do servidor. Se você remover um arquivo do repositório e quiser tirá-lo do ar, apague também pelo File Manager do CyberPanel.
