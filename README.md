# planandgo-site

Site público do Plan&Go: a página inicial e as páginas que a Google Play exige (Política de Privacidade, Termos de Uso e Excluir conta). É HTML e CSS, com um script curto só no `404.html`: não tem build, não tem cookies nem análise de uso e não carrega nada de fora.

> **Este repositório é público.** Tudo o que for enviado para a `main` vai ao ar no GitHub Pages, inclusive este arquivo. Revise no navegador antes de enviar e nunca coloque chave, senha ou dado pessoal aqui.

## Arquivos

| Arquivo | O que é |
|---|---|
| `index.html` | Página inicial: o que o app já faz, papéis do grupo, "Em breve no Google Play" |
| `privacidade/index.html` | Política de Privacidade (LGPD e Data safety da Play), em `/privacidade` |
| `termos/index.html` | Termos de Uso, em `/termos` |
| `excluir-conta/index.html` | Como pedir a exclusão da conta, o que é apagado e o que fica, em `/excluir-conta` |
| `404.html` | Página de endereço inexistente (ajusta a base dos links sozinha, veja abaixo) |
| `styles.css` | Todo o visual, com as cores e as fontes do app |
| `fonts/` | Inter Tight, Space Mono e Fredoka, com as licenças OFL (`OFL-*.txt`) |
| `favicon.svg` | Ícone da aba do navegador |
| `robots.txt` | Libera os buscadores |
| `.nojekyll` | Faz o GitHub Pages publicar os arquivos como estão, sem processar |

## Ver no computador antes de publicar

As páginas legais ficam em pastas (`privacidade/index.html`) para os endereços saírem limpos (`/privacidade`). Por isso, abrir os arquivos direto no navegador não segue os links entre páginas. Na pasta do site (a que tem o `index.html`), rode:

```powershell
python -m http.server 8000 --bind 127.0.0.1
```

Depois abra `http://localhost:8000`. Confira também na largura de celular (380 px) pelas ferramentas do navegador.

## Publicar no GitHub Pages

1. Antes do primeiro commit neste computador, rode nesta pasta `git config user.name "planandgotrip"` e `git config user.email "planandgotrip@gmail.com"`, e confira com `git log -1 --format="%an <%ae>"` antes do `git push`. Sem isso, o commit sai com o nome e o e-mail pessoais configurados no computador, e eles ficam públicos para sempre no histórico. As mensagens de commit também ficam públicas: escreva-as sem número de tarefa nem caminho do repositório do app.
2. Antes do primeiro envio, confira que a limpeza do registro de uso da Central está ativa no banco: a migration `20261015170000` do app aplicada e o job do pg_cron rodando (a Política promete apagar esse registro em até 2 dias).
3. Envie os arquivos para a `main` do repositório `planandgotrip/planandgo-site`.
4. No GitHub, abra **Settings > Pages**.
5. Em **Build and deployment**, escolha **Source: Deploy from a branch**, **Branch: `main`** e a pasta **`/ (root)`**. Salve.
6. Em um ou dois minutos o site fica em `https://planandgotrip.github.io/planandgo-site/`, com as páginas em `.../privacidade/`, `.../termos/` e `.../excluir-conta/`.

Os endereços definitivos são os do domínio próprio (seção abaixo). O app já abre estes endereços, então o domínio precisa estar no ar, com HTTPS, antes de qualquer versão do app chegar a testadores e antes de preencher a Play Console:

- `https://planandgo.com.br/privacidade`
- `https://planandgo.com.br/termos`
- `https://planandgo.com.br/excluir-conta`

O GitHub Pages leva o endereço sem barra no fim (`/privacidade`) para a pasta (`/privacidade/`) sozinho. Com o domínio ligado, o site fica na raiz do domínio, e o nome do repositório não aparece no endereço.

Todos os links entre as páginas são relativos, então funcionam no endereço do GitHub e no domínio. A exceção é o `404.html`: o GitHub o mostra em qualquer caminho que não existe, inclusive em subpastas, e por isso um script de poucas linhas define a base dos links (`/planandgo-site/` no github.io e `/` no domínio próprio). Se o repositório mudar de nome, troque `/planandgo-site/` nesse script.

## Ligar o domínio planandgo.com.br

Depois de o conteúdo ser aprovado e o domínio comprado no Registro.br. Confira os IPs na documentação do GitHub antes de criar os registros: [Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).

1. **Verificar o domínio na organização** (impede que outro repositório use o domínio): no GitHub, na organização `planandgotrip`, abra **Settings > Pages > Add a domain**, crie no DNS o registro TXT que o GitHub mostrar e espere o domínio aparecer como verificado.
2. **No repositório**, em **Settings > Pages > Custom domain**, escreva `planandgo.com.br` e salve. O GitHub cria o arquivo `CNAME` na `main`: rode `git pull` antes do próximo envio e nunca use `git push --force`, que apagaria esse arquivo e tiraria o domínio do ar.
3. **DNS no Registro.br** (modo avançado, ou no provedor de DNS escolhido):
   - domínio sem `www` (`planandgo.com.br`), quatro registros **A**:
     `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`;
   - os mesmos em IPv6, quatro registros **AAAA**:
     `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`;
   - `www`, um registro **CNAME** para `planandgotrip.github.io`.
4. Espere o certificado ficar pronto e marque **Enforce HTTPS**.
5. Confira que `https://planandgo.com.br/privacidade`, `/termos` e `/excluir-conta` abrem com o cadeado. Só então coloque esses endereços na Play Console (Política de Privacidade e exclusão de conta) e no `Site URL` do painel do Supabase. O app já usa esses endereços; não há link a trocar nele.

O endereço antigo `planandgotrip.github.io/planandgo-site/` passa a redirecionar para o domínio.

## Ao mudar o app

A Política de Privacidade e a página de exclusão descrevem o comportamento do app. Quando o app passar a coletar um dado novo, usar outro fornecedor ou mudar o caminho de exclusão, atualize a página e a data no topo dela, e avise por e-mail se a mudança for relevante.

## Fontes

As fontes (Inter Tight, Space Mono e Fredoka) estão sob a SIL Open Font License 1.1, cujo texto está em `fonts/OFL-*.txt`. São servidas pelo próprio site, sem Google Fonts, em WOFF2 recortado só com o latim e alguns sinais (aspas, travessões, setas e o ⋯). Um caractere fora desse recorte aparece na fonte do sistema: para incluir outro, gere de novo os arquivos a partir das fontes originais com o `pyftsubset` do pacote fonttools.
