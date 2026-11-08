# Site do Processo Vivo (www.processovivo.com.br)

Página de vendas estática. Versão no ar: **v1.4.4** (08/11/2026), com o chat do Evo CRM. A versão aparece nos comentários do topo do `index.html`.

O sistema (app.processovivo.com.br) fica em outro repositório: `webmedula/processovivo`.

## Como funciona a publicação

Cada alteração enviada para a branch `main` publica o site sozinha, pelo GitHub Actions (`.github/workflows/deploy.yml`), que copia os arquivos para o `public_html` do servidor do CyberPanel por SSH. A publicação não apaga nada do servidor.

Veja o passo a passo em `COMO-ATUALIZAR.md`.

## O que fica aqui

| Arquivo | Para que serve |
|---|---|
| `index.html` | a página (planos e preços vêm do app, em `GET /v1/planos`) |
| `favicon.svg`, `favicon-32.png` | ícone da aba |
| `apple-touch-icon.png` | ícone ao salvar na tela do celular |
| `og-image.png` | imagem do link compartilhado |
| `img/processovivo-logo.svg`, `img/processovivo-logo-escuro.svg`, `img/processovivo-simbolo.svg` | logos oficiais (opcionais para o site) |
| `robots.txt`, `sitemap.xml` | indexação no Google |
| `LEIA-ME.md` | notas da versão (não vai para o servidor) |

As páginas de Termos (`termos.html`) e Privacidade (`privacidade.html`) continuam só no servidor por enquanto e não são tocadas pela publicação.

## Ajustes rápidos no `index.html`

- Dias de teste: bloco CONFIG no fim do arquivo, `DIAS_TESTE`.
- CNPJ no rodapé: procure por `CNPJ:`.
- Preços e planos: no /admin do app (o site lê sozinho).
