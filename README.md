# Bundestagswahl 2025: Gemeinde- und Wahlkreis-Daten

In der Wahlnacht des 23. Februar hat Zeit Online die Daten der Gemeindeverbände Deutschlands zusammengetragen. Der vorliegende Datensatz kombiniert diese mit den offiziellen Wahlkreis-Ergebnissen der Bundeswahlleiterin zu einem Datensatz der grob gesagt „auf dem Land“ die Gemeindeverbände mit den Wahlkreisen „in der Stadt“ vereint. Die Daten wurden am 25. Februar 2025 in diesem [Artikel auf Zeit Online](https://www.zeit.de/politik/deutschland/2025-02/bundestagswahl-wahlergebnisse-gemeinden-wahlkarte) veröffentlicht.

Der Datensatz enthält Fehler. Diese sind einerseits dort entstanden, wo Personen falsche Werte eingetragen haben (z.B. mehr Wählende als Wahlberechtigte). Andererseits sind auch Fehler, die durch das Zusammentragen der Daten entstanden sind (Scraping), nicht ausgeschlossen. Sie können gerne per [Mail an uns](mailto:bundestagswahl@zeit.de) gemeldet werden.

## Quellen

Die Daten wurden von folgenden Stellen bezogen.

 **Bundesland**             | **Quelle**                                                                
----------------------------|---------------------------------------------------------------------------
 **Baden-Württemberg**      | [Statistisches Landesamt](https://www.statistik-bw.de/Wahlen/Bundestag/Download.jsp)                 
 **Bayern**                 | Seiten der VoteGroup ([Bsp](https://btwahl.ingolstadt.de/ergebnisse_gemeinde_09161000.html))
 **Berlin**                 | Bundeswahlleiterin       
 **Brandenburg**            | [Statistik Berlin Brandenburg](https://www.statistik-berlin-brandenburg.de/bundestagswahlen-brandenburg)  
 **Bremen**                 | Bundeswahlleiterin                                                                          
 **Hamburg**                | Bundeswahlleiterin                          
 **Hessen**                 | [Statistisches Landesamt](https://statistik.hessen.de/unsere-zahlen/wahlen)
 **Mecklenburg-Vorpommern** | https://www.laiv-mv.de/Wahlen/                                            
 **Niedersachsen**          | Seiten der VoteGroup und von VoteManager ([Bsp](http://vote-aws.kdo.de/20250223/03452000/praesentation/))                                                                          
 **Nordrhein-Westfalen**    | Seiten von VoteManager und [KRZN](https://wahl.krzn.de/bw2025/)                                                                          
 **Rheinland-Pfalz**        | [Landeswahlleiter](https://www.wahlen.rlp.de/bundestagswahl/ergebnisse)                       
 **Saarland**               | [Landeswahlleiterin](https://wahlergebnis.saarland.de/BTW/)
 **Sachsen**                | [Statistisches Landesamt](https://wahlen.sachsen.de/bundestagswahl-2025.html)                        
 **Sachsen-Anhalt**         | [Statistisches Landesamt](https://wahlergebnisse.sachsen-anhalt.de/)                                 
 **Schleswig-Holstein**     | Seiten der VoteGroup ([Bsp](https://www.wahlen-sh.de/btw25/))
 **Thüringen**              | [Landesamt für Statistik](https://wahlen.thueringen.de/ )                                            

## Datenbeschreibung

### gem_wk_data

Liegt als CSV und RDS (R data serialized) vor und beinhaltet folgende Spalten:

 **Spaltenname**     | **Typ**   | **Beschreibung**                                                                                      
---------------------|-----------|-------------------------------------------------------------------------------------------------------
 **id**              | character | Identifikationsnummer der Geografie (1- bis 3-stellig für Wahlkreise, 9-stellig für Gemeindeverbände) 
 **name**            | character | Name der Geografie                                                                                    
 **wahl**            | factor    | Erst-/Zweitstimme                                                                                     
 **partei**          | factor    | Parteikürzel                                                                                          
 **wahlberechtigte** | integer   | Anzahl Wahlberechtigte                                                                                
 **waehlende**       | integer   | Anzahl Wählende                                                                                       
 **ungueltige**      | integer   | Anzahl ungültige Stimmen                                                                              
 **gueltige**        | integer   | Anzahl gültige Stimmen                                                                                
 **stimmen**         | integer   | Anzahl Stimmen für die Partei                                                                         

Die 4 Spalten wahlberechtigte bis gueltige sind über jede Geometrie identisch. Die Doppelung existiert an dieser Stelle, sodass die Anteile jeder Partei einfacher berechnet werden kann (`stimmen / gueltige`).

Für die Gemeinden Augsburg, Berlin, Bremen, Bremerhaven, Dresden, Gelsenkirchen, Hamburg, Leipzig, Leverkusen und München liegen keine Gemeindedaten sondern nur Wahlkreisdaten vor.

### shapes

Passend zu den Daten liegen 4 Shapefiles vor:
- **gemeindeverbaende_ohne_staedte.shp**\
Gemeindeverbände ohne Städte (4.553 Features)
- **gemeindeverbaende.shp**\
Alle Gemeindeverbände (4.593 Features)
- **kombiniert.shp**\
Gemeindeverbände und Wahlkreise in Städten (4.617 Features)
- **staedte_wahlkreise.shp**\
Wahlkreise in Städten einzeln (64 Features)
- **wahlkreise.shp**\
Alle Wahlkreise (299 Features)

Die Datei **kombiniert.shp** ist die Kombination aus den Dateien **staedte_wahlkreise.shp** und **gemeindeverbaende_ohne_staedte.shp** wobei die Grenzen der Gemeinden den Wahlkreisen angeglichen wurde, sodass valide Topografien entstehen.

Alle Dateien liegen im Koordinatensystem WGS84 vor und haben folgende Spalten:

 **Spaltenname** | **Typ**   | **Beschreibung**                                                                                      
-----------------|-----------|-------------------------------------------------------------------------------------------------------
 **id**          | character | Identifikationsnummer der Geografie (1- bis 3-stellig für Wahlkreise, 9-stellig für Gemeindeverbände) 
 **name**        | character | Name der Geografie                                                                                    
 **land**        | character | Bundesland                                                                                            
 **type**        | character | Bezeichnung des Geografie-Typs (z.B. Wahlkreis oder Samtgemeinde)                                     

Die Daten und die Shapes können anhand der `id` gematcht werden. Die Daten beinhalten der Vollständigkeit halber auch alle Wahlkreise außerhalb der Städte.

Die Gemeinden Großröhrsdorf (146250200), Arnsdorf (146250010), Wachau (146250600), Radeberg (146250480) und Ottendorf-Okrilla (146250430) in Sachsen werden im kombinierten File nicht dargestellt zugunsten des Wahlkreises Dresden II – Bautzen II (159).

Quellen der Shapefiles: [Bundesamt für Kartographie und Geodäsie BKG](https://gdz.bkg.bund.de/index.php/default/open-data.html) für Gemeindeverbände und [Bundeswahlleiterin](https://www.bundeswahlleiterin.de/bundestagswahlen/2025/wahlkreiseinteilung/downloads.html) für Wahlkreise.

---

<p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://github.com/ZeitOnline/zg-bundestagswahl-2025-gemeinde-daten">Dieser Datensatz</a> von <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://www.zeit.de/daten-und-visualisierung">Zeit Online</a> untersteht der Lizenz <a href="https://creativecommons.org/licenses/by-sa/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">CC BY-SA 4.0 <img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1" alt=""><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1" alt=""><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/sa.svg?ref=chooser-v1" alt=""></a></p> 