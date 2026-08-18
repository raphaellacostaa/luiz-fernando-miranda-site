# Luiz Fernando Miranda — site institucional

Site institucional do cientista político **Luiz Fernando Miranda**, professor de Ciência
Política da UFPA e vice-presidente da Transparência Brasil.

Página única, estática, bilíngue (PT/EN), sem dependências nem etapa de build.

**No ar:** https://luiz-fernando-miranda-site.vercel.app

---

## Arquivos

| Arquivo | Função |
|---|---|
| `index.html` | O site inteiro — HTML, CSS e JS em um arquivo só |
| `banner.webp` / `banner.jpg` | Imagem da hero, desktop (WebP com fallback JPG) |
| `banner-mobile.webp` / `.jpg` | Mesma imagem recortada para telas até 768px |
| `og-image.jpg` | Miniatura de compartilhamento (WhatsApp, LinkedIn, X) |
| `vercel.json` | Headers de segurança e cache |
| `llms.txt` | Versão estruturada do conteúdo para leitura por IAs |
| `robots.txt` / `sitemap.xml` | Indexação por buscadores |
| `.gitattributes` | Trava quebras de linha em LF — **ver aviso abaixo** |

## Rodar localmente

```bash
python -m http.server 5173
```

E abrir http://localhost:5173. Não há dependências para instalar.

## Publicar

O deploy é automático: todo push na branch `main` dispara a Vercel.

---

## ⚠️ Antes de editar o index.html

O `vercel.json` carrega uma Content-Security-Policy que autoriza os quatro blocos
`<script>` do `index.html` por **hash SHA-256** do conteúdo de cada um.

Consequência prática: **qualquer alteração dentro de um `<script>` invalida o hash
correspondente**, e o navegador passa a bloquear aquele script em produção. Quebram o
menu mobile, o seletor PT/EN e os links de WhatsApp e e-mail — sem nenhum erro visível
além de uma mensagem no console.

Se você mexer em qualquer `<script>`, recalcule os hashes e atualize o `vercel.json`:

```bash
python -c "
import re, hashlib, base64
html = open('index.html', encoding='utf-8', newline='').read()
for m in re.finditer(r'<script(?![^>]*\bsrc=)[^>]*>(.*?)</script>', html, re.S):
    h = hashlib.sha256(m.group(1).encode()).digest()
    print(\"'sha256-\" + base64.b64encode(h).decode() + \"'\")
"
```

Mexer só no HTML ou no CSS é seguro — não afeta os hashes.

### Não edite pela interface web do GitHub

O editor do GitHub salva o arquivo com quebras de linha CRLF. Como o hash é calculado
sobre os bytes exatos do script, CRLF invalida os quatro hashes de uma vez. O
`.gitattributes` corrige isso no `git push`, mas não em commits feitos pelo navegador.

Edite localmente e publique com `git push`.

---

## Domínio

O domínio aparece em 25 lugares, distribuídos entre `index.html` (canonical, Open Graph,
Twitter Card e JSON-LD), `llms.txt`, `robots.txt` e `sitemap.xml`. Ao migrar para domínio
próprio, troque em todos: um canonical apontando para o endereço antigo faz o Google
ignorar o novo.

## Contato do titular

E-mail e telefone não aparecem em texto na página, por decisão do titular. São montados
em tempo de execução pelo primeiro `<script>` do `index.html`.
