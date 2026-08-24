# OpenShift TSR Viewer

Visualizador web (HTML single-file) para relatórios JSON gerados pela ferramenta **Phased Gates** de análise de must-gather do OpenShift.

## Descrição

O `tsr-viewer.html` é um arquivo HTML autocontido (sem dependências externas) que permite carregar, visualizar, filtrar, anotar e exportar os resultados de análises Phased Gates de clusters OpenShift. Funciona 100% offline no navegador.

## Como Usar

1. Abra `tsr-viewer.html` em qualquer navegador moderno.
2. Clique no botão **Load JSON** (ícone de pasta) para carregar um arquivo `.pg.json` gerado pelo Phased Gates.
3. Navegue pelos checks usando a tabela principal e os filtros na barra lateral.

## Funcionalidades

### Visualização
- Tabela interativa com todos os checks do relatório
- Painel de detalhes com resultado completo, links, dicas e remediações
- Colunas configuráveis (number, name, status, tags, etc.)
- Colunas redimensionáveis com drag-and-drop
- Layout responsivo com sidebar redimensionável e painel de detalhes ajustável
- Tema dark (Red Hat branded)

### Filtros (Sidebar)
- **Session** — filtro por sessão (quando multi-sessão)
- **Check status** — `pass`, `fail`, `warning`, `info`, `NA`, `skip`, `skip_with_content`
- **Inner result status** — status internos como `limitation`, `support limitation`, etc.
- **Topic** — tópicos/seções do relatório (e.g., "ETCD", "Topology")
- **Subsection** — subseções hierárquicas
- **Tags** — tags dos checks
- **More** — filtros avançados:
  - Número do check (e.g. "4.8")
  - error_key, component, skip reason
  - Filtro por links (com/sem)
  - Somente checks com inner FAIL
  - Esconder SKIP e N/A
  - Somente marcados
  - Somente checks com notas

### Busca
- Busca global na toolbar (busca em nome, número, mensagens, tags)
- Busca dentro do painel de detalhes com navegação por resultados
- Suporte a operadores: aspas para frase exata, `-` para excluir

### Marcação e Notas
- Marcar/desmarcar checks individuais (checkbox na tabela)
- Adicionar notas por check (campo de texto persistente)
- Notas expandem em modal para edição detalhada
- Dados persistem no `localStorage` do navegador

### Exportação
- **JSON (marked)** — exporta checks marcados em formato JSON
- **HTML (marked)** — exporta checks marcados como relatório HTML
- **JSON (all)** — exporta todos os checks filtrados
- **HTML (all)** — exporta todos os checks filtrados como relatório HTML

### Comparação
- Carrega um segundo JSON para comparar com o atual
- Identifica checks que **pioraram**, **melhoraram**, foram **adicionados** ou **removidos**
- Indicadores visuais na tabela (barra lateral colorida)
- Filtro por tipo de mudança
- Exportação do diff (JSON e HTML)

### Atalhos de Teclado
- Pressione `?` para ver todos os atalhos disponíveis
- Navegação por setas na tabela
- Atalhos para filtros rápidos

### Sessão
- Estado da UI (filtros, layout, notas, marcações) persiste automaticamente via `localStorage`
- Botão "Forget session" limpa todos os dados salvos
- URL com hash atualiza para o check selecionado (permite compartilhar link direto)

## Formato de Entrada

O viewer aceita arquivos `.pg.json` gerados pelo Phased Gates. A estrutura esperada é:

```json
{
  "analysis_metadata": { ... },
  "rule_sets": {
    "ccx_rules_ocp": { ... },
    "phased_gates": {
      "must_gather": { ... },
      "skips": { ... }
    }
  },
  "system": {
    "metadata": {
      "cluster_id": "...",
      "cluster_name": "...",
      "cluster_version": "..."
    }
  }
}
```

O arquivo contém checks hierárquicos organizados em seções numeradas (1.x a 7.x), onde cada check possui `node_metadata`, `content`, `result` e `tags`.

## Requisitos

- Navegador moderno (Chrome, Firefox, Edge, Safari)
- Nenhuma dependência externa ou servidor necessário
- Funciona completamente offline

## API JavaScript (Avançado)

O viewer expõe `window.TSRViewer` com métodos utilitários:

- `ingest(data)` — carrega dados programaticamente
- `applyFilters()` — aplica filtros atuais
- `getChecks()` — retorna todos os checks
- `getFiltered()` — retorna checks filtrados
- `getMarked()` — retorna checks marcados
- `getNotes()` — retorna notas
- `buildExportJson()` / `buildTsrHtml()` — gera exportações
- `buildCompareJson()` / `buildCompareHtml()` — gera comparações
