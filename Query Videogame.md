#### SELECT

1.  Selezionare tutte le software house americane (3)
		SELECT * 
		FROM videogame.software_houses
		WHERE country LIKE 'United States'
		
2.  Selezionare tutti i giocatori della cittÃ di 'Rogahnland' (2) 
		SELECT * 
		FROM players
		WHERE city LIKE 'Rogahnland'
		
3.  Selezionare tutti i giocatori il cui nome finisce per "a" (220)
		SELECT * 
		FROM players
		WHERE name LIKE '%a'
		
4. Selezionare tutte le recensioni scritte dal giocatore con ID = 800 (11)
		SELECT * 
	  FROM videogame.reviews
	  WHERE player_id = 800
			
5.  Contare quanti tornei ci sono stati nell'anno 2015 (9)
		SELECT * 
		FROM videogame.tournaments
		WHERE year = 2015
		
6.  Selezionare tutti i premi che contengono nella descrizione la parola 'facere' (2)
		SELECT * 
		FROM videogame.awards
		WHERE description LIKE '%facere%' 
		
7.  Selezionare tutti i videogame che hanno la categoria 2 (FPS) o 6 (RPG), mostrandoli una sola volta (del videogioco vogliamo solo l'ID) (287)
		select distinct  videogame_id
		from category_videogame 
		where category_id =  6  or category_id = 2
		
8.  Selezionare tutte le recensioni con voto compreso tra 2 e 4 (2947)
	select * 
	from reviews 
	where rating >= 2 and rating <= 4. 
	
9.  Selezionare tutti i dati dei videogiochi rilasciati nell'anno 2020 (46)

	SELECT * 
	FROM videogame.videogames
	WHERE YEAR(release_date) = 2020
	 
10.  Selezionare gli id dei videogame che hanno ricevuto almeno una recensione da 5 stelle, mostrandoli una sola volta (443)
		SELECT DISTINCT videogame_id
		FROM videogame.reviews
		WHERE rating = 5
	

#### GROUP BY

1.  Contare quante software house ci sono per ogni paese (3)
		SELECT country, COUNT(*)
		FROM videogame.software_houses
		GROUP BY country;
		
2.  Contare quante recensioni ha ricevuto ogni videogioco (del videogioco vogliamo solo l'ID) (500)
		SELECT videogame_id, COUNT(*)
		FROM videogame.reviews
		GROUP BY videogame_id
		
3.  Contare quanti videogiochi hanno ciascuna classificazione PEGI (della classificazione PEGI vogliamo solo l'ID) (13)
		SELECT pegi_label_id, COUNT(*)
		FROM videogame.pegi_label_videogame
		GROUP BY pegi_label_id
		
4.  Mostrare il numero di videogiochi rilasciati ogni anno (11)
		SELECT YEAR(release_date), COUNT(*)
		FROM videogame.videogames
		GROUP BY YEAR(release_date)
		
5.  Contare quanti videogiochi sono disponbiili per ciascun device (del device vogliamo solo l'ID) (7)
	SELECT device_id, COUNT(*)
	FROM videogame.device_videogame
	GROUP BY device_id
	
6.  Ordinare i videogame in base alla media delle recensioni (del videogioco vogliamo solo l'ID) (500)
	SELECT videogame_id, AVG(rating) AS 'avgRating'
	FROM videogame.reviews
	GROUP BY videogame_id
	ORDER BY avgRating DESC



#### JOIN

1.  Selezionare i dati di tutti giocatori che hanno scritto almeno una recensione, mostrandoli una sola volta (996)
	SELECT DISTINCT players.id, players.*
		FROM reviews
			JOIN players
				ON reviews.player_id = players.id
				
2.  Sezionare tutti i videogame dei tornei tenuti nel 2016, mostrandoli una sola volta (226)
	SELECT DISTINCT videogames.*
		FROM tournaments
			JOIN tournament_videogame
				ON tournaments.id = tournament_videogame.tournament_id
			JOIN videogames
				ON tournament_videogame.videogame_id = videogames.id
		WHERE year = 2016 
		
3.  Mostrare le categorie di ogni videogioco (1718)
	SELECT videogames.id, videogames.name, categories.name
		FROM videogames
			JOIN category_videogame
				ON videogames.id = category_videogame.videogame_id
			JOIN categories
				ON category_videogame.category_id = categories.id
		ORDER BY videogames.id -- OPZIONALE 
		
4.  Selezionare i dati di tutte le software house che hanno rilasciato almeno un gioco dopo il 2020, mostrandoli una sola volta (6)
		SELECT DISTINCT software_houses.*
			FROM videogames
				JOIN software_houses
					ON videogames.software_house_id = software_houses.id
			WHERE YEAR(release_date) > 2020
			
5.  Selezionare i premi ricevuti da ogni software house per i videogiochi che ha prodotto (55)
	SELECT software_houses.id, software_houses.name, awards.name, videogames.name, award_videogame.year
		FROM awards
			JOIN award_videogame
				ON awards.id = award_videogame.award_id
			JOIN videogames
				ON award_videogame.videogame_id = videogames.id
			JOIN software_houses
				ON videogames.software_house_id = software_houses.id
		ORDER BY software_houses.id, year -- OPZIONALE. 
6.  Selezionare categorie e classificazioni PEGI dei videogiochi che hanno ricevuto recensioni da 4 e 5 stelle, mostrandole una sola volta (3363)
			SELECT DISTINCT videogames.id, videogames.name, pegi_labels.name, categories.name
		FROM videogame.videogames
			JOIN reviews
				ON videogames.id = reviews.videogame_id
			JOIN pegi_label_videogame
				ON videogames.id = pegi_label_videogame.videogame_id
			JOIN pegi_labels
				ON pegi_label_videogame.pegi_label_id = pegi_labels.id
			JOIN category_videogame
				ON videogames.id = category_videogame.videogame_id
			JOIN categories
				ON category_videogame.category_id = categories.id
		WHERE reviews.rating >= 4
		ORDER BY videogames.id, pegi_labels.name, categories.name -- OPZIONALE
		
7.  Selezionare quali giochi erano presenti nei tornei nei quali hanno partecipato i giocatori il cui nome inizia per 'S' (474)
	SELECT DISTINCT videogames.*
		FROM players
			JOIN player_tournament
				ON players.id = player_tournament.player_id
			JOIN tournaments
				ON player_tournament.tournament_id = tournaments.id
			JOIN tournament_videogame
				ON tournaments.id = tournament_videogame.tournament_id
			JOIN videogames
				ON tournament_videogame.videogame_id = videogames.id
		WHERE players.name LIKE 's%'
		
8. Selezionare le cittÃ in cui Ã¨ stato giocato il gioco dell'anno del 2018 (36)
	SELECT awards.id, awards.name, award_videogame.year, videogames.id, videogames.name, tournaments.name, tournaments.city
		FROM awards
			JOIN award_videogame
				ON awards.id = award_videogame.award_id
			JOIN videogames
				ON award_videogame.videogame_id = videogames.id
			JOIN tournament_videogame
				ON videogames.id = tournament_videogame.videogame_id
			JOIN tournaments
				ON tournament_videogame.tournament_id = tournaments.id
		WHERE awards.name LIKE "Gioco dell'anno"
			AND award_videogame.year = 2018
			
9.  Selezionare i giocatori che hanno giocato al gioco piÃ¹ atteso del 2018 in un torneo del 2019 (330)
	SELECT tournaments.id, tournaments.name, tournaments.year, players.name, players.lastname, players.nickname
		FROM awards
			JOIN award_videogame
				ON awards.id = award_videogame.award_id
			JOIN videogames
				ON award_videogame.videogame_id = videogames.id
			JOIN tournament_videogame
				ON videogames.id = tournament_videogame.videogame_id
			JOIN tournaments
				ON tournament_videogame.tournament_id = tournaments.id
			JOIN player_tournament
				ON tournaments.id = player_tournament.tournament_id
			JOIN players
				ON player_tournament.player_id = players.id
		WHERE awards.name LIKE "Gioco più atteso"
			AND award_videogame.year = 2018
		    AND tournaments.year = 2019