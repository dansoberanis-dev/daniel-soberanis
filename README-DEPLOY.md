# Publicar o portfólio — passo a passo

Site estático: um `index.html` + a pasta `assets/`. Não tem build, não tem dependência.
Funciona em qualquer hospedagem; este guia usa **GitHub → Vercel**, que é o caminho mais simples
e dá deploy automático a cada `git push`.

---

## 1. Testar localmente

```bash
cd portfolio-v2
python3 -m http.server 3001 --bind 0.0.0.0
# abrir http://localhost:3001
```

## 2. Gravar o endereço definitivo

Antes de publicar (ou assim que a Vercel der a URL), rode:

```bash
python3 deploy/preparar.py https://SEU-ENDERECO.vercel.app
```

Isso escreve a URL no `canonical`, no `og:url`, nas imagens de compartilhamento e nos dados
estruturados do Google, e gera `robots.txt` + `sitemap.xml`.
**Sem isso, o link compartilhado no WhatsApp e no Instagram aparece sem imagem.**

## 3. Enviar para o GitHub

Crie o repositório vazio no GitHub (sem README, sem .gitignore) e rode:

```bash
cd portfolio-v2
git init
git add .
git commit -m "Portfólio Daniel Soberanis — primeira versão"
git branch -M main
git remote add origin git@github.com:SEU-USUARIO/SEU-REPO.git
git push -u origin main
```

Se o `git push` pedir senha, use um token pessoal (GitHub → Settings → Developer settings →
Personal access tokens) ou configure a chave SSH.

## 4. Publicar na Vercel

**Pelo site (mais simples):**
1. Acesse vercel.com e entre com a conta do GitHub.
2. **Add New → Project**, escolha o repositório.
3. A Vercel detecta site estático sozinha: **Framework Preset = Other**, build vazio.
4. Clique em **Deploy**. Em menos de um minuto você recebe a URL.

**Pelo terminal:**
```bash
npm i -g vercel
vercel login          # abre o navegador para autenticar
vercel --prod         # publica
```

## 5. Depois de publicar

```bash
python3 deploy/preparar.py https://A-URL-QUE-A-VERCEL-DEU
git add . && git commit -m "Endereço definitivo no canonical e OG" && git push
```
A Vercel detecta o push e republica sozinha.

### Checklist final
- [ ] Abrir a URL no celular (4G, tela pequena) e no computador
- [ ] Colar o link no WhatsApp e conferir se aparece imagem, título e descrição
- [ ] Conferir o favicon na aba do navegador
- [ ] Cadastrar a URL no Google Search Console e enviar o `sitemap.xml`
- [ ] (Opcional) ativar o GA4: descomente o bloco no fim do `<head>` e troque `G-XXXXXXXXXXXX`
- [ ] (Opcional) domínio próprio: Vercel → Settings → Domains

---

## O que tem no pacote

```
index.html            site inteiro (HTML + CSS inline + JS inline)
assets/               imagens, ícones e favicon (2,9 MB)
scripts/              ferramentas usadas na construção (não vão para o site)
deploy/preparar.py    grava a URL definitiva e gera robots.txt/sitemap.xml
ANALISE-v2.md         histórico técnico de cada alteração
AUDITORIA-CONSULTOR-v2.md   diagnóstico de conversão
```

Os arquivos em `scripts/` não são usados pelo site — podem ficar no repositório ou ser removidos.
