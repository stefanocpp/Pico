---
Title: Benvenuti
description: Pico è un CMS stupidamente semplice, incredibilmente veloce e basato su files di testo.
---

## Benvenuti su Pico

Congratulazioni, hai installato con successo [Pico][] %version%.  
%meta.description% <!-- replaced by the above Description header -->

## Creare contenuti

Pico è un CMS basato su files. Questo significa che non c'è un backend di amministrazione o un database di cui occuparsi. È sufficiente creare i file `.md` nella cartella `content` e quei file diventano le tue pagine. Per esempio, questo file si chiama `index.md` e viene mostrata come pagina di inizio del sito.

Quando si installa, Pico viene fornito con alcuni contenuti di esempio che verranno visualizzati fino a quando non si aggiungono i propri contenuti. Aggiungete semplicemente alcuni file `.md` nella vostra cartella `content` nella cartella principale di Pico e il gioco è fatto. Non è richiesta alcuna configurazione, Pico utilizzerà automaticamente la cartella `content` non appena si crea il proprio file `index.md`. Dai un'occhiata ai [Contenuti campione di Pico][SampleContents] per trovare un file di esempio!

Se si crea una cartella all'interno della directory di contenuto (ad es. `content/sub`) e si mette un `index.md` all'interno di esso, è possibile accedere a quella cartella all'URL `%base_url%?sub`. Se si desidera aggiungere una pagina all'interno della sottocartella, è sufficiente creare un file di testo .md e sarà possibile accedervi
(ad esempio `content/sub/pagina.md` è accessibile dall'URL `%base_url%?sub/pagina`).
Di seguito abbiamo mostrato alcuni esempi di posizioni e dei loro URL corrispondenti:

<table style="width: 100%; max-width: 40em;">
    <thead>
        <tr>
            <th style="width: 50%;">Posizione effettiva</th>
            <th style="width: 50%;">URL</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>content/index.md</td>
            <td><a href="%base_url%">/</a></td>
        </tr>
        <tr>
            <td>content/sub.md</td>
            <td><del>?sub</del> (non accessibile, vedi sotto)</td>
        </tr>
        <tr>
            <td>content/sub/index.md</td>
            <td><a href="%base_url%?sub">?sub</a> (come sopra)</td>
        </tr>
        <tr>
            <td>content/sub/page.md</td>
            <td><a href="%base_url%?sub/page">?sub/page</a></td>
        </tr>
        <tr>
            <td>content/a/very/long/url.md</td>
            <td>
              <a href="%base_url%?a/very/long/url">?a/very/long/url</a>
              (non esiste)
            </td>
        </tr>
    </tbody>
</table>

Se un file non può essere trovato, il file `content/404.md` sarà mostrato. È possibile aggiungere un file `404.md` a qualsiasi directory. Così, per esempio, se si vuole usare una pagina di errore personalizzata per il tuo blog, puoi semplicemente creare `content/blog/404.md`.

Pico separa rigorosamente i contenuti del tuo sito web (i file Markdown nel tuo sito web nella cartella `content`) e come questi contenuti dovrebbero essere visualizzati (i files Twig nella cartella "themes"). Tuttavia, non tutti i file nel vostro `content` potrebbe essere in realtà una pagina distinta. Per esempio, alcuni temi (tra cui il tema predefinito di Pico) usa qualche file speciale "nascosto" per gestire i metadati (come `_meta.md` nel contenuto del campione di Pico). Alcuni altri temi usano un `_footer.md` a rappresentano il contenuto del piè di pagina del sito web. Il punto comune è il `_`: tutti i files e le directory precedute da un `_` nella vostra directory `content` sono nascosti.
Queste pagine non sono accessibili da un browser web, Pico mostrerà un errore 404 al loro posto.

Come pratica comune, vi raccomandiamo di separare i vostri contenuti e le risorse (come immagini, download, ecc.). Neghiamo addirittura l'accesso alla vostra directory `content` di default. Se si desidera utilizzare alcune risorse (ad esempio un'immagine) in uno dei propri contenuti
si dovrebbe creare una cartella `assets` nella root directory di Pico e caricare lì le risorse. È quindi possibile accedervi dal codice Markdown utilizzando <code>&#37;base_url&#37;/assets/</code> per esempio:
<code>!\[Titolo immagine\](&#37;base_url&#37;/assets/image.png)</code>

### Sintassi dei files di testo

I file di testo sono scritti utilizzando [Markdown][] e [Markdown Extra][MarkdownExtra].
Possono anche contenere HTML normale.

Nella parte superiore dei file di testo è possibile inserire un blocco commento e specificare alcuni attributi meta della pagina utilizzando [YAML][] ("YAML header"). Per esempio:

    ---
    Title: Benvenuti
    Description: Questa descrizione finirà nel tag meta description
    Author: Joe Bloggs
    Date: 2001-04-25
    Robots: noindex,nofollow
    Template: index
    ---

Questi valori saranno contenuti nella variabile `{{ meta }}` nei temi (vedere sotto). Le intestazioni meta a volte hanno un significato speciale: Per esempio, Pico non legge solo il meta header "Date", ma lo interpreta in modo da "capire" realmente quando questa pagina è stata creata. Questo entra in gioco quando si vuole ordinare le pagine non solo in ordine alfabetico, ma anche per data. Un altro esempio è il
meta header `Template`: esso determina quale sia il template di Twig che Pico usa per visualizzare questa pagina (ad esempio, se si aggiunge `Template: blog`, Pico usa `blog.twig`).

Nel tentativo di separare i contenuti e lo stile, si consiglia di non usare CSS inline nei tuoi file Markdown. Dovresti piuttosto aggiungere appropriate classi CSS al tuo tema per stabilire quanto spazio abbia a disposizione un'immagine (e.g. `img.small { width: 80%; }`). Puoi poi usare queste classi CSS nei tuoi files Markdown, per esempio: <code>!\[Image Title\](&#37;base_url&#37;/assets/image.png) {.small}</code>

Ci sono anche alcune variabili che puoi usare nei tuoi files di testo:

* <code>&#37;site_title&#37;</code> - Il titolo del tuo sito con Pico
* <code>&#37;base_url&#37;</code> - L'URL del tuo sito Pico; i link interni possono essere specificati usando <code>&#37;base_url&#37;?sub/page</code>
* <code>&#37;theme_url&#37;</code> - L'URL al tema correntemente in uso
* <code>&#37;version&#37;</code> - La versione di Pico attuale (e.g. `2.0.0`)
* <code>&#37;meta.&#42;&#37;</code> - Accedi a qualunque variabile della pagina corrente, es <code>&#37;meta.author&#37;</code> viene sostituito con `Joe Bloggs`

### Blog

Pico non è un software di blogging - ma è molto semplice usarlo per gestire un blog. È possibile trovare molti plugin là fuori che implementano le tipiche caratteristiche di un blog come autenticazione, tagging, impaginazione e plugin sociali. Vedere sotto la sezione Plugins per i dettagli.

Se vuoi usare Pico come software di blogging, probabilmente vuoi fare
qualcosa del genere:

1. Mettere tutti i tuoi articoli del blog in una cartella separata `blog` nella cartella `content`. Tutti questi articoli dovrebbero avere una meta-intestazione "Data".
2. Creare un file `blog.md` o `blog/index.md` nella vostra directory `contenuto`. Aggiungere `Template: blog-index` all'intestazione YAML di questa pagina. Questo servirà a mostrare un elenco di tutti gli articoli del tuo blog (vedi passo 3).
3. Creare il nuovo modello di Twig `blog-index.twig` (il nome del file deve corrispondere a quello di `Template: blog-index` del punto 2) nella vostra cartella di temi. Questo modello probabilmente non è molto diverso dal vostro `index.twig` predefinito (cioè puoi copiare
   `index.twig`) e creerà una lista di tutti i tuoi articoli del blog. Aggiungere il seguente snippet Twig a `blog-index.twig` vicino a `{{ content }}`:
   ```
    {% for page in pages|sort_by("time")|reverse %}
        {% if page.id starts with "blog/" and not page.hidden %}
            <div class="post">
                <h3><a href="{{ page.url }}">{{ page.title }}</a></h3>
                <p class="date">{{ page.date_formatted }}</p>
                <p class="excerpt">{{ page.description }}</p>
            </div>
        {% endif %}
    {% endfor %}
     ```    


## Personalizzazione

Pico è altamente personalizzabile in due modi diversi: Da un lato è possibile cambiare l'aspetto di Pico utilizzando i temi, dall'altro lato è possibile aggiungere nuove funzionalità utilizzando i plugin. Fare il primo include la modifica dell'HTML, CSS e JavaScript di Pico, il secondo consiste principalmente nella programmazione PHP.

Questo è tutto arabo per te? Non preoccupatevi, non devi perdere tempo su questi discorsi tecnici - è molto facile usare uno dei magnifici temi o plugin che altri hanno sviluppato e rilasciato in pubblico. Si prega di fare riferimento alle sezioni successive per i dettagli.

### Temi

È possibile creare temi per l'installazione di Pico nella cartella `themes`. Controlla il tema predefinito per un esempio. Pico usa [Twig][] per il rendering dei template. È possibile selezionare il tema impostando l'opzione `theme` in `config/config.yml` al nome della cartella del tema.

Tutti i temi devono includere un file `index.twig` per definire la struttura HTML del tema. Sotto ci sono le variabili Twig che sono disponibili per l'uso nel tuo tema. Si prega di notare che i percorsi (es. `{{base_dir }}`) e gli URL (es. `{{base_url }}`) non hanno un trailing slash.

* `{{ site_title }}` - Scorciatoia al titolo del sito (see `config/config.yml`)
* `{{ config }}` - Contiene i valori impostati in `config/config.yml`
                   (e.g. `{{ config.theme }}` diventa `default`)
* `{{ base_dir }}` - Il percorso alla directory root di Pico
* `{{ base_url }}` - l'URL del tuo sito Pico; usa il filtro `link` di Twig per
                     specificare i link interni (e.g. `{{ "sub/page"|link }}`),
                     questo garantisce che i tuoi link funzionino dia che l'URL rewriting sia abilitato o meno
* `{{ theme_dir }}` - il percorso al tema correntemente in uso
* `{{ theme_url }}` - L'URL al tema correntemente in uso
* `{{ version }}` - La versione corrente di Pico (e.g. `2.0.0`)
* `{{ meta }}` - Contiene i valori meta della pagina corrente
    * `{{ meta.title }}`
    * `{{ meta.description }}`
    * `{{ meta.author }}`
    * `{{ meta.date }}`
    * `{{ meta.date_formatted }}`
    * `{{ meta.time }}`
    * `{{ meta.robots }}`
    * ...
* `{{ content }}` - Il contenuto della pagina corrente dopo che è stato processato da Markdown
* `{{ pages }}` - Una collezione di tutte le pagine del tuo sito
    * `{{ page.id }}` - Il percorso relativo al file dei contenuti (unique ID)
    * `{{ page.url }}` - L'URL alla pagina
    * `{{ page.title }}` - Il titolo della pagina (intestazione YAML)
    * `{{ page.description }}` - La descrizione della pagina (intestazione YAML)
    * `{{ page.author }}` - L'autore della pagina (intestazione YAML)
    * `{{ page.time }}` - Il [timestamp Unix][UnixTimestamp] derivato dall'intestazione `Date`
    * `{{ page.date }}` - La data della pagina (intestazione YAML)
    * `{{ page.date_formatted }}` - La data della pagina formattata secondo quanto specificato dal parametro `date_format` nel file `config/config.yml`
    * `{{ page.raw_content }}` - Il contenuto grezzo della pagina prima che sia interpretato; usa il filtro di Twig `content` per ottenere il contenuto interpretato della pagina fornendo il suo ID (e.g. `{{ "sub/page"|content }}`)
    * `{{ page.meta }}`- I valori meta della pagina (vedi `{{ meta }}` più sopra)
    * `{{ page.previous_page }}` - I dati della pagina rispettivamente precedente
    * `{{ page.next_page }}` - I dati della pagina rispettivamente successiva
    * `{{ page.tree_node }}` - Il nodo della pagina nel page tree di Pico
* `{{ prev_page }}` - I dati della pagina precedente (rispetto a `current_page`)
* `{{ current_page }}` - I dati della pagina corrente (vedi sopra `{{ pages }}`)
* `{{ next_page }}` - I dati della pagina successiva (rispetto a `current_page`)

Le pagine possono essere usate come nell'esempio seguente:

    <ul class="nav">
        {% for page in pages if not page.hidden %}
            <li><a href="{{ page.url }}">{{ page.title }}</a></li>
        {% endfor %}
    </ul>

Oltre a utilizzare l'elenco `{{ pages }}`, è anche possibile accedere alle pagine utilizzando l'albero delle pagine di Pico (page tree). L'albero delle pagine consente di iterare attraverso le pagine di Pico usando una struttura ad albero, in modo da poter ad esempio iterare solo i figli diretti di una pagina. Permette di creare menu ricorsivi (come i menu a tendina) e di filtrare le pagine più facilmente. Basta andare alla [documentazione dell'albero delle pagine][FeaturesPageTree] di Pico per i dettagli.

