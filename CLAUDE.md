# Site Guardian Tech Consultoria

Site público (HTML + CSS + JS puro, sem build) publicado como Worker com assets no Cloudflare.
Base: padrão do repositório Portal (kleitonmetal-design/Portal), exceto o Cloudflare Access:
este site é público e NÃO usa Access.

## Fluxo
1. Editar e testar visualmente (Playwright ou HTML local).
2. Resumir a mudança (o que muda, arquivos, por quê) e aguardar aprovação explícita.
3. Commit na branch de trabalho, prefixo `[claude]`, push e PR.
4. Ativar os hooks uma vez por clone: `git config core.hooksPath .githooks`.

## Segurança
- Workflow `.github/workflows/security.yml` (TruffleHog + Semgrep) em todo push e PR.
- Cabeçalhos em `_headers` (HSTS, CSP, nosniff, frame-ancestors).
- Nunca commitar segredos. Chaves só no Cloudflare/GitHub Secrets.
- Cloudflare da zona: SSL estrito, HTTPS obrigatório, TLS 1.2+, WAF gerenciado, DDoS.

## Deploy no Cloudflare (estado atual)
- Worker `guardiantech-consultoria` ligado ao domínio `guardiantechit.com.br`.
- A primeira publicação foi feita pela API, com os arquivos embutidos no Worker e os cabeçalhos de `_headers` no código.
- Para deploy automático a cada push na `main` (igual ao Portal): Cloudflare > Workers > `guardiantech-consultoria` > Settings > Builds > conectar este repositório. O `wrangler.jsonc` já está pronto.
- HSTS ativo na zona (1 ano, sem subdomínios). DNSSEC ainda desligado: exige registro DS no registro.br.
