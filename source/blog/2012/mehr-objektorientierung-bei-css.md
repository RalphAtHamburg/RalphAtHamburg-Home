--- 
title: Mehr Objektorientierung bei CSS 
date: 2012/06/23
tags: css, Klassen, OO
---

Im Frontend Bereich ist dank CSS3, HTML5, JavaScript, CSS Transitions und Responsive Design zur Zeit viel in Bewegung. Es wird an verschiedenen Stellen nach dem optimalen Workflow und nach - neuen - Best Practice gesucht. Alte Best practice werden auf den Prüfstand gestellt und u.U. verworfen. [SMACCS](http://smacss.com/) ist ein Guide um moderneres, modulareres CSS zu schreiben. (Es gibt weitere Versuche zur Strukturierung von CSS wie [BEM](http://bem.github.com/bem-method/pages/beginning/beginning.en.html) oder [OOCSS](http://oocss.org/)).

Die Hauptziele sind:

* Mehr Semantik im HTML Markup per Klassen
* Weniger Koppelung des CSS an die HTML Struktur

Es wird empfohlen, die Stylesheets in unterschiedliche Layer zu gruppieren. SMACCS schlägt diese 5 Layer vor:

* Base (Defaul Gestaltung, resets ...)
* Layout (Seitenaufbau, Spalten ...)
* Modules ( Listen, Text Boxen, Störer ...)
* State ( Zustände, is-actice, is-selected ..)
* Themes ( )

Ziel ist es, den CSS Code konsistenter, weniger redundant und wiederverwendbarer zu gestalten. Konsequenterweise wird dabei den Klassen Selektoren mehr Gewicht beigemessen als den ID-Selektoren.

> At the very core of SMACSS is categorization. By categorizing CSS rules, we begin to see patterns and can define better practices around each of these patterns.

Interessant wird es im Kapitel über Module. Module sind CSS Klassen, die an allen Stellen im HTML angewendet werden 
können. Typische Module sind die Navigationsleiste, Listen, Widgets usw.

> Modules sit inside Layout components. Modules can sometimes sit within other Modules, too. Each Module should be designed to exist as a standalone component. In doing so, the page will be more flexible. If done right, Modules can easily be moved to different parts of the layout without breaking.

Was ist zu beachten, wenn man Module entwickelt, die den o.g. Anforderungen entsprechen? Hier ist ein Vortrag von Andy Hume interessant [Audio](http://audio.sxsw.com/2012/podcasts/10-ACC-CSS_for_Grownups.mp3), [Slides](https://speakerdeck.com/u/andyhume/p/css-for-grown-ups-maturing-best-practises). Andy Hume stellt u.a. ein altes Paradigma in Frage, demzufolge man keine beschreibenden Klassennamen verwenden soll. Er sieht das HTML eher als API, an die das CSS gebunden wird. Dabei fackelt er nicht lange herum, sondern stellt gleich zu Anfang die provokante Frage, was denn an dieser Klasse so schlimm sein soll.

~~~ css
.green {
  color: green;
}
~~~ 

Sieht man das HTML als Art API für die Gestaltung per CSS, dann ist das eine klare eindeutige Schnittstelle. Bisher ist es eher so, dass die Gestaltung per Type Selector an HTML Elemente angedockt wird:
  
~~~ css
h1 { color: blue; }
~~~
    
Dieses Vorgehen wird jetzt heftig in Frage gestellt.

Der Beispielcode beschreibt einmal eine Produkt Detail Seite mit einem Produkt und eine Listing Seite mit einem Claim und einer Reihe von Produkten. (Die Struktur, h1 h2 .., ist hier vom SEO so vorgegeben, sorry)

~~~ html
<!-- products list page -->
<div id="products-page">
  <h1>Kaufe coole Klamotten Online</h1>

<h2>Jeans 501</h2>
  <p>.....</p>

  <h2>Hemd Hawaii</h2>
  <p>.....</p>
</div>

<!-- product detail page -->
<div id="product-page">
  <h1>Jeans 501</h1>
  <h2>Die coolste Hose.</h2>
  <p>.....</p>
  <img src=".." alt="Jeans 501" />
</div>
~~~
    
~~~ css
#product-page h1
#products-page h2 {
  font-size: 24px;
  color: #223322;
}
~~~

Die Gestaltung ist mit dem HTML Markup (h1 und h2) verknüpft. h1 und h2 haben aber, je nach dem innerhalb welchem Kontext sie benutzt werden, eine unterschiedliche Semantik. Auf der Produnkt Listing Seite ist die Semantik des h1 Elements "Claim", auf der Produkt Detail Seite "Product Headline". Aus CSS Sicht ist die Semantik nicht erkennbar.

Die vorgeschlagene Lösung besteht jetzt darin, die Gestaltung nicht an HTML Elemente, sondern ausschließlich an Klassen zu binden, womit man gleich wieder bei den so schlimmen beschreibenden Klassennamen landet, die dann unvermeidlich werden.

> Only include a selector that includes semantics. A span or div holds none. A heading has some. A class defined on an element has plenty.

~~~ html
<!-- products list page -->
<div id="content">
  <h1>Kaufe coole Klamotten Online</h1>

  <h2 class="product-headline">Jeans 501</h2>
  <p>.....</p>

  <h2 class="product-headline">Hemd Hawaii</h2>
  <p>.....</p>
</div>

<!-- product detail page -->
<div id="content">
  <h1 class="product-headline">Jeans 501</h1>
  <h2>Die coolste Hose.</h2>
  <p>.....</p>
  <img src="" />
</div>
~~~

~~~ css
.product-headline {
  font-size: 24px;
  color: #223322;
}
~~~

Im Sinne von HTML als API haben wir jetzt eine klare Schnittstelle realisiert über Klassen. Im CSS wird jetzt 
beschrieben, wie eine Produkt Headline auszusehen hat. Das ist etwas anderes als zu beschreiben, wie ein h1 auszusehen hat, wenn es die Semantik Produkt headline hat.

Jetzt ist es so, dass Klassen in unterschiedlichen Bereichen anders aussehen. Auf der Produkt Detail Seite 
sollte die Schrift z.B. größer sein als auf der Listing Seite. 

> When we have the same module in different sections, the first instinct is to use a parent element to style that module differently.

~~~ css
.product-headline {
  font-size: 24px;
  color: #223322;
}

#product-page .product-headline {
  font-size: 36px;
}
~~~

