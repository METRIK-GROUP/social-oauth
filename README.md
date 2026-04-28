# social-oauth

Hub estatico de fluxos OAuth para plataformas sociais do METRIK. Hospedado em GitHub Pages.

URL producao: https://metrik-group.github.io/social-oauth/

## Plataformas

| Plataforma | Auth | Callback | Status |
|---|---|---|---|
| TikTok | [/tiktok/auth/](./tiktok/auth/) | [/tiktok/callback/](./tiktok/callback/) | ativo |

## Por que estatico

OAuth aqui so precisa de:
1. Pagina que monta a URL de autorizacao (com state CSRF) e redireciona
2. Pagina que recebe o `code` no callback e mostra para o operador

A troca de `code` por `refresh_token` (que usa `client_secret`) acontece em GitHub Actions, nunca no front. Por isso nao precisa de backend.

## Adicionar nova plataforma

1. Criar pasta `<plataforma>/auth/` e `<plataforma>/callback/`
2. Adaptar `tiktok/auth/index.html` (trocar CLIENT_KEY, SCOPES, AUTH_URL)
3. Adaptar `tiktok/callback/index.html` (trocar REPO/WORKFLOW para o workflow correspondente)
4. Registrar o novo redirect URI no painel de developer da plataforma
5. Adicionar entrada no `index.html` raiz

## Seguranca

- `client_key` / `client_id` sao publicos (ok no HTML)
- `client_secret` NUNCA aparece aqui — fica em GitHub Secrets do repo de execucao
- State CSRF gerado com `crypto.getRandomValues` e validado no callback
- `noindex,nofollow` em todas as paginas

## TikTok

- App ID painel: https://developers.tiktok.com/apps/
- Repo de execucao: [METRIK-GROUP/social-scheduler](https://github.com/METRIK-GROUP/social-scheduler)
- Workflow OAuth-finish: `tiktok-oauth-finish.yml`
- Refresh token: 365 dias de validade — reautorizar 1x/ano
