## Lista de Perguntas + Soluções

### `A. Informações Gerais das Músicas`

1. Quais são as 5 músicas mais reproduzidas em 2023?

```sql
SELECT   track_name,
         streams
FROM     spotify
WHERE    released_year = 2023
ORDER BY streams DESC
LIMIT    5;
```

**Resultado:**

| track_name                            | streams   |
| ------------------------------------- | --------- |
| Ella Baila Sola                       | 725980112 |
| Shakira: Bzrp Music Sessions, Vol. 53 | 721975598 |
| TQG                                   | 618990393 |
| La Bebe - Remix                       | 553634067 |
| Die For You - Remix                   | 518745108 |

<br>

2. Quantos artistas únicos contribuíram para o conjunto de dados?

```sql
SELECT COUNT(DISTINCT artist(s)_name) AS unique_artists_count
FROM   spotify;
```

**Resultado:**

| unique_artists_count |
| -------------------- |
| 595                  |

<br>

3. Qual é a distribuição das músicas ao longo dos anos de lançamento?

```sql
SELECT   CONCAT(FLOOR(released_year / 10) * 10, '-', FLOOR(released_year / 10) * 10 + 11) AS decade_range,
         COUNT(*) AS songs_count
FROM     spotify
GROUP BY 1
ORDER BY 1;
```

**Resultado:**

| decade_range | songs_count |
| --|--|
| 1930-1939 | 1 |
| 1940-1949 | 2 |
| 1950-1959 | 9 |
| 1960-1969 | 4 |
| 1970-1979 | 4 |
| 1980-1989 | 8 |
| 1990-1999 | 4 |
| 2000-2009 | 6 |
| 2010-2019 | 99 |
| 2020-2029 | 725 |

<br>

4. Quem são os 10 principais artistas com base na popularidade e quais são as médias de danceabilidade e energia de suas músicas?

```sql
SELECT   artist_name,
         AVG(`danceability_%`) AS avg_danceability,
         AVG(`energy_%`) AS avg_energy
FROM     spotify
GROUP BY artist_name
ORDER BY SUM(streams) DESC
LIMIT    10;
```

**Resultado:**

| artist_name | streams | avg_danceability | avg_energy |
| --|--|--|--|
| Taylor Swift | 800840817 | 59.6061 | 56.0909 |
| Bad Bunny | 303236322 | 75.1579 | 66.7368 |
| The Weeknd | 1647990401 | 59.1500 | 63.4500 |
| Olivia Rodrigo | 140003974 | 51.2857 | 50.5714 |
| Harry Styles | 743693613 | 62.4000 | 56.4000 |
| Doja Cat | 1329090101 | 80.0000 | 59.6667 |
| SZA | 1163093654 | 57.9474 | 52.6842 |
| BTS | 118482347 | 67.1250 | 69.0000 |
| Bruno Mars | 1481349984 | 61.6667 | 52.6667 |
| Ed Sheeran | 195576623 | 69.0000 | 78.0000 |

<br>

### `B. Métricas do Spotify`

1. Qual é a música presente no maior número de playlists do Spotify?

```sql
SELECT   track_name,
         artist_name,
         MAX(in_spotify_playlists) AS highest_playlists_count
FROM     spotify;
```

**Resultado:**

| track_name | artist_name | highest_playlists_count |
|--|--|--|
| Seven (feat. Latto) (Explicit Ver.) | Latto, Jung Kook | 29499 |

<br>

2. Existe uma correlação entre o número de reproduções e a presença de uma música nos rankings do Spotify?

```sql
SELECT   ROUND((
           COUNT(*) * SUM(streams * in_spotify_charts) -
           SUM(streams) * SUM(in_spotify_charts)
         ) /
         SQRT(
           (COUNT(*) * SUM(streams * streams) - (SUM(streams) * SUM(streams))) *
           (COUNT(*) * SUM(in_spotify_charts * in_spotify_charts) - (SUM(in_spotify_charts) * SUM(in_spotify_charts)))
         ), 4) AS correlation_coeff
FROM     spotify;
```

**Resultado:**

| correlation_coeff |
| ----------------- |
| 0.1859            |

<br>

3. Qual é o BPM médio das músicas no Spotify?

```sql
SELECT AVG(bpm) as average_bpm
FROM   spotify;
```

**Resultado:**

| average_bpm |
| ----------- |
| 123.0800    |

<br>

4. Qual é a média de danceabilidade das 15 músicas mais populares?

```sql
SELECT AVG(`danceability_%`) AS top15_avg_danceability
FROM (
    SELECT   track_name, streams, `danceability_%`
    FROM     spotify
    ORDER BY streams DESC
    LIMIT    15
) AS top_15_songs;
```

**Resultado:**

| top15_avg_danceability |
| --|
| 66.1333 |

---

(TODO: Continuar a tradução para todas as seções restantes mantendo os títulos das colunas e os conteúdos das linhas originais.)

