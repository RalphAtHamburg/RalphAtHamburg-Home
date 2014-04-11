---
title: Blogbeiträge als statische Seiten
date: 2012/05/23
---

Schon öfters habe ich mich gefragt, ob es Sinn macht, ein CMS (Wordpress ect.) einzusetzen, wenn sich die Inhalte der Seite nur selten ändern. Zu groß sind die Nachteile: Man braucht eine Datenbank, meist (MYSQL), und ein CMS (meist Wordpress). Damit verliert man alle Freiheiten, die man bei der Entwicklung einer rein statische Website hat. Man muss sich jetzt zusätzlich um die Datenbank kümmern (Backup) und sein CMS up-to-date halten. Darüber hinaus hat man sich auch gleich noch ein Performance Problem eingehandelt. Die Standardkonfigurationen der bekannten Hoster sind alle "out-of-the-box" recht langsam.

Wenn man also schon weiß, dass man es doch nur schafft, eine Handvoll Beiträge pro Jahr zu schreiben, kann man sie auch gleich als statische Seiten anlegen. Erst recht, wenn man Webentwickler ist und die notwendigen Werkzeuge sowieso auf dem Rechner hat.

Das geht bequemer als man glaubt, da es Tools zur Unterstützung gibt. Eins davon ist [Middleman](http://middlemanapp.com/), mit dem auch diese Seite erstellt ist.

Zuerst erstellt man ein Template für die Blog Seite(n) und baut es in die Website ein. Für die gewünschten Seitentypen erstellt man einfach Templates, z.B.:

~~~ ruby
blog_year_template
blog_month_template
blog_day_template
~~~

Dabei stehen einem eine Reihe von Methoden zur Verfügung. 

~~~ ruby
is_blog_article?
current_article.date
current_article.title
current_article.metadata
paginate
tag_path(tag)
~~~
    
Steht die Struktur erst einmal, kann man anfangen, seine Beiträge zu schreiben.

Die Beiträge werden im Middleman Projekt in einer festen Ordnerstruktur abgelegt, z.B.:

    blog/2012/05
    
Man kann auch einfach "middleman article 'Statische Blog Seiten'" eintippen, Middleman erstellt dann die entsprechende Datei  an der richtigen Stelle.

Es ist noch nicht einmal nötig, die Beiträge in HTML zu verfassen. Man kann alles in **Markdown** schreiben. Die Formatierungsmöglichkeiten reichen für ein Blog vollkommen aus. Middleman unterstützt dabei  verschiedene Markdown Engines.

Jetzt ist also der richtige Zeitpunkt, sich einen dieser ultraschicken, minimalistischen, stylischen Markdown Editoren wie [iaWriter](http://www.iawriter.com/) oder [Byword](http://bywordapp.com/) zuzulegen. Dank iCloud Unterstützung kann man so von überall mit iPhone oder iPad an seinen Beiträgen arbeiten. Ein simpler Texteditor tut es natürlich such.

Ein Artikel in Markdown sieht dann so aus:

	---
	title: Statische Blog Seiten
	date: 2012/05/23
	tags: blogging, middleman, static, development
	---

	# Hello World

	lorem ipsum .....

	* one
    * two

      def foo
        bar
      end

    lorem ipsum .....

Mit dem Befehl "middleman build" wird anschließend die statische Website gebaut, die man dann nur noch auf den Server hochladen muss. Noch einfacher geht das mit dem [rakefile](https://gist.github.com/1902178#file_rakefile) von [Scott W.Bradley](http://scottwb.com/blog/2012/02/24/middleman-deployment-rakefile/). Damit ist nur noch der Befehl `rake deploy` einzutippen.

Fehlt schließlich noch die Kommentarfunktion. Die kann man gut outsourcen, z.B. auf [disqus](http://disqus.com). Und schon hat man eine schlanke performante Seite ohne unnötigen Overhead.

Das gleiche gilt natürlich auch für andere (quasi) statische Webseiten, bei denen der Inhalt nicht unbedingt aus einer Datenbank kommen muss.
