import json
import os
import urllib.parse
import urllib.request
from pathlib import Path

API_KEY = os.environ["OMDB_API_KEY"]

LISTS = {
    "ssd-1": [
        {"title": "2 Guns", "year": 2013, "imdbId": "tt1272878"},
        {"title": "A Few Good Men", "year": 1992, "imdbId": "tt0104257"},
        {"title": "Amadeus", "year": 1984, "imdbId": "tt0086879"},
        {"title": "American Hustle", "year": 2013, "imdbId": "tt1800241"},
        {"title": "Catch Me If You Can", "year": 2002, "imdbId": "tt0264464"},
        {"title": "City of Lies", "year": 2018, "imdbId": "tt2677722"},
        {"title": "Deadpool 2", "year": 2018, "imdbId": "tt5463162"},
        {"title": "Die Hard", "year": 1988, "imdbId": "tt0095016"},
        {"title": "Die Hard 2", "year": 1990, "imdbId": "tt0099423"},
        {"title": "Die Hard with a Vengeance", "year": 1995, "imdbId": "tt0112864"},
        {"title": "Django Unchained", "year": 2012, "imdbId": "tt1853728"},
        {"title": "Enemy at the Gates", "year": 2001, "imdbId": "tt0215750"},
        {"title": "EuroTrip", "year": 2004, "imdbId": "tt0356150"},
        {"title": "Good Will Hunting", "year": 1997, "imdbId": "tt0119217"},
        {"title": "High School High", "year": 1996, "imdbId": "tt0116531"},
        {"title": "Home Alone", "year": 1990, "imdbId": "tt0099785"},
        {"title": "Home Alone 2: Lost in New York", "year": 1992, "imdbId": "tt0104431"},
        {"title": "I, Robot", "year": 2004, "imdbId": "tt0343818"},
        {"title": "Interstate 60", "year": 2002, "imdbId": "tt0165832"},
        {"title": "John Carter", "year": 2012, "imdbId": "tt0401729"},
        {"title": "King Arthur", "year": 2004, "imdbId": "tt0349683"},
        {"title": "Kung Fu Hustle", "year": 2004, "imdbId": "tt0373074"},
        {"title": "L.A. Confidential", "year": 1997, "imdbId": "tt0119488"},
        {"title": "Lucky Number Slevin", "year": 2006, "imdbId": "tt0425210"},
        {"title": "Now You See Me", "year": 2013, "imdbId": "tt1670345"},
        {"title": "Ong-Bak: The Thai Warrior", "year": 2003, "imdbId": "tt0368909"},
        {"title": "Ong Bak 2", "year": 2008, "imdbId": "tt0785035"},
        {"title": "Ong Bak 3", "year": 2010, "imdbId": "tt1653690"},
        {"title": "Overboard", "year": 2018, "imdbId": "tt1563742"},
        {"title": "Raise Your Voice", "year": 2004, "imdbId": "tt0361696"},
        {"title": "Red Scorpion", "year": 1988, "imdbId": "tt0098180"},
        {"title": "RocknRolla", "year": 2008, "imdbId": "tt1032755"},
        {"title": "Romeo Must Die", "year": 2000, "imdbId": "tt0165929"},
        {"title": "Snatch", "year": 2000, "imdbId": "tt0208092"},
        {"title": "Stand Up Guys", "year": 2012, "imdbId": "tt1389096"},
        {"title": "Taken", "year": 2008, "imdbId": "tt0936501"},
        {"title": "Target Number One", "year": 2020, "imdbId": "tt1656177"},
        {"title": "The Departed", "year": 2006, "imdbId": "tt0407887"},
        {"title": "The International", "year": 2009, "imdbId": "tt0963178"},
        {"title": "The Mask of Zorro", "year": 1998, "imdbId": "tt0120746"},
        {"title": "The 6th Day", "year": 2000, "imdbId": "tt0216216"},
        {"title": "The Courier", "year": 2019, "imdbId": "tt8075016"},
        {"title": "The Green Mile", "year": 1999, "imdbId": "tt0120689"},
        {"title": "The Karate Kid", "year": 1984, "imdbId": "tt0087538"},
        {"title": "The New Guy", "year": 2002, "imdbId": "tt0241760"},
        {"title": "The Professor", "year": 2018, "imdbId": "tt6865690"},
        {"title": "The Siege", "year": 1998, "imdbId": "tt0133952"},
        {"title": "The Sum of All Fears", "year": 2002, "imdbId": "tt0164184"},
        {"title": "The Thomas Crown Affair", "year": 1999, "imdbId": "tt0155267"},
        {"title": "The Usual Suspects", "year": 1995, "imdbId": "tt0114814"},
        {"title": "The Warrior's Way", "year": 2010, "imdbId": "tt1032751"},
        {"title": "This Means War", "year": 2012, "imdbId": "tt1596350"},
        {"title": "Tron: Legacy", "year": 2010, "imdbId": "tt1104001"},
        {"title": "Uncle Drew", "year": 2018, "imdbId": "tt7334528"},
        {"title": "V for Vendetta", "year": 2005, "imdbId": "tt0434409"},
        {"title": "Wanted", "year": 2008, "imdbId": "tt0493464"},
    ],

    # Pentru următoarele SSD-uri adaugi listele aici:
    "ssd-2": [],
    "ssd-3": [],
}

POSTERS_DIR = Path("posters")
MOVIES_DIR = Path("movies")

POSTERS_DIR.mkdir(exist_ok=True)
MOVIES_DIR.mkdir(exist_ok=True)


def get_json(url):
    with urllib.request.urlopen(url, timeout=30) as response:
        return json.loads(response.read().decode("utf-8"))


def download(url, path):
    with urllib.request.urlopen(url, timeout=30) as response:
        path.write_bytes(response.read())


def enrich_movie(movie):
    imdb_id = movie["imdbId"]
    url = f"https://www.omdbapi.com/?i={urllib.parse.quote(imdb_id)}&apikey={API_KEY}"
    data = get_json(url)

    enriched = dict(movie)
    enriched["imdbRating"] = data.get("imdbRating", "N/A")
    enriched["runtime"] = data.get("Runtime", "")
    enriched["genre"] = data.get("Genre", "")
    enriched["plot"] = data.get("Plot", "")

    poster_url = data.get("Poster")
    poster_path = POSTERS_DIR / f"{imdb_id}.jpg"

    if poster_url and poster_url != "N/A" and not poster_path.exists():
        print(f"Downloading poster: {enriched['title']}")
        download(poster_url, poster_path)

    return enriched


total = 0

for list_name, movie_list in LISTS.items():
    enriched_movies = [enrich_movie(movie) for movie in movie_list]

    output_path = MOVIES_DIR / f"{list_name}.json"
    output_path.write_text(
        json.dumps(enriched_movies, ensure_ascii=False, indent=2),
        encoding="utf-8",
    )

    print(f"Updated {output_path} with {len(enriched_movies)} movies.")
    total += len(enriched_movies)

print(f"Done. Updated {len(LISTS)} lists, {total} movies total.")
