# Site de vendas Processo Vivo — v1.4.3 (05/11/2026)

Versão para produção do site www.processovivo.com.br. O visual é o mesmo da v1.2.0 aprovada.

## O que vai no servidor

Suba todos estes arquivos para a raiz do site (`public_html` do domínio no CyberPanel):

| Arquivo | Para que serve |
|---|---|
| `index.html` | a página |
| `favicon.svg`, `favicon-32.png` | ícone da aba do navegador |
| `apple-touch-icon.png` | ícone quando alguém salva o site na tela do celular |
| `og-image.png` | imagem que aparece quando o link é compartilhado (WhatsApp, LinkedIn etc.) |
| `robots.txt`, `sitemap.xml` | para o Google achar e indexar o site |
| `img/processovivo-logo*.svg`, `img/processovivo-simbolo.svg` | os logos oficiais, para usar em outras páginas (não são obrigatórios para o site) |

Não apague as páginas de Termos e Privacidade que já existem no servidor.

## Passo a passo no CyberPanel

1. Faça uma cópia do `index.html` atual (por exemplo, renomeie para `index-antigo.html`).
2. Em **File Manager**, abra o `public_html` de processovivo.com.br.
3. Envie os arquivos do pacote (e os 3 logos para dentro da pasta `img` que já existe), substituindo o `index.html`.
4. Abra https://www.processovivo.com.br e force a atualização (Ctrl+F5).

## Conferências antes de divulgar

- [ ] **Planos aparecem com preço.** Se aparecer só o botão "Testar grátis", confira no Easypanel se `PROCESSOVIVO_SITE_URL` está exatamente como `https://www.processovivo.com.br` (sem barra no final) e reinicie o serviço.
- [ ] **Sem www também funciona.** Abra http://processovivo.com.br. O ideal é que redirecione para https://www.processovivo.com.br, porque os planos só carregam na origem com www.
- [ ] **Termos e Privacidade.** Os links do rodapé abrem `termos.html` e `privacidade.html`, que já estão no servidor.
- [ ] **Botões.** "Testar 7 dias grátis", "Assinar agora" e "Entrar" abrem o app.
- [ ] **Compartilhamento.** Cole o link no WhatsApp e veja se a imagem aparece. Se não aparecer, teste em https://developers.facebook.com/tools/debug/ e clique em "Extrair novamente".
- [ ] **Google.** No Google Search Console, adicione o domínio e envie `https://www.processovivo.com.br/sitemap.xml`.

## O que mudar no futuro

- **Dias de teste:** no fim do `index.html`, no bloco CONFIG, altere `DIAS_TESTE`. Todos os textos da página acompanham.
- **Preços e planos:** mude no /admin do app. O site lê os planos sozinho, com até 60 segundos de atraso por causa do cache.
- **CNPJ no rodapé:** procure por "CNPJ:" no `index.html`. O comentário mostra onde acrescentar.
- **Logo:** é o logo oficial (o mesmo do app), desenhado dentro do `index.html`. Não precisa de arquivo separado.
