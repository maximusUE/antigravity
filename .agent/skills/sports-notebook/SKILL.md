# Skill: Sports Notebook Builder

## Descripcion
Genera cuadernos NotebookLM especializados en eventos deportivos:
futbol (Premier League, La Liga, Champions League, Copa America, Mundial, MLS),
con datos en vivo, analisis post-partido, previews y estadisticas de jugadores.
Siempre responde en espanol.

## Cuando usar este skill
- Cuando el usuario pida analizar un partido especifico
- Cuando se pida un preview de jornada o competicion
- Cuando se necesiten estadisticas de un equipo o jugador
- Cuando se pida contenido deportivo para redes sociales o blog
- Cuando se pregunte sobre resultados, tabla de posiciones o clasificacion
- Cuando haya un partido en vivo y se quiera seguimiento en tiempo real

## Ligas soportadas (IDs API-Football)
- Premier League: ID 39 (Inglaterra)
- La Liga: ID 140 (Espana)
- Champions League: ID 2 (Europa)
- Serie A: ID 71 (Italia)
- Ligue 1: ID 61 (Francia)
- Bundesliga: ID 78 (Alemania)
- MLS: ID 253 (USA)
- Copa America: ID 9
- Mundial FIFA: ID 1
- Liga MX: ID 262 (Mexico)

## Herramientas MCP para usar

### Datos deportivos primarios (soccer-mcp)
Siempre usar como fuente principal. Requiere RAPID_API_KEY_FOOTBALL.

**Datos de liga:**
- `get_standings(league_id, season)` -> tabla de posiciones
- `get_league_fixtures(league_id, season)` -> todos los partidos de la liga
- `get_league_schedule_by_date(league_name, date, season)` -> agenda del dia
- `get_league_info(league_name)` -> info de la liga

**Datos de equipo:**
- `get_team_fixtures(team_name, type, limit)` -> partidos pasados/futuros
- `get_team_fixtures_by_date_range(team_name, from_date, to_date)` -> rango de fechas
- `get_team_info(team_name)` -> info basica del equipo

**Datos de partido:**
- `get_fixture_statistics(fixture_id)` -> stats detalladas del partido
- `get_fixture_events(fixture_id)` -> goles, tarjetas, cambios
- `get_multiple_fixtures_stats(fixture_ids)` -> stats de varios partidos

**Datos en vivo:**
- `get_live_match_for_team(team_name)` -> partido en vivo del equipo
- `get_live_stats_for_team(team_name)` -> stats en tiempo real
- `get_live_match_timeline(team_name)` -> timeline de eventos en vivo

**Datos de jugador:**
- `get_player_id(player_name)` -> buscar jugador
- `get_player_profile(player_name)` -> perfil completo
- `get_player_statistics(player_id, seasons, league_name)` -> stats por temporada

### Noticias del partido (google-news-trends)
- `get_news_by_keyword(keyword)` -> noticias del partido/equipo
- `get_news_by_topic(topic="SPORTS")` -> noticias deportivas generales
- `get_trending_keywords(geo)` -> si el partido es trending

### Reaccion en redes (twitter-mcp)
- `search_tweets(query)` -> reaccion de fans al partido
- `get_trends()` -> si el equipo/partido esta trending en Twitter

### Video y multimedia (youtube-transcript)
- `get_transcript(url)` -> transcript de rueda de prensa, highlights, analisis

### NotebookLM (destino final)
- `notebooklm-ingest` -> crear notebook con todas las fuentes
- `notebooklm` -> query final para generar el contenido estructurado

## Flujo: Analisis post-partido

1. `get_fixture_events(fixture_id)` -> goles, tarjetas, cambios
2. `get_fixture_statistics(fixture_id)` -> posesion, tiros, pases
3. `get_news_by_keyword("[equipo A] vs [equipo B]")` -> cobertura de prensa
4. `search_tweets("[equipo A] [equipo B]")` -> reaccion de fans
5. `get_transcript(url)` -> highlights o rueda de prensa en YouTube
6. `notebooklm-ingest` -> crear notebook con todo
7. `notebooklm` -> query: "Genera analisis completo del partido con citas"

## Flujo: Preview de jornada

1. `get_league_schedule_by_date(league_name, date)` -> partidos del fin de semana
2. `get_standings(league_id, season)` -> contexto de tabla
3. `get_team_fixtures(team_name, type="past", limit=5)` -> forma reciente de cada equipo
4. `get_news_by_topic(topic="SPORTS")` -> noticias previas
5. `notebooklm-ingest` -> notebook con el contexto
6. `notebooklm` -> query: "Genera preview con predicciones y datos"

## Flujo: Estadisticas de jugador

1. `get_player_id(player_name)` -> obtener ID
2. `get_player_statistics(player_id, seasons, league_name)` -> stats completas
3. `get_news_by_keyword(player_name)` -> noticias recientes
4. `notebooklm-ingest` + `notebooklm` -> analisis con citas

## Reglas de calidad
- SIEMPRE usar soccer-mcp como fuente primaria de datos oficiales
- Complementar con minimo 2 fuentes adicionales (noticias, Twitter o YouTube)
- Todos los outputs en espanol
- Incluir estadisticas numericas concretas (goles, asistencias, posesion, etc.)
- Bullets listos para publicar en Instagram, Twitter/X o blog
- Para partidos en vivo: actualizar cada 5 minutos con get_live_stats_for_team

## Output esperado para post-partido
1. **Resultado final** con goleadores y minutos
2. **Stats clave** (posesion, tiros a puerta, corners, tarjetas)
3. **Momento decisivo** del partido
4. **Actuacion destacada** del jugador del partido
5. **5 bullets** listos para redes sociales
6. **Link al cuaderno** en NotebookLM

## Ejemplos de comandos
- "Analiza el ultimo partido del Real Madrid"
- "Dame el preview de la Premier League este fin de semana"
- "Estadisticas de Erling Haaland esta temporada en la Premier"
- "Esta jugando el Liverpool ahora mismo?"
- "Tabla de posiciones de la Champions League"
- "Crea contenido del Manchester City vs Arsenal para mis redes"
