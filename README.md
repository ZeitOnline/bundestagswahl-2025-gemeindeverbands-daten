# Gemeinde- und Wahlkreis-Daten Bundestagswahl 2025

In der Wahlnacht des 23. Februar hat Zeit Online die Daten der Gemeindeverbände Deutschlands zusammengetragen. Der vorliegende Datensatz kombiniert diese mit den offiziellen Wahlkreis-Ergebnissen der Bundeswahlleiterin zu einem Datensatz der grob gesagt „auf dem Land“ die Gemeindeverbände mit den Wahlkreisen „in der Stadt“ vereint.

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

Die Daten und die Shapes können anhand der `id` gematcht werden. Die Daten beinhalten einige Gemeinden und Wahlkreise mehr als die Shapes. Bei den Gemeinden sind dies die Städte (nicht vollständig) und bei den Wahlkreisen alle regulären Wahlkreise außerhalb der Städte.

Die Gemeinden Großröhrsdorf (146250200), Arnsdorf (146250010), Wachau (146250600), Radeberg (146250480) und Ottendorf-Okrilla (146250430) in Sachsen werden im kombinierten File nicht dargestellt zugunsten des Wahlkreises Dresden II – Bautzen II (159).