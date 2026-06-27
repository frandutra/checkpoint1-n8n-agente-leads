# Checkpoint 1 — Agente Base y Motor de Razonamiento (n8n)

**Autor:** Francisco Dutra
**Entregable:** `checkpoint1_francisco_dutra.json` (flujo exportado de n8n, listo para importar)

## Descripción del flujo

Motor de razonamiento agéntico construido en n8n con un **AI Agent en modo Tools Agent**
que actúa como **Asistente de Calificación de Leads**.

### Arquitectura

```
Chat Trigger ─main─▶ AI Agent (Tools Agent) ─main─▶ Reporte Observabilidad (Gmail)
                          ▲              ▲
              ai_languageModel        ai_tool
                          │              │
            Google Gemini Chat Model   Registrar Lead en Google Sheets
```

### Componentes

| Componente | Nodo | Detalle |
|---|---|---|
| Disparador | `Chat Trigger` | Captura el mensaje desestructurado del usuario |
| Cerebro | `AI Agent` (Tools Agent, v3.1) | `promptType: auto`, conectado al modelo |
| Modelo | `Google Gemini Chat Model` | Conexión nativa `ai_languageModel` |
| Guardrail | `maxIterations: 8` | Límite estricto de iteraciones (rango 5–10) |
| System Prompt | modular | Rol → Ámbito → Objetivo → Reglas → Restricciones → Escalamiento. Excluye lenguaje inclusivo |
| Herramienta | `Registrar Lead en Google Sheets` | Conectada lateralmente (`ai_tool`), con descripción semántica extensa |
| Observabilidad | `Reporte Observabilidad (Gmail)` | Envía el `output` + `intermediateSteps` (Execution Log) como reporte de supervisión |

## Cómo importar

1. n8n → **Import from File** → seleccionar `checkpoint1_francisco_dutra.json`
2. Reasignar credenciales (Google Gemini, Google Sheets, Gmail) en cada nodo
3. Ajustar el `documentId` de Google Sheets a una planilla con columnas:
   `Nombre, Email, Empresa, Interes, Score`
4. **Execute Workflow** y enviar un prompt de prueba al agente

> Nota: el JSON no contiene secretos — n8n solo exporta IDs y nombres de credenciales, no las API keys.
