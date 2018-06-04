# Senzill

**Senzill** és un projecte, basat amb [Jekyll](https://jekyllrb.com/), destinat a facilitar el desenvolupament de llocs webs estàtics o [JAMstack](https://jamstack.org/).

# Instal·lació

## Descarregar

Podem descarregar el projecte fent-ne ús de [git](https://git-scm.com/), mitjançant la _shell_:

```
$ git clone https://github.com/rogerforner/senzill.git
```

Si es desitja un nom de directori diferent al del repositori (senzill):

```
$ git clone https://github.com/rogerforner/senzill.git nomNou
```

## npm

Un cop descarregat hem d'executar [npm](https://www.npmjs.com/) ja que són necessaris alguns paquets

```
$ npm install
```

S'instal·laran els següents paquets (_package.json_):

```json
"devDependencies": {
    "axios": "^0.18.0",
    "moment": "^2.22.2",
    "vue": "^2.5.16"
},
"dependencies": {
    "gulp": "^4.0.0",
    "gulp-autoprefixer": "^5.0.0",
    "gulp-clean-css": "^3.9.3",
    "gulp-concat": "^2.6.1",
    "gulp-htmlmin": "^4.0.0",
    "gulp-sass": "^4.0.1",
    "gulp-shell": "^0.6.5",
    "gulp-uglify": "^3.0.0"
}
```

# Execució

Jekyll disposa de les seves pròpies comandes per dur a terme la compilació de _SASS_ i els fitxers que en componguin el projecte. Ara bé, en el nostre cas no en farem un ús directe d'aquestes, emprarem [gulp](https://gulpjs.com/).

**$ gulp**

Podem veure la configuració de gulp en el fitxer de l'arrel _gulpfile.js_.

Executant, en la _shell_, **gulp** (sense watch) compilarem la totalitat del lloc web, quedant preparat per ser "consumit".

```
$ gulp
```

**$ gulp watch**

Executant, en la _shell_, **gulp watch** també es compilarà el lloc web, però amb la diferència de que quan un fitxer sigui modificat, segons el tipus d'aquests, es durà endavant una compilació automàtica.

```
$ gulp watch
```

S'ha de tenir en compte que mitjançant la comanda **jekyll build** no s'executa cap tipus de servidor, de fet, només es compilen els fitxers que formen el nostre lloc web, sent enviats, desprès de la compilació, al directori **/_site.**

Dit això, hem de tenir present que l'execució del lloc web es durà a terme directament amb un **servidor local**, és indiferent si es tracta d'un [Laragon](https://laragon.org/) o un [XAMPP](https://www.apachefriends.org/es/index.html) o un [Wamp](http://www.wampserver.com/en/) o un [LAMP](https://es.wikipedia.org/wiki/LAMP) etc.

Tenint en compte que el directori on es compila el lloc web és /_site, la nostra URL del localhost es visualitzarà (o similar):

```
http://localhost/nomProjecte/_site/ca
http://senzill.oo/_site/ca
```

**/ca** fa referència a l'idioma, per defecte, al qual s'hagi configurat el projecte. A aquest directori s'arriba de forma automàtica, mitjançant una redirecció escrita amb JavaScript, aquesta situada al _index.html_ de /_site.

```html
<script type="text/javascript">
window.location.href = "{{- site.url | append:site.baseurl -}}/{{- site.t.default_lang -}}";
</script>
```

:bulb: **Idea**

Amb aquest fitxer es podria jugar. Per exemple, ampliant el temps d'espera abans de que es dugui a terme la redirecció, proporcionant el temps suficient com per convertir el fitxer index.html amb una pantalla d'inici de càrrega o introductoria.

- Amb un Spinner.
- Amb un vídeo.
- Etc.

## Compilació

A través de la compilació es duen a terme "dos" processos, un corresponent al **Gulp** i un altre al **Jekyll**.

**Gulp**:

- Els fitxers **SASS** són compilats a un fitxer _CSS_.
- Els fitxers **CSS** són concatenats i minificats.
- Els fitxers **JS** són concatenats i minificats.
- Els fitxers **HTML** són minificats.

**Jekyll**:

- Els fitxers **YML** són decodificats i emprats per permetre'n la correcta compilació del lloc web, aquest dividit en diversos directoris /_nomDirectori.

# Configuració

## _config.yml

Aquest és el **principal fitxer de configuració** de Jekyll, és emprat per determinar els directoris que seran inclosos, o no, en la compilació del lloc web, la URL principal d'aquest, l'idioma per defecte i els que en faran del lloc web un lloc multi idioma, també els tipus de publicacions que s'hagin d'emprar per generar contingut, entre altres apartats que poden ser ampliats segons la necessitat.

### Tractament de fitxers i directoris

> Predeterminat de Jekyll.

La variable _exclude_ és emprada per definir els fitxers i directoris que seran **exclosos** en la compilació del lloc web.

```yml
exclude:
  # SRC
  - src
  # Informació
  - LICENCE.md
  - README.md
  # Gestors de paquets
  ## Bundle
  - Gemfile
  - Gemfile.lock
  ## Node (npm)
  - node_modules
  - package.json
  - package-lock.json
  # Gulp (task runner)
  - gulpfile.js
```

### URL i baseurl

> Predeterminat de Jekyll.

Mitjançant la la variable _url_ es defineix quina és la URL principal del lloc web mentre que mitjançant la variable _baseurl_ es defineix quin és el directori (arrel) del lloc web.

És a dir, el nostre projecte es pot trobar situat en l'arrel on apunta el nostre domini:

```yml
> https://rogerforner.com

url: "https://rogerforner.com"
baseurl: ""
```

O bé es pot trobar situat en l'arrel d'un directori on apunti el nostre domini:

```yml
> https://rogerforner.com/ilercapp

url: "https://rogerforner.com"
baseurl: "ilercapp"
```

### Idiomes

> No predeterminat de Jekyll. Afegit per nosaltres.

Mitjançant l'array _langs_ es defineixen els idiomes que conformaran el lloc web.

```yml
langs:
  - "ca"
  - "es"
```

Tots els idiomes del lloc web hi seran referenciats a través de la URL, de tal manera que es visualitzaran com si es tractessin d'un directori en el mateix domini.

```
https://rogerforner.com/ca
https://rogerforner.com/es

https://rogerforner.com/ilercapp/ca
https://rogerforner.com/ilercapp/es
```

### Traduccions

> No predeterminat de Jekyll. Afegit per nosaltres.

Les traduccions ens seran útils per traduir aquell text que es mantingui estàtic en el lloc web, és a dir, que no presenti canvis de forma constant.

En l'exemple següent es pot veure la traducció de les meta etiquetes i peu de pàgina (_site_info_), també les de geolocalització (_geo_tag_) i les de _Open Graph_.

```yml
t:
  default_lang: "ca"
  ca:
    site_info:
      name: "Senzill"
      motto: "Per Roger Forner Fabre"
      description: "Senzill és un projecte, basat en Jekyll, destinat a facilitar el desenvolupament de llocs webs estàtics o JAMstack."
      locale: "ca-es"
      geo_tag:
        region: "ES-CT"
        placename: "Tortosa"
        position: "40.812578;0.521442"
        icbm: "40.812578, 0.521442"
    open_graph:
      facebook:
        app_id:
        publisher: #https://www.facebook.com/compteLLocWeb
        author: #https://www.facebook.com/compteAutorRa
        img:
      google_plus:
        publisher: #https://plus.google.com/compteLLocWeb
        author: #https://plus.google.com/compteAutorRa
        img:
      twitter:
        publisher: #@compteLlocWeb
        author: "@rogerforner" #@compteAutorRa
        img:
  es:
    site_info:
      name: "Senzill"
      motto:
      description: "Senzill es un proyecto, basado en Jekyll, destinado a facilitar el desarrollo de aplicaciones estáticas o JAMstack."
      locale: "es-es"
      geo_tag:
        region:
        placename:
        position:
        icbm:
    open_graph:
      facebook:
        app_id:
        publisher: #https://www.facebook.com/compteLLocWeb
        author: #https://www.facebook.com/compteAutorRa
        img:
      google_plus:
        publisher: #https://plus.google.com/compteLLocWeb
        author: #https://plus.google.com/compteAutorRa
        img:
      twitter:
        publisher: #@compteLlocWeb
        author: #@compteAutorRa
        img:
```

_Cada cas en pot requerir menys o més, poden ser afegides a voluntat._

Com es pot veure, les traduccions van introduïdes per l'array _t_, el qual és precedit per un seguit d'arrays. Cadascun d'aquests s'inicia amb el text que en defineix l'idioma al qual es correspondrà cada traducció, idioma establert mitjançant la variable _langs_. I també es pot veure la variable _default_lang_, la qual en defineix l'idioma per defecte, el principal, del lloc web.

```yml
t:
  default_lang: "ca"
  ca:
    ...
  es:
    ...
```

### Col·leccions (tipus de publicacions)

> Predeterminat de Jekyll.

Les col·leccions són els tipus de contingut que es generarà en el lloc web i se'n poden afegir tantes com es desitgi.

```yml
collections:
  pages:
    output: true
  posts:
    output: true
  feed:
    output: true
```

#### Enllaços permanents

Els enllaços permanents són emprats per definir les URLs dels diversos tipus de contingut que s'hagin predefinit. És una manera de garantir-ne la visibilitat de les URLs d'una forma amigable, entendible, de cara als/les nostres usuaris/àries.

Per defecte deixem la variable _permalink_ de la següent manera:

```yml
permalink: "pretty"
```

Desprès en definirem les rutes per a cadascun dels tipus de contingut que tinguem i, **molt important**, per a cadascun dels idiomes.

```yml
defaults:
  - # Pàgines
    scope:
      path: "_pages/ca"
    values:
      permalink: "/ca/:title/"
      lang: "ca"
      layout: "default"
  -
    scope:
      path: "_pages/es"
    values:
      permalink: "/es/:title/"
      lang: "es"
      layout: "default"
  - # Publicacions
    scope:
      path: "_posts/ca"
    values:
      permalink: "/ca/posts/:title/"
      lang: "ca"
      layout: "default"
  -
    scope:
      path: "_posts/es"
    values:
      permalink: "/es/posts/:title/"
      lang: "es"
      layout: "default"
  - # etc.
```

### Service worker

> No predeterminat de Jekyll. Afegit per nosaltres.

S'han declarat les variables que seran emprades per configurar el fitxer _sw.js_.

```yml
pwa:
  manifest:
    name: "Senzill"
    short_name: "Senzill"
    theme_color: "#0b1728"
    background_color: "#afc6e9"
  sw_cache:
    - "/"
    - "/assets/"
```

### Termes legals

> No predeterminat de Jekyll. Afegit per nosaltres.

S'han declarat variables que seran emprades per emplenar el _Copyright_ del peu de pàgina, també el nom del/la creador/ra del lloc web, així com per emplenar alguns espais de l'avís legal.

```yml
legal:
  owner:
    name: "Roger Forner Fabre"
    id: "DNI/NIF/..."
    domicile: "Domicili"
    email: "hello@rogerforner.com"
    domain: "rogerforner.com"
  developer:
    name: "IlercApp"
    url: "https://rogerforner.com/ilercapp"
```

## _config-prod.yml

Es tracta d'un fitxer de configuració que té exactament la mateixa funcionalitat que el fitxer _config.yml_, però amb la diferència de que en aquest hi afegiríem tota aquella configuració que **només** volem que estigui activa en un entorn de producció i no en un de desenvolupament.

Per exemple, si estem realitzant canvis en el nostre projecte, no és desitjable que Google Analytics estigui en funcionament en local. Mitjançant aquest fitxer de configuració això és possible ja que disposem de la possibilitat de compilar el lloc web sense la necessitat de que es compilin les meta etiquetes que de Google.

_Aquest fitxer segueix sent tan escalable com l'altre, és a dir, es pot ampliar segons la necessitat_.

```yml
# Bing Webmaster Center: http://www.bing.com/webmaster
# Google Search Console: https://www.google.com/webmasters/verification/home?hl=en
# Pinterest Site Verification: http://pinterest.com/
# Yandex Webmaster: https://webmaster.yandex.com/
site_verification:
  bing:
  google:
  pinterest:
  yandex:

# google_analytics: https://www.google.com/analytics/web/#home/
analytics:
  google_analytics: #ID de seguiment (UA-000000-2).

# etc.
```

**Per evitar la compilació** d'aquest fitxer i d'altres que es puguin crear per necessitat, és recomanable comentar la línia **99** del fitxer _gulpfile.js_.

En el cas de que es desitgi emprar, només s'haurà de comentar la línia **98** i _descomentar la 99_.

```js
 97 gulp.task('build', shell.task([
 98   'jekyll build'
 99   // 'jekyll build --config _config.yml,_config-prod.yml'
100 ]));
```