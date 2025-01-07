## Esquema do banco de dados:
```sql
CREATE TABLE Musica (
    id INTEGER PRIMARY KEY,
    titulo TEXT NOT NULL,
    letra TEXT
);
CREATE TABLE Obra (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT NOT NULL
);
CREATE TABLE MusicaObra (
    musica_id INTEGER NOT NULL,
    obra_id INTEGER NOT NULL,
    PRIMARY KEY (musica_id, obra_id),
    FOREIGN KEY (musica_id) REFERENCES Musica (id),
    FOREIGN KEY (obra_id) REFERENCES Obra (id)
);
CREATE TABLE Artista (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT NOT NULL
);
CREATE TABLE ArtistaMusica (
    artista_id INTEGER NOT NULL,
    musica_id INTEGER NOT NULL,
    PRIMARY KEY (artista_id, musica_id),
    FOREIGN KEY (artista_id) REFERENCES Artista (id),
    FOREIGN KEY (musica_id) REFERENCES Musica (id)
);
```


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
WHERE titulo = 'Desafiar a Gravidade';
```

## Para achar músicas de um artista pelo id dele

```sql
SELECT titulo
FROM Musica
WHERE id = (
    SELECT musica_id
    FROM ArtistaMusica
    WHERE artista_id = 'id da música'
);
```

## Para achar o nome de uma obra pelo id dela

```sql
SELECT nome
FROM Obra
WHERE id = 'id da obra';
```

## Para achar o id de uma obra pelo nome dela

```sql
SELECT id
FROM Obra
WHERE nome = 'Wicked (Brasil, 2016)';
```


## Para achar em que obra uma música está (procura pelo nome, então pode haver músicas diferentes com o mesmo nome)

```sql
SELECT nome
FROM Obra
WHERE id IN (
    SELECT obra_id
    FROM MusicaObra
    WHERE musica_id IN (
        SELECT id
        FROM Musica
        WHERE titulo = 'Desafiar a Gravidade'
    )
);
```
## Para achar todas as músicas dentro de uma obra pelo nome dela

```sql
SELECT titulo
FROM Musica
WHERE id IN (
    SELECT musica_id
    FROM MusicaObra
    WHERE obra_id = (
        SELECT id
        FROM Obra
        WHERE nome = 'Wicked (Brasil, 2016)'
    )
);
```

## Para achar as músicas associadas a mais de uma obra

```sql
SELECT 
    m.id AS musica_id,
    m.titulo AS musica_titulo,
    COUNT(mo.obra_id) AS numero_de_obras
FROM 
    Musica m
JOIN 
    MusicaObra mo ON m.id = mo.musica_id
GROUP BY 
    m.id, m.titulo
HAVING 
    COUNT(mo.obra_id) > 1;
```
