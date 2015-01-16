--- 
title: Einfache Namenskonventionen für modulares SASS
date: 2014/09/20
tags: css, Klassen, OO
---

In den letzten Jahren habe ich für mich für mich eine recht simple Namenskonventionen für Modulares SASS entwickelt.

Als Einteilung gibt es nur Module. Elemente innerhalb eines Module werden mit zwei Minuszeichen getrennt. Falle es noch Unterelemente gibt werden diese mit einem Minuszeichen getrennt.

~~~ scss
.product {
    font-size: 24px;
    color: #223322;

    .product--list {
        background-color: #f1f1f1;

        .product--list-price {
           font-weight: 600;
        }
    }
}
~~~

Das Layout Module ist einfach ein normale Modul. Es beschreibt die grobe Struktur der Seite. Die weiteren Elemente werden in speziellen Modulen beschrieben. Das Layout Module benutzt ein Grid System und bildet deren Grid Klassen auf semantisches SASS ab.

~~~ scss
.layout--header {
    @include grid-row();

    .layout--header-logo {
        @include grid-column(4);
    }
    
    .layout--header-login {
      @include grid-column(8);
    }
}
~~~

Modifier stehen im dazugehörigen Modul und fangen mit is- oder has- an.

~~~ scss
.form--features {
    list-style-type: none;

    li {  
        background-color: $feature-off;

    &.is-on {
        background-color: $feature-on;
    }
  }
}
~~~

Für weitere Beispiele habe ich für die Desksurfing Seiten einen [Living Style Guide](http://desksurfing.net/assets/styleguide.html/) geschrieben.

Zusammenfassung: Mit diesen simplen Konventionen lässt sich gut wartbarer SASS Code schreiben. Auf Dateiebene hat jedes Modul einen eigenen Ordner. Für responsive Seiten gibt es für jeden Step eine eigene SASS Datei. Der zum Modul gehörige JavaScript Code kommt ebenfalls in eine Datei innerhalb der Modul Ordners. Verknüpft werden die JS Dateien mit [RequireJS]( http://requirejs.org) so das sich folgende Struktur ergibt:

    Module
      -> module_s.sass
      -> module_m.sass
      -> module_l.sass
      -> module.js