# LeMetadatarr

LeMetadatarr is a collection of typed Python clients and scrapers for media
and reference sources — music, video, games, books, and specialist lexicons.
The goal is twofold: **voice-enabling websites** (typed clients that let
voice assistants search and play from these sources) and **creating open
datasets** for speech and NLP work — entity vocabularies, name
pronunciations, and search-intent corpora for ASR, TTS, and NER training.
Each client emits typed records (either the shared `mediavocab` schema for
media releases, or a source-specific typed model), so downstream code does
not need to special-case each site's response format.

The datasets produced by these clients are published to the
[LeData](https://huggingface.co/LeData) organization on Hugging Face,
organized into collections: [Media Metadata](https://huggingface.co/collections/LeData/media-metadata-6a4952f7f8ef9e590a4e8be2),
[Music](https://huggingface.co/collections/LeData/music-metadata-6a4953339beb85d82c863580),
[Movies](https://huggingface.co/collections/LeData/movie-metadata-6a49537993b4d11cf9fddf37),
[IMDB](https://huggingface.co/collections/LeData/imdb-metadata-6a49534fad51e30093e400fc),
[Anime & Manga](https://huggingface.co/collections/LeData/anime-and-manga-metadata-6a49537092b7e578190e1211),
[Games](https://huggingface.co/collections/LeData/games-metadata-6a495363b0032ce145576f29),
[ROM Hacks](https://huggingface.co/collections/LeData/rom-hacks-metadata-6a49536ab0032ce145576fd6),
[Books & Podcasts](https://huggingface.co/collections/LeData/books-and-podcasts-6a49531b6f0b826071942f29), and
[Drugs and Substances](https://huggingface.co/collections/LeData/drugs-and-substances-6a4952c9a7b9921d3f9a3ed7).
LeMetadatarr is the extraction layer; LeData is where the resulting
datasets live. Most repos ship a `dataset.py` module that exports the
client's data as Hugging Face-compatible JSONL/parquet configs, matching
what is published under LeData.

## Transport / infrastructure

Shared low-level tooling used by the scraper clients to reach sources that
block plain HTTP requests.

| Repo | Purpose |
| --- | --- |
| [unblock_requests](https://github.com/LeMetadatarr/unblock_requests) | `requests.Session` subclass that bypasses Cloudflare via curl_cffi, FlareSolverr, or the Wayback Machine |
| [anon_requests](https://github.com/LeMetadatarr/anon_requests) | Anonymous / proxy-rotated requests |
| [sitemapper](https://github.com/LeMetadatarr/sitemapper) | Site discovery and URL enumeration (sitemap.xml, robots.txt, crawl fallback) |

## Orchestration

| Repo | Purpose |
| --- | --- |
| [metadatarr](https://github.com/LeMetadatarr/metadatarr) | Pydantic-powered clients plus a keyless cross-source entity resolver (MusicBrainz, Wikidata, OpenLibrary, AniList, Jikan, Discogs, ...). Also bundles ~50 resumable dataset scrapers under `scrapers/` — drug registries (FDA, EMA, WHO ATC, ChEMBL, PubChem, RxNorm, DailyMed, KEGG and 12+ national registers), pronunciation lexicons (CMU, Wiktionary), and media catalogues (OpenLibrary, Steam, LibriVox, ListenNotes, PodcastIndex, Tidal, Deezer, TMDB, TVmaze, RAWG, RadioBrowser, Wikidata) — feeding the LeData collections |
| [media-archivist](https://github.com/LeMetadatarr/media-archivist) | Indexes, canonicalizes, and deduplicates media catalogues from YouTube, Bandcamp, SoundCloud, and Internet Archive into a typed `mediavocab` dataset |

## Music

| Repo | Purpose |
| --- | --- |
| [pymusicbrainz](https://github.com/LeMetadatarr/pymusicbrainz) | MusicBrainz web service and streaming bulk data dumps |
| [pydiscogs](https://github.com/LeMetadatarr/pydiscogs) | Discogs monthly bulk data dumps (Artists, Labels, Masters, Releases) |
| [py_bandcamp](https://github.com/LeMetadatarr/py_bandcamp) | Bandcamp scraper — tracklists, credits, Creative Commons licensing |
| [nuvem_de_som](https://github.com/LeMetadatarr/nuvem_de_som) | SoundCloud client |
| [pymetal](https://github.com/LeMetadatarr/pymetal) | Metal Archives client |
| [pyclassicalarchives](https://github.com/LeMetadatarr/pyclassicalarchives) | Classical Archives client |
| [pyjazzmusicarchives](https://github.com/LeMetadatarr/pyjazzmusicarchives) | Jazz Music Archives client |
| [pyprogarchives](https://github.com/LeMetadatarr/pyprogarchives) | Prog Archives client |
| [pyrateyourmusic](https://github.com/LeMetadatarr/pyrateyourmusic) | RateYourMusic (Sonemic) scraper |
| [py-music-assistant](https://github.com/LeMetadatarr/py-music-assistant) | Music Assistant server API client |
| [xazam](https://github.com/LeMetadatarr/xazam) | Async Shazam API client |
| [pyheartradio](https://github.com/LeMetadatarr/pyheartradio) | iHeartRadio API client |
| [tunein](https://github.com/LeMetadatarr/tunein) | TuneIn radio and IPTV scraper |

## Video / anime

| Repo | Purpose |
| --- | --- |
| [tutubo](https://github.com/LeMetadatarr/tutubo) | YouTube scraper — channels, videos, music, podcasts, livestreams, IPTV |
| [pyimdb](https://github.com/LeMetadatarr/pyimdb) | IMDb client (suggestion API, bulk datasets, page crawl) |
| [pymal](https://github.com/LeMetadatarr/pymal) | MyAnimeList client (anime, manga, characters, ARM cross-references) |

## Games / ROM hacking

| Repo | Purpose |
| --- | --- |
| [pyromhacking](https://github.com/LeMetadatarr/pyromhacking) | romhacking.net (RHDN) — ROM hacks, fan translations, patching utilities |
| [pysmwcentral](https://github.com/LeMetadatarr/pysmwcentral) | smwcentral.net public API client |
| [pytcrf](https://github.com/LeMetadatarr/pytcrf) | The Cutting Room Floor — unused/cut/debug game content |
| [pyvndb](https://github.com/LeMetadatarr/pyvndb) | VNDB (Visual Novel Database) client |

## Substance lexicons (ASR / NER data)

Substance and pharmaceutical names are a hard out-of-vocabulary problem for
speech recognition. These clients collect substance nomenclature — names,
synonyms, and cross-references — as training data for ASR and
entity-recognition models.

| Repo | Purpose |
| --- | --- |
| [pyerowid](https://github.com/LeMetadatarr/pyerowid) | Erowid client and dataset dumper — substance-name vocabulary |
| [pypsychonaut](https://github.com/LeMetadatarr/pypsychonaut) | PsychonautWiki client and dataset dumper — substance nomenclature and cross-references |
| [pytripsit](https://github.com/LeMetadatarr/pytripsit) | TripSit factsheet client — substance names, aliases, and interaction metadata |

## Books & audiobooks

| Repo | Purpose |
| --- | --- |
| [pygutenberg](https://github.com/LeMetadatarr/pygutenberg) | Project Gutenberg client (Gutendex API, public-domain text, bulk catalog) |
| [audiobooker](https://github.com/LeMetadatarr/audiobooker) | Public-domain audiobooks (LibriVox, LoyalBooks, Anna's Archive) |

Book and podcast catalogue scrapers (OpenLibrary, LibriVox, ListenNotes,
PodcastIndex) ship inside [metadatarr](https://github.com/LeMetadatarr/metadatarr)`/scrapers/`
and publish to the [Books & Podcasts](https://huggingface.co/collections/LeData/books-and-podcasts-6a49531b6f0b826071942f29)
collection.

## Media reference

| Repo | Purpose |
| --- | --- |
| [pytvtropes](https://github.com/LeMetadatarr/pytvtropes) | tvtropes.org scraper |

## AI transparency

Most of the scrapers in this organization were written with AI assistance
(Anthropic Claude), with human review. Dataset cards on
[LeData](https://huggingface.co/LeData) document how each dataset was
collected and processed.
