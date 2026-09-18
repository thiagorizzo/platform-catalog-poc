# platform-catalog-poc

POC do fluxo de **solicitação e aprovação de role** descrito no
[ADR-005 (seção 3.6.1)](https://github.com/grupoprimo/platform/blob/feat/primo-identity/docs/adr/0005-primo-service-identity.md)
do grupo Primo.

Este repositório representa, em miniatura, o `platform-catalog` real:
o vínculo pessoa→role vive aqui, particionado por macrodomínio, com
aprovação via CODEOWNERS.

## Estrutura

```
access/
  investimento/roles.yaml   # CODEOWNERS: aprovador do macrodomínio "investimento"
  core/roles.yaml           # CODEOWNERS: aprovador do macrodomínio "core"
```

## Fluxo

1. **Solicitação** — um script (`request-role`, no repo da POC principal)
   simula o Software Template do Backstage: pede macrodomínio + role +
   justificativa, e abre um Pull Request adicionando a pessoa a
   `access/<domínio>/roles.yaml`.
2. **Aprovação** — o `CODEOWNERS` exige o owner do macrodomínio como revisor.
   O merge do PR é a aprovação.
3. **Efetivação** — após o merge, um script (`apply-role-binding`) lê este
   repositório e aplica a role na identidade da pessoa no Clerk
   (`publicMetadata`).
4. **Uso** — a role passa a compor o token de sessão da pessoa; o guardião do
   serviço-alvo autoriza (ou não) com base nela.

Ver o repositório principal da POC (`clerk-poc`) para os scripts e o restante
do fluxo (identidade de serviço, guardião Envoy, etc.).
