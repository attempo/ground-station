# Fork attempo/ground-station — modo REFERÊNCIA (Opção A)

**Decisão arquitetural:** 2026-04-25 — adotada **Opção A** da análise técnica em `attempo/clima-brasil/docs/integracao/ground-station/ANALISE_TECNICA.md`.

---

## Resumo

Este fork **não será integrado** com `attempo/thanatos-demo-platform` nem com `attempo/clima-brasil`. Fica como **estudo / referência**.

**Por quê:**
- Ground Station é plataforma HAM radio + SDR + tracking orbital — domínio técnico **diferente** de fogo (Thanatos) e clima/saúde (clima-brasil).
- Licença GPL-3.0 viral: importar código aqui contaminaria os outros projetos.
- Hardware-bound (precisa SDR USB físico).
- Stack frontend (React 19 puro JS + MUI Joy + ReactFlow + Leaflet) acopla aos componentes HAM-específicos (waterfall, DSP, hardware SDR).

**Portal único da attempo:** será desenvolvido no `thanatos-demo-platform/frontend` (Next.js 16 + Tailwind + TypeScript) consumindo:
- Thanatos backend `/api/v1/*` (focos de fogo, hotspots, alerts)
- clima-brasil API `/api/*` (clima, saúde, FA, dossiê) — cliente TS pronto em `clima-brasil/docs/integracao/typescript/`

---

## O que pode ser feito aqui (sem violar Opção A)

✅ **Estudar** padrões de Socket.IO, ReactFlow para topologia, layout MUI Joy
✅ **Rodar localmente** para entender funcionamento (sem SDR mostra mapa orbital + lista de satélites)
✅ **Sincronizar com upstream** (`git fetch upstream && git merge upstream/main`)
✅ **Customizar** para uso pessoal (continua GPL-3.0)

❌ **Não copiar** arquivos para Thanatos ou clima-brasil (contagia GPL)
❌ **Não fazer merge** com os outros 2 projetos
❌ **Não tornar** este fork "o portal" da attempo

---

## Como rodar (referência rápida)

```bash
cd ground-station

# Sem SDR (só UI + cálculos orbitais):
cd frontend && npm install && npm run dev          # http://localhost:5173
cd backend && pip install -r requirements.txt && python app.py    # http://localhost:8000

# Com SDR (requer Docker + dispositivo USB):
docker build -t ground-station .
docker run --rm -p 8000:8000 -p 5173:5173 --device /dev/bus/usb ground-station
```

---

## Sincronização com upstream

```bash
git fetch upstream
git merge upstream/main          # ou rebase
git push origin main
```

Remotes configurados:
- `origin` → `attempo/ground-station`
- `upstream` → `sgoudelis/ground-station`

---

## Quando reabrir esta decisão

Reabrir só se:
1. **Decidir capturar telemetria de satélites de fogo via SDR** (custo: USRP + antena + downconverter — adiar Q3+).
2. **Surgir caso de uso HAM/SDR claro** dentro do produto attempo.

Caso contrário, este fork fica como referência permanente.

— Beto, 2026-04-25