Per chiamare le risorse dal proprio tema, usare `{{ theme_url }}`. Per esempio, per includere il file CSS `themes/my_theme/example.css`, aggiungere
`<link rel="stylesheet" href="{{ theme_url }}/example.css" type="text/css" />` to your `index.twig`. Questo funziona per i file arbitrari nella cartella del tema, incluse immagini e file JavaScript.

Oltre all'ampia lista di filtri, funzioni e tags di Twigs, Pico fornisce anche alcuni utili filtri aggiuntivi per rendere più facile la gestione dei temi.

* Passa l'ID di una pagina al filtro `link` per restituire il link alla pagina
  (e.g. `{{ "sub/page"|link }}` ottiene `%base_url%?sub/page`).
* Per ottenere il contenuto interpretato di una pagina, passa il suo ID al filtro `content` (e.g. `{{ "sub/page"|content }}`).
* Puoi interpretare qualsiasi stringa Markdown usando il filtro `markdown` (e.g. puoi usare Markdown nella meta variabile `description`  e interpretarlo poi nel tema usando `{{ meta.description|markdown }}`). Puoi usare i dati nel meta come parametri segnaposto da sostituire <code>&#37;meta.&#42;&#37;</code>  (e.g.
  `{{ "Scritto *da %meta.author%*"|markdown(meta) }}` restituisce "Scritto *da John Doe*").
