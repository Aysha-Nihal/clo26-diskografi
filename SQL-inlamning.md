# SQL-inlämning — Diskografi

**Namn:** Aysha Nihal  
**Artist:** A R Rahman  
**Datum:** 2026-10-21

---

> Lägg länk till eller bild på ditt ER-diagram här:
> `[ER-diagram](er_diagram.png)`

---

## 1. Skapa databasen — CREATE TABLE

```sql
CREATE TABLE IF NOT EXISTS artists (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE IF NOT EXISTS albums (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    release_year INTEGER,
    artist_id INTEGER,
    FOREIGN KEY (artist_id) REFERENCES artists(id)
);

CREATE TABLE IF NOT EXISTS songs (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    duration_seconds INTEGER NOT NULL,
    album_id INTEGER,
    FOREIGN KEY (album_id) REFERENCES albums(id)
);

CREATE TABLE IF NOT EXISTS genres (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL UNIQUE
);

CREATE TABLE IF NOT EXISTS song_genres (
    song_id INTEGER,
    genre_id INTEGER,
    PRIMARY KEY (song_id, genre_id),
    FOREIGN KEY (song_id) REFERENCES songs(id),
    FOREIGN KEY (genre_id) REFERENCES genres(id)
);

```

---

## 2. Seeda med data — INSERT

```sql
-- Add to Artist table
INSERT INTO artists (id, name)
VALUES (1, 'A. R. Rahman');

-- Add to Albums table
INSERT INTO albums (id, title, release_year, artist_id)
VALUES
    (1, 'Roja', 1992, 1),
    (2, 'Bombay', 1995, 1),
    (3, 'Dil Se..', 1998, 1);

-- Add to Songs table
INSERT INTO songs (id, title, duration_seconds, album_id)
VALUES
    (1, 'Chinna Chinna Aasai', 250, 1),
    (2, 'Kadhal Rojave', 270, 1),
    (3, 'Pudhu Vellai Mazhai', 300, 1),
    (4, 'Kannalane', 300, 2),
    (5, 'Uyire Uyire', 280, 2),
    (6, 'Poovukkenna Poottu', 260, 2),
    (7, 'Chaiyya Chaiyya', 420, 3),
    (8, 'Dil Se Re', 360, 3),
    (9, 'Jiya Jale', 300, 3);

-- Add to Genres table
INSERT INTO genres (id, name)
VALUES
    (1, 'Soundtrack'),
    (2, 'Folk'),
    (3, 'Pop');

-- Connect songs to genres
INSERT INTO song_genres (song_id, genre_id)
VALUES
    (1, 1),
    (2, 1),
    (3, 1),
    (4, 1),
    (5, 1),
    (6, 1),
    (7, 1),
    (7, 2),
    (8, 1),
    (8, 3),
    (9, 1);
 

```

---

## 3. Söka — SELECT

```sql
query 1: 
SELECT *
FROM albums
ORDER BY release_year ASC;


query 2: 
SELECT title
FROM songs
WHERE album_id = 1; 


query 3: 
SELECT
    albums.title,
    COUNT(songs.id) AS number_of_songs
FROM albums
JOIN songs
    ON albums.id = songs.album_id
GROUP BY albums.id; 


query 4: 
SELECT title, duration_seconds
FROM songs
ORDER BY duration_seconds DESC
LIMIT 1;


query 5: 
SELECT title
FROM songs
ORDER BY title ASC; 

```



## 4. Lägg till en ny rad — INSERT

```sql

INSERT INTO albums (id, title, release_year, artist_id)
VALUES (4, 'Alaipayuthey', 2000, 1);

INSERT INTO songs (id, title, duration_seconds, album_id)
VALUES (10, 'Pachai Nirame', 357, 4);


```



## 5. Ta bort en rad — DELETE

```sql

DELETE FROM songs
WHERE id = 11;

```



## 6. Uppdatera en rad — UPDATE

```sql

UPDATE albums
SET release_year = 2001
WHERE id = 4;

```



## 7. VG-queries (om du siktar på VG)

```sql
query 6: 
SELECT
    albums.title AS album_title,
    AVG(songs.duration_seconds) AS average_duration_seconds
FROM albums
JOIN songs
    ON albums.id = songs.album_id
GROUP BY albums.id, albums.title;


query 7: 
SELECT
    albums.title AS album_title,
    COUNT(songs.id) AS number_of_songs
FROM albums
JOIN songs
    ON albums.id = songs.album_id
GROUP BY albums.id, albums.title
ORDER BY number_of_songs DESC
LIMIT 1;


query 8: 
SELECT
    songs.title AS song_title,
    songs.duration_seconds,
    albums.title AS album_title
FROM songs
JOIN albums
    ON songs.album_id = albums.id
WHERE songs.duration_seconds > 240
ORDER BY songs.duration_seconds DESC;

```
