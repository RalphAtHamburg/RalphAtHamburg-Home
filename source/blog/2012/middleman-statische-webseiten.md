--- 
title: Middleman für statische Webseiten
date: 2012/03/15
TODO: Umlaute gehen aufgrund eines Bugs nicht UTF-8 and ASCII-8BIT encoding
---

Wenn man es gewohnt ist Webanwendungen mit Ruby on Rails zu entwickeln, guckt man ziemlich dumm drein, wenn man auf einmal eine komplexe statische Webseite bauen muss. Die ganzen Möglichkeiten sein Projekt zu strukturieren, die vielen Helper Methoden und jede Menge weiterer Features fehlt einem plötzlich. Doch zum Glück gibt es [Middleman](http://middlemanapp.com/).

Middleman ist ein Generator für statische Webseiten. Er ist in Ruby geschrieben und basiert auf dem Web Framework Sinatra. Salopp gesagt handelt es sich um Ruby on Rails, bei dem von der MVC Architektur nur die View Schicht übrig geblieben ist. Modell und Controller fehlen. Zudem unterstützt Middleman eine Vielzahl Templatesprachen (Haml, Sass, Compass, Slim, CoffeeScript, und viele mehr). Die Komprimierung von .js und .css Dateien sowie zur Erzeugung von Sprites  runden den Entwicklungszyklus ab. Hier jetzt einige Bereiche, in denen Middelman bei der Erstellung von statischen Webprojekten helfen kann.

Elemente, die auf mehreren Seiten vorkommen, wie z.B. Navigation, Fußzeile, kann man in eigene Dateien auslagern und als `partial` in die Seiten einfügen. 

~~~ erb
<header>
  <%= image_tag 'logo.png' %>
  <%= partial 'menu' %>
</header>
~~~
    
Auch Stylesheet Dateien lassen sich einfach auf mehre Dateien aufteilen. So verliert man nicht mehr den überblick in 1000 Zeilen langen CSS Dateien. Beim Erstellen der Seite fasst Middelman alles zu einer Datei zusammen und spart somit unnötige Requests.

Innerhalb der Seiten hat man vollen Zugriff auf die Sprache Ruby. So kann man leicht über Variablen z.B. den active Zustand von Menüs steuern. (Codebeispiele in Haml und mit Ruby on Rails tag helper) 
    
~~~ haml
%li= link_to 'Home', 'index.html', :class => ('active' if @home)
%li= link_to 'About', 'about.html', :class => ('active' if @about)  
%li= link_to 'Contact', 'contact.html', :class => ('active' if @contact)  
~~~
    
Komplexere Logik läßt sich in Helper Methoden auslagern. Weiterhin stehen einem die  Ruby on Rails Helper Tags zur Verfügung.

Für die Internationalisierung von Seiten kann man die Rails I18n API nutzen. Dabei werden alle Strings in eine yaml Datei ausgelagert. 

~~~ yaml
de:  
  title:   
    headline: "SocialWeb Development"  
    subheadline: "Beratung Entwicklung"  
    kontakt: "Kontakt"   
    impressum: "Impressum"  
~~~
    
Auf den Seiten angesprochen werden sie in der Form "t("title.headline")". Dank der Möglichkeit Ruby einsetzen zu können, lassen sich auch Grafiken oder andere nicht textuelle Features sprachabhängig anpassen.

~~~ haml
%div
  = image_tag 'pic-de.jpg' if locale == :de
  = image_tag 'pic-en.jpg' if locale == :en
~~~
    
Für jede weitere Sprache ist es lediglich nötig, die entsprechende Sprachdatei in den Ordner "locales" zu legen. Middleman generiert dann alle nötigen statischen Seiten.

Middleman kommt mit vorkonfigurierten leeren Projekten (z.B. HTML5). Auf Github gibt es eine ganze Menge fertiger Projekte, wie z.B. [Middleman Bootstrap](https://github.com/nathos/middleman-bootstrap) welches auf Boilerplate, Compass und dem Suzy Grid basiert. Eigene Projekte lassen sich einfach in einem Verzeichnis unter "~/.middleman" ablegen. Dann kann man mit "middleman init myproject" gleich mit einem leeren Rahmen loslegen.

Dies sind nur einige Beispiele dafür, was man mit Middleman machen kann. Die Möglichkeiten Middleman bietet sind schon gewaltig. Ich habe Middleman gerade in einem komplexen Projekt eingesetzt und werde es für statische Seiten jetzt immer nehmen. Middleman wir seit 2010 entwickelt, die zur Zeit aktuelle Version ist 2.0. Middleman 3.0 ist aber schon im Beta.

Am Beispielcode dieser Seite zeigt einige Features von Middleman. Zusätzlich zu den oben beschriebenen Themen nutze ich noch einige Compass Mixins sowie die automatische Generierung von Sprites.

Links:  
[Quellcode dieser Seite](https://github.com/RalphAtHamburg/RalphAtHamburg), [HAML](http://haml-lang.com/),  [SASS](http://sass-lang.com/),  [Compass](http://compass-style.org/)