* Collezioni (array) possono essere ordinate secondo un criterio usando il filtro `sort_by` (e.g. `{% for page in pages|sort_by([ 'meta', 'nav' ]) %}...{% endfor %}` itera attraverso tutte le pagine, ordinandole econdo il meta header `nav` ; prego notare che la parte `[ 'meta', 'nav' ]` dell'esempio istruisce Pico a ordinare secondo `page.meta.nav`. Elementi che non possono essere ordinati dal filtro vengono messi in fondo all'elenco; è possibile specificare `bottom` (mette elementi in fondo; default), `top` (mette elementi in cima), `keep` (mantiene l'ordine originale) or `remove` (rimuove elementi) come secondo parametro per modificare il comportamento del filtro.
* Puoi ottenere tutti i valori di un parametro di una collezione (array) usando il filtro `map` (e.g. `{{ pages|map("title") }}` restituisce tutti i titoli di pagina).
* Usa le funzioni Twig `url_param` e `form_param` per accedere ai parametri HTTP GET (i.e. la query string di un URL tipo `?some-variable=my-value`) e HTTP POST (i.e. i dati di un form). Questo permette di implementare cose come paginazione, tags e categorie, pagine dinamiche e molto più - usando solo puro Twig! Guarda la nostra [pagina introduttiva per accedere ai parametri HTTP][FeaturesHttpParams] per ulteriori dettagli.

### Plugin

#### Plugin per utenti

I plugin ufficialmente testati possono essere trovati su http://picocms.org/plugins/, ma ci sono moltissimi plugin di terze parti! Un buon punto di partenza per trovarli è [la nostra Wiki][WikiPlugins].

Pico rende molto facile aggiungere nuove funzionalità al tuo sito web usando i plugin. Proprio come Pico, è possibile installare i plugin sia usando [Composer][] (ad esempio `composer require phrozenbyte/pico-file-prefixes`), sia caricando manualmente il file del plugin (solo per piccoli plugin costituiti da un singolo file, ad esempio `PicoFilePrefixes.php`) o la directory (ad esempio `PicoFilePrefixes`) nella directory `plugins`. Vi raccomandiamo sempre di usare Composer ogni volta che è possibile, perché rende l'aggiornamento di Pico e dei  plugin installati più facile. In ogni caso, a seconda del plugin che si desidera installare, potrebbe essere necessario compiere alcuni passi in più (ad esempio specificando le variabili di configurazione) per far funzionare il plugin. Per questa ragione dovresti sempre controllare i documenti del plugin o il file `README.md` per imparare i passi necessari.

I plugin che sono stati scritti per funzionare con Pico 1.0 e successivi possono essere abilitati e disabilitati attraverso il file `config/config.yml`. Se si desidera, ad esempio, disabilitare il plugin `PicoDeprecated`, aggiungere la seguente riga al file `config/config.yml`: `PicoDeprecated.enabled: false`. Per forzare il plugin ad essere abilitato, sostituire `false` con `true`.

#### Plugins per sviluppatori

Sei uno sviluppatore di plugin? Vi vogliamo bene ragazzi! Potete trovare un sacco di informazioni su come sviluppare i plugin su http://picocms.org/development/. Se avete già sviluppato un plugin in precedenza e volete aggiornarlo a Pico 2.0, fate riferimento alla [sezione di aggiornamento nei documenti][PluginUpgrade].

## Config

Configurare Pico è davvero stupidamente semplice: Basta creare un file `config/config.yml` per sovrascrivere le impostazioni predefinite di Pico (e aggiungere le proprie impostazioni personalizzate). Date un'occhiata al template `config/config.yml.template` per una breve panoramica delle impostazioni disponibili e dei loro valori predefiniti. Per sovrascrivere un'impostazione, copiare semplicemente la riga da `config/config.yml.template` a `config/config.yml` e impostare il valore personalizzato.

Ma non ci siamo fermati qui. Piuttosto che avere solo un singolo file di configurazione, è possibile utilizzare un numero arbitrario di file di configurazione. Creare semplicemente un file `.yml` nella cartella `config` di Pico e siete pronti a partire. Questo permette di aggiungere struttura alla vostra configurazione, come - per esempio - un file di configurazione separato per il tuo tema (`config/my_theme.yml`).

Si prega di notare che Pico carica i file di configurazione in un modo speciale di cui dovreste essere consapevoli. Prima di tutto carica il file di configurazione principale `config/config.yml`, e poi qualsiasi altro file `*.yml` in ordine alfabetico. L'ordine dei file è cruciale: I valori di configurazione già impostati non possono essere sovrascritti da un file successivo. Per esempio, se si imposta `site_title: Pico` in `config/a.yml` e `site_title: Il mio sito impressionante `in `config/b.yml`, il tuo sito si chiamerà "Pico".

Dato che i file YAML sono file di testo, gli utenti possono leggere le tue configurazioni di Pico navigando su `%base_url%/config/config.yml`. Questo non è un problema in primo luogo, ma potrebbe diventare un problema se si utilizzano plugin che richiedono di memorizzare dati rilevanti per la sicurezza nel file di configurazione (come le credenziali d'accesso). Quindi dovresti *sempre* assicurarti di configurare il tuo server web per negare l'accesso alla cartella `config` di Pico. Basta fare riferimento alla sezione "URL Rewriting" qui sotto. Seguendo le istruzioni, non solo si abilita la riscrittura dell'URL, ma si nega anche l'accesso alla cartella `config` di Pico.

### URL Rewriting

PicoGli URL che Pico usa di default (e.g. %base_url%/?sub/page) sono già molto user-friendly.
In aggiunta, Pico offre la funzione di URL rewrite per renderli ancora più user-friendly (e.g. %base_url%/sub/page). Qui sotto troverai alcune informazioni base su come configurare il tuo server a dovere per abilitare l'URL rewriting.

#### Apache

Se usi un server web Apache, l'URL rewriting potrebbe già essere abilitato - prova te stesso cliccando sul [secondo URL](%base_url%/sub/page). Se l'URL rewriting non funziona (cioè ottieni un errore `404 Non trovato` da Apache), abilita il [modulo `mod_rewrite`][ModRewrite] gli override in `.htaccess`. Potresti dover impostare la [directive `AllowOverride`][AllowOverride] a `AllowOverride All` nel file config del host virtuale nel `httpd.conf`/`apache.conf` globale. Assumendo che il reindirizzamento URL funzioni, ma che Pico non li usi, forza il URL
rewriting impostando `rewrite_url: true` nel tuo `config/config.yml`. Se invece ottieni un errore `500 Internal Server Error` a prescindere da quello che fai, prova a rimuovere la directive `Options` dal file `.htaccess` di Pico (è l'ultima linea).

#### Nginx

Se usi Nginx, puoi usare la configurazione seguente per abilitare l' URL rewriting (righe da `5` a `8`) e impedire l'accesso ai files interni di Pico (righe da `1` a `3`). Dovrai inoltre sistemare il percorso (`/pico` sulle righe `1`, `2`, `5` e `7`) perché corrisponda alla cartella d'installazione di Pico. Inoltre, dovrai abilitare l' URL
rewriting impostando `rewrite_url: true` nel tuo `config/config.yml`. La configurazione di Nginx dovrebbe fornirvi il *minimo essenziale* che serve per far girare Pico. Nginx è un argomento molto vasto. Per qualsiasi altra necessità, prego leggere i nostri [documenti di configurazione Nginx][NginxConfig].

```
location ~ ^/pico/((config|content|vendor|composer\.(json|lock|phar))(/|$)|(.+/)?\.(?!well-known(/|$))) {
    try_files /pico/index.php$is_args$args =404;
}

location /pico/ {
    index index.php;
    try_files $uri $uri/ /pico/index.php$is_args$args;
}
```

#### Lighttpd

Pico funziona tranquillamente su Lighttpd. Potete usare la configurazione seguente per abilitare l'URL rewriting (linee da `6` a `9`) e impedire l'accesso ai file interni di Pico (linee da `1` a `4`). Assicuratevi di modificare il percorso (`/pico` sulle linee `2`, `3` e `7`) affinché corrisponda alla cartella d'installazione, far sapere a Pico che l'URL rewriting è diponibile impostando `rewrite_url: true` in  `config/config.yml`. La configurazione qui sotto dovrebbe fornirvi il *minimo essenziale* che serve per far girare Pico.

```
url.rewrite-once = (
    "^/pico/(config|content|vendor|composer\.(json|lock|phar))(/|$)" => "/pico/index.php",
    "^/pico/(.+/)?\.(?!well-known(/|$))" => "/pico/index.php"
)

url.rewrite-if-not-file = (
    "^/pico(/|$)" => "/pico/index.php"
)
```

## Documentazione

Per maggiori informazioni, fare riferimento alla documentazione di Pico su http://picocms.org/docs.

[Pico]: http://picocms.org/
[SampleContents]: https://github.com/picocms/Pico/tree/master/content-sample
[Markdown]: http://daringfireball.net/projects/markdown/syntax
[MarkdownExtra]: https://michelf.ca/projects/php-markdown/extra/
[YAML]: https://en.wikipedia.org/wiki/YAML
[Twig]: http://twig.sensiolabs.org/documentation
[UnixTimestamp]: https://en.wikipedia.org/wiki/Unix_timestamp
[Composer]: https://getcomposer.org/
[FeaturesHttpParams]: http://picocms.org/in-depth/features/http-params/
[FeaturesPageTree]: http://picocms.org/in-depth/features/page-tree/
[WikiThemes]: https://github.com/picocms/Pico/wiki/Pico-Themes
[WikiPlugins]: https://github.com/picocms/Pico/wiki/Pico-Plugins
[OfficialThemes]: http://picocms.org/themes/
[PluginUpgrade]: http://picocms.org/development/#upgrade
[ModRewrite]: https://httpd.apache.org/docs/current/mod/mod_rewrite.html
[AllowOverride]: https://httpd.apache.org/docs/current/mod/core.html#allowoverride
[NginxConfig]: http://picocms.org/in-depth/nginx/