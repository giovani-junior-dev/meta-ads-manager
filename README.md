# meta-ads-manager

Playbook de gestão de campanhas **Meta Ads** (Facebook + Instagram) para o Claude Code: estrutura, objetivos, públicos, Advantage+, criativos, Pixel/CAPI, budget e o **fluxo real de criação** no Gerenciador de Anúncios.

## Diferencial

Além do framework conceitual, traz as **armadilhas reais** que travam na prática e quase nunca aparecem em guias:

- **Erro `#1815199`** (identidade do Instagram) — diagnóstico + correção.
- **Bug do combobox de identidade** que não troca → **fix por duplicação do anúncio**.
- **DESTINATION_MISMATCH** (URL de exibição ≠ destino com redirect).
- Pipeline de **criativos** (Replicate gpt-image-2 + Pillow + resvg), UTMs no Rastreamento, AEM, aprimoramentos de IA OFF.

## Como dispara

Mencione "Meta Ads", "Facebook/Instagram Ads", "Meta Pixel", "CAPI", "Advantage+", "lookalike", "subir campanha", "criativo pra anúncio" — ou peça para criar/otimizar/auditar campanha Meta.

## Fontes

- Base conceitual: [`kostja94/marketing-skills` → meta-ads v1.1.1](https://github.com/kostja94/marketing-skills) (estrutura, Advantage+, learning phase, checklist).
- Fluxo prático + armadilhas: operação real **MegaAPI** (campanha de Tráfego montada do zero, 2026).

## Relacionadas

`marketing-expert` (copy), `google-ads-manager-2026` (Google), `svg-logo-designer` / `social-image-studio` (arte).
