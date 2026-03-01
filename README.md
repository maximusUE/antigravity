# AntiGravity

Agente AI especializado en creacion de contenido, noticias mundiales y eventos deportivos.  
Utiliza MCP servers + NotebookLM para construir cuadernos centralizados y listos para publicar.

## Estructura

```
.agent/
├── mcp_config.json              # Configuracion de todos los MCP servers
└── skills/
    ├── notebooklm-content/      # Skill: noticias y contenido mundial
    │   └── SKILL.md
    └── sports-notebook/         # Skill: eventos deportivos y futbol
        └── SKILL.md
```

## Skills disponibles

### notebooklm-content
Construye cuadernos NotebookLM sobre noticias trending, temas globales y contenido para publicar.  
Fuentes: Google News, RSS feeds, TheNewsAPI, YouTube, Twitter/X.

### sports-notebook
Analisis post-partido, previews de jornada, stats de jugadores y contenido deportivo.  
Ligas: Premier League, La Liga, Champions League, Serie A, MLS, Copa America, Mundial.

## MCP Servers incluidos

| Server | Descripcion | API Key |
|--------|-------------|----------|
| `google-news-trends` | Google News + Trends por keyword/topic/location | No |
| `rss-news-agent` | RSS feeds de cualquier web + resumen con IA | No |
| `mcp-rss` | Monitor multi-feed RSS en paralelo con cache | No |
| `the-news-api` | TheNewsAPI con filtros avanzados | Si (gratis) |
| `soccer-mcp` | API-Football: fixtures live, stats, jugadores | Si (gratis) |
| `youtube-transcript` | Transcripts de YouTube por URL | No |
| `youtube-full` | Busqueda, trending, stats de canal | Si (gratis) |
| `twitter-mcp` | Trending topics, hashtags, timelines | Si (gratis) |
| `notebooklm` | Crear y consultar cuadernos con citas | Cuenta Google |
| `notebooklm-ingest` | Ingestar URLs, PDFs, YouTube, Markdown | Cuenta Google |

## Setup en servidor (EasyPanel / Docker)

```bash
# 1. Instalar uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# 2. Clonar soccer-mcp (unico que requiere clonar)
git clone https://github.com/obinopaul/soccer-mcp-server.git /opt/mcp-servers/soccer-mcp-server
cd /opt/mcp-servers/soccer-mcp-server && pip install -r requirements.txt

# 3. Los servers npx/uvx se instalan automaticamente al primer uso
```

## Variables de entorno requeridas

```env
RAPID_API_KEY_FOOTBALL=    # rapidapi.com/api-sports/api/api-football (100 req/dia gratis)
THE_NEWS_API_KEY=          # thenewsapi.com (100 req/dia gratis)
YOUTUBE_API_KEY=           # console.cloud.google.com (10k req/dia gratis)
TWITTER_API_KEY=           # developer.twitter.com (Basic tier gratis)
TWITTER_API_SECRET=
TWITTER_ACCESS_TOKEN=
TWITTER_ACCESS_SECRET=
```

## Flujo de uso

```
Trigger (cron / evento / manual)
  ↓
[soccer-mcp] + [google-news-trends] + [youtube-transcript] + [twitter-mcp]
  ↓
[notebooklm-ingest] → Crear notebook con todas las fuentes
  ↓
[notebooklm] → Query con citas → Output estructurado
  ↓
Contenido listo para publicar (redes, blog, n8n)
```

## Ejemplo de uso

> "Crea un cuaderno post-partido del Manchester City vs Arsenal"

1. Obtiene stats y eventos del partido via `soccer-mcp`
2. Busca cobertura de prensa via `google-news-trends`
3. Extrae transcript de highlights en YouTube
4. Revisa reaccion en Twitter
5. Crea notebook en NotebookLM con todas las fuentes
6. Genera analisis con citas listo para publicar en espanol
