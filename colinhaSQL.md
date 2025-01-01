
## Para achar o id do artista pelo nome

```sql
SELECT id
FROM Artista 
WHERE nome = 'Victor Mühlethaler';
```

## Para achar o id da música pelo nome

```sql
SELECT id
FROM Musica
WHERE titulo = 'Desafiar a Gravidade'
```

## Para achar músicas de um artista pelo id dele

```sql
SELECT titulo
FROM Musica
WHERE id = (
    SELECT musica_id
    FROM ArtistaMusica
    WHERE artista_id = '1817'
);
```