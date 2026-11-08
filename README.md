# Site do Processo Vivo (www.processovivo.com.br)

Página de vendas estática. Versão atual do site: **v2.2.0** (a versão aparece no rodapé e na tag `generator` do `index.html`).

O sistema (app.processovivo.com.br) fica em outro repositório: `webmedula/processovivo`.

## Como funciona a publicação

Cada alteração enviada para a branch `main` publica o site sozinha, pelo GitHub Actions (`.github/workflows/deploy.yml`), que copia os arquivos para o servidor do CyberPanel por SSH.

Veja o passo a passo em `COMO-ATUALIZAR.md`.

## O que fica aqui

- `index.html` — página de vendas (arquivo único: CSS, JS e ilustrações embutidos)
- `termos.html`, `privacidade.html` — documentos legais
- `img/` — logo, favicon, imagem do sistema (`carteira-claro.webp`), imagem de compartilhamento (`og.png`)
- `fontes/` — fontes usadas pelos termos e pela privacidade
