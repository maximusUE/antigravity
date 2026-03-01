# Skill: NotebookLM Content Builder

## Descripcion
Construye cuadernos de NotebookLM centralizados en creacion de contenido,
noticias de relevancia mundial y eventos deportivos.
Siempre responde en espanol.

## Cuando usar este skill
- Cuando el usuario pida crear un cuaderno de noticias o contenido
- Cuando se necesite investigar un tema trending a nivel mundial
- Cuando se necesite un resumen estructurado de un evento deportivo
- Cuando se pida analizar contenido de YouTube, Twitter o RSS para publicar
- Cuando se pida investigar relevancia de un tema a nivel global

## Herramientas MCP disponibles

### Noticias y Trending Mundial
- `google-news-trends`: Buscar noticias por keyword/topic/ubicacion y trending de Google
  - Tools: `get_news_by_keyword`, `get_top_news`, `get_trending_keywords`, `get_news_by_topic`, `get_news_by_location`
- `rss-news-agent`: Descubrir y resumir feeds RSS de cualquier web
  - Tools: `fetch_rss_feed`, `search_news`, `get_top_news_by_category`, `fetch_webpage`
- `mcp-rss`: Monitor multi-feed RSS en paralelo con cache
  - Tools: `fetch_rss_feed`, `fetch_multiple_feeds`, `monitor_feed_updates`, `search_content`
- `the-news-api`: Noticias filtradas avanzadas via TheNewsAPI
  - Tools: busqueda por categoria, pais, fecha, relevancia

### Deportes en Vivo
- `soccer-mcp`: Datos de futbol via API-Football (requiere RAPID_API_KEY_FOOTBALL)
  - Tools: `get_live_match_for_team`, `get_fixture_events`, `get_fixture_statistics`,
    `get_standings`, `get_player_statistics`, `get_team_fixtures`, `get_league_fixtures`,
    `get_league_schedule_by_date`, `get_live_match_timeline`, `get_live_stats_for_team`

### YouTube y Video
- `youtube-transcript`: Extraer transcript de videos por URL (sin API key)
  - Tools: `get_transcript(url, lang)`
- `youtube-full`: Buscar videos, trending por region, stats de canal (requiere YOUTUBE_API_KEY)
  - Tools: busqueda avanzada, trending, comentarios, transcripts, stats

### Twitter / X
- `twitter-mcp`: Trending topics, busqueda por hashtag, timelines (requiere TWITTER_API_KEY)
  - Tools: `get_trends`, `search_tweets`, `get_timeline`, `get_list_tweets`

### NotebookLM
- `notebooklm`: Crear y consultar cuadernos con citas
  - Tools: crear notebook, hacer queries, gestionar fuentes
- `notebooklm-ingest`: Ingestar URLs, PDFs, YouTube, Markdown como fuentes
  - Tools: ingest de multiples tipos de contenido

## Flujo estandar para cuaderno de evento deportivo

1. Usar `soccer-mcp` -> obtener datos del partido (eventos, stats, resultado)
2. Usar `google-news-trends` -> buscar noticias del partido con `get_news_by_keyword`
3. Usar `youtube-transcript` -> transcripts de highlights relevantes en YouTube
4. Usar `twitter-mcp` -> `get_trends` + `search_tweets` sobre el evento
5. Usar `notebooklm-ingest` -> crear notebook con todas las fuentes recopiladas
6. Usar `notebooklm` -> hacer query estructurada con citas para output final

## Flujo estandar para cuaderno de noticias trending

1. Usar `google-news-trends` -> `get_trending_keywords(geo="US")` y `get_top_news()`
2. Usar `rss-news-agent` -> buscar articulos extendidos por keyword
3. Usar `youtube-transcript` -> si hay cobertura relevante en YouTube
4. Usar `twitter-mcp` -> reaccion en redes al tema
5. Usar `notebooklm-ingest` -> armar notebook con todas las fuentes
6. Usar `notebooklm` -> generar resumen con citas listo para publicar

## Reglas de calidad
- Siempre usar minimo 3 fuentes antes de crear el cuaderno en NotebookLM
- Siempre incluir fuentes de noticias + redes sociales + video cuando sea posible
- El output final debe estar en espanol
- Incluir titulo del cuaderno, fuentes usadas, resumen ejecutivo y bullets para publicar
- Los bullets deben estar listos para copiar y pegar en redes sociales o blog

## Output esperado
Siempre devolver:
1. **Titulo del cuaderno** creado en NotebookLM
2. **Fuentes ingresadas** (URLs, videos, feeds usados)
3. **Resumen ejecutivo** en espanol con citas
4. **Puntos clave** (5-7 bullets) listos para publicar en redes o blog
5. **Link al cuaderno** en NotebookLM si esta disponible

## Ejemplos de uso
- "Crea un cuaderno sobre las noticias mas importantes del dia"
- "Que esta trending en Google hoy en Estados Unidos?"
- "Analiza el partido del Real Madrid del fin de semana"
- "Crea contenido sobre la Copa America para mis redes sociales"
- "Investiga el tema [X] y crea un cuaderno con las mejores fuentes"