Wieder eine Best practice, die es zu überprüfen gilt. Nach einem Jahr, in dem vielleicht auch noch mehrere Entwickler an den Seiten gearbeitet haben, sieht die CSS Datei wahrscheinlich so aus:

~~~ css
#product-page .product-headline,
#home-page .product-headline,
#product-of-the-month .product-headline,
#mixed-page .single-product .product-headline,
#featured-page .product-headline {
  font-size: 36px;
}
~~~

Alles andere als übersichtlich.

> The problem with this approach is that you can run into specificity issues that require adding even more selectors to battle against it or to quickly fall back to using !important.

Wir haben hier wieder eine enge Bindung der Gestaltung an die Position im HTML, was laut SMACCS zu vermeiden ist.  Dieser CSS Code ist jetzt davon abhängig, dass sich das HTML Markup nicht mehr ändert.

Aus objektorientierter Sicht ist der Sachverhalt ganz einfach. Wir haben eine Klasse product-headline und eine weitere Klasse, die sich leicht unterscheidet, also "eine Art" product-headline. Die neue Klasse erbt alle Features der Basisklasse und fügt ihre eigenen hinzu. Genau wie die Basisklasse bekommt die abgeleitete Klasse einen beschreibenden Namen.

Hier wird diese neue Klasse aber über Selektoren beschrieben, nicht über einen Klassennamen.

Softwaretechnisch, wieder unter OO Gesichtspunkten betrachtet ist das ein ziemlicher Unsinn. Das wäre in etwa so, als ob man das Verhalten eines Objektes abhängig davon machen würde, in welcher Datei es vorkommt. Also in Datei a anders als in Datei b und am Anfang der Datei anders als an deren Ende.

> Try to avoid conditional styling based on location. If you are changing the look of a module for usage elsewhere on the page or site, sub-class the module instead.

Was ist also das Spezielle an dieser Headline? Da das Produkt an einer hervorgehobenen Stelle angezeigt wird, soll ein größerer Font genommen werden.


~~~ css
.product-headline {
  font-size: 24px;
  color: #223322;
}

.product-headline--big {
  font-size: 36px;
}
~~~

~~~ html
<!-- product detail page -->
<div id="product-page">
  <h1 class="product-headline product-headline--big">Jeans 501</h1>
  <h2>Die cooleste Hose</h2>
  <p>.....</p>
  <img src=".." alt="Jeans 501" />
</div>
~~~
    
Natürlich ist die Beschreibung hier auch semantisch und lautet nicht etwa: product-headline--36px. CSS ist keine OO Sprache. Man muss sich also mit Konstrukten wie `class="product-headline product-headline--big` helfen. Namenskonventionen können die Arbeit weiter vereinfachen. Preprozessoren wie SASS und LESS haben eigene Funktionalitäten, um Subklassen zu definieren (@extend).

Dieser Ansatz wird z.B. im [Twitter Bootstrap](http://twitter.github.com/bootstrap/components.html)  sowie in den Google Produkten angewendet.

Im Sinn von SMACCS sind diese unabhängigen Klassen Module. Das Besondere an ihnen ist, das sie an beliebiger Stelle im Markup benutzt werden können. Das bedeutet auch, dass sie ohne den Kontext einer (Projekt)spezifischen Seite entwickelt werden können. Das erlaubt ein ganz anderes Herangehen bei der Entwicklung solcher Module und bietet ganz neue Möglichkeiten. Ein Beispiel ist das Tool [KSS](https://github.com/kneath/kss/blob/master/SPEC.md) für die automatische Generierung von Dokumentation und Styleguides von Modulen.

Weitere Links zum Thema:

Nicolas Gallaghter: [About HTML semantics and frontend architecture](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/)

Nicole Sullivan: [Our (CSS) best practices are killing us](http://www.stubbornella.org/content/2011/04/28/our-best-practices-are-killing-us/)