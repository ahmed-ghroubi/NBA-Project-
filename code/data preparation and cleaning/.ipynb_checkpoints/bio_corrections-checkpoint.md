## Main data source

Player bio data are collected from NBA.com through the Python package
[`nba_api`](https://github.com/swar/nba_api), specifically the
`LeagueDashPlayerBioStats` endpoint.

The `nba_api` package provides a Python interface to access NBA statistics
endpoints and was used to retrieve player biographical information for each
regular season from 1996-97 to 2025-26.

Sources:

- `nba_api` GitHub repository (documentation and source code):  
https://github.com/swar/nba_api

- `nba_api` Python package on PyPI (installation and package information):  
https://pypi.org/project/nba_api/

## Manual data corrections

A small number of values returned by the NBA API were missing or inconsistent. These cases were checked manually against external sources before being corrected.

### Draft information

The NBA API contained inconsistent draft information for three players. These records were checked manually and corrected using external player/draft references.

| Player | Player ID | Corrected draft year | Draft round | Draft number | Reference |
|---|---:|---:|---|---|---|
| Jerry Smith | 202536 | 2010 | Undrafted | Undrafted | [RealGM](http://basketball.realgm.com/player/Jerry-Smith/Summary/2391) |
| Walker Russell Jr. | 201041 | 2006 | Undrafted | Undrafted | [RealGM](http://basketball.realgm.com/player/Walker-Russell/Summary/1967) |
| Mark Jones | 2891 | 1998 | Undrafted | Undrafted | [The Draft Review](http://www.thedraftreview.com/index.php?option=com_content&view=article&id=14076) |

### Missing height and weight

Missing height and weight values are first filled from another season of the **same player** using forward/backward filling. Only players for whom no value exists in any observed season are corrected manually.

The remaining missing values were manually verified using the corresponding
Basketball-Reference player pages.

| Player | Player ID | Height used | Weight used | Basketball-Reference |
|---|---:|---:|---:|---|
| Ratko Varda | 2237 | 216 cm | 117 kg | [Profile](https://www.basketball-reference.com/players/v/vardara01.html) |
| Jerrelle Benimon | 203968 | 203 cm | 111 kg | [Profile](https://www.basketball-reference.com/players/b/benimje01.html) |
| Khalid El-Amin | 2064 | 178 cm | 90 kg | [Profile](https://www.basketball-reference.com/players/e/elamikh01.html) |
| Ruben Garces | 2092 | 206 cm | 111 kg | [Profile](https://www.basketball-reference.com/players/g/garceru01.html) |
| Terrance Roberson | 2121 | 201 cm | 97 kg | [Profile](https://www.basketball-reference.com/players/r/roberte01.html) |
| Will Cherry | 203805 | 183 cm | 83 kg | [Profile](https://www.basketball-reference.com/players/c/cherrwi01.html) |
| Chris Youngblood | 1642959 | 193 cm | 100 kg | [Profile](https://www.basketball-reference.com/players/y/youngch01.html) |
| Jahmyl Telfort | 1643141 | 200.7 cm | 102 kg | [Profile](https://www.basketball-reference.com/players/t/telfoja01.html) |
| Jaylen Wells | 1642377 | 200.7 cm | 93 kg | [Profile](https://www.basketball-reference.com/players/w/wellsja01.html) |
| LJ Cryer | 1643018 | 182.9 cm | 91 kg | [Profile](https://www.basketball-reference.com/players/c/cryerlj01.html) |
| Lawson Lovering | 1643133 | 213.4 cm | 111 kg | [Profile](https://www.basketball-reference.com/players/l/loverla01.html) |
| Tolu Smith | 1642449 | 210.8 cm | 115 kg | [Profile](https://www.basketball-reference.com/players/s/smithto05.html) |
| Patrick Christopher | 203565 | 196 cm | 94 kg | [Profile](https://www.basketball-reference.com/players/c/chrispa01.html) |
| Jamal Robinson | 1554 | 201 cm | 96 kg | [Profile](https://www.basketball-reference.com/players/r/robinja03.html) |
| Ike Fontaine | 1829 | 193 cm | 95 kg | [Profile](https://www.basketball-reference.com/players/f/fontais01.html) |
| Reggie Williams | 199 | 201 cm | 86 kg | [Profile](https://www.basketball-reference.com/players/w/willire01.html) |

Some Basketball-Reference profiles display metric measurements rounded to whole centimeters or kilograms. Decimal centimeter values in the cleaned dataset come from converting the listed feet/inches measurements using `1 inch = 2.54 cm`.
## Output

The raw file preserves the original API output, while the processed file
contains the selected, renamed, converted, and manually checked player bio
variables used later in the project.
