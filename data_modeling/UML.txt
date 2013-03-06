DATA MODELING
=============


Definizione delle entità
------------------------

posts: il contenuto vero e proprio del blog.
tags: elenco dei tags di un post.
categories: elenco delle categorie di un post.
comments: commenti scritti dagli utenti relativi ai post.
users: elenco di utenti che hanno scritto un commento o postato un nuovo articolo, identificati con un'autorizzazione.
countries: elenco di tutte le country di questo fantastico pianeta (per ora è solo una vaga idea)
statistics: statistiche del blog per ogni post.
preferences: una serie di preferenze del blog.

Di seguito elenco in maniera dettagliata quali attributi saranno presenti in ogni entità.

posts: id_post, id_user, title, short_description, content, creation_date, update_date, comments_enabled, status.
tags: id_tag, name.
categories: id_category, name.
comments: id_comment, id_user, id_post, comment, rating, creation_date, status.
users: id_user, name, surname, email, mobile_number, address, city, state, id_country, auth, website, status.
preferences: preference_name, value.
statistics: id_statistic, id_post, user_agent, referral, creation_date.

Entità generate dalle relazioni N-N.
post_tags: id_post, id_tag.
post_categories: id_post, id_category.


Definizione delle relazioni
---------------------------

Un utente può scrivere n post, mentre un post appartiene ad un solo utente, pertanto abbiamo una relazione 1-N.
Un post può avere associati più tags o categorie, questi/e possono comparire in più post diversi, pertanto stabiliamo due relazione N-N, da tale relazioni si creano due nuove entità, post_tags e post_categories.
Un post può avere uno o più commenti, mentre un commento è presente solo nel relativo post in cui è stato scritto, di conseguenza abbiamo una relazione 1-N.
Un utente può scrivere n commenti, mentre un commento è scritto da un solo utente, la relazione sarà 1-N.
L'entità preferences serve a memorizzare informazioni di vario genere del blog, non è in relazione con nessun altro oggetto.
Le statistiche sono generate a seguito della visualizzazione di un post, pertanto avranno una relazione 1-N con l'entità
posts.

UML
---

Work in progress here:
http://www.asciiflow.com/#Draw5607261392855616643/1589133572

                                      http://www.asciiflow.com

+------------------+
|    CATEGORIES    |                               ->             +-----------------------+
|------------------+                            GENERATE          |      STATISTICS       |
|                  |                                            N |-----------------------|
|  id_category{PK} |                   +------------+------------<|                       |
|  name            |                   |            |             |  id_statistic{PK}     |
|                  |                   |    +---------------+     |  id_post              |
+------------------+                   |    |               |     |  user_agent           |
         V N                           |    | creation_date |     |  referral             |
         |                             |    |               |     |                       |
         |                             |    +---------------+     +-----------------------+
         |                             |
         | 1                           |
+--------+----------+                1 |                          +-----------------------+
|   CATEGORY_POST   |        +---------+-----------+              |         USERS         |
|-------------------|        |        POSTS        |              |-----------------------|
|                   | N    1 |---------------------|              |                       |
|  id_post{PPK}     |>-------+                     |              |  id_user{PK}          |
|  id_category{PPK} |        |  id_post{PK}        | N          1 |  name                 |
|                   |        |  id_user            |>-------------+  surname              |
+-------------------+        |  title              |      <-      |  email                |
                             |  short_description  |   PUBLISHES  |  mobile_number        |
                             |  content            |              |  address              | N
+---------------+            |  creation_date      |              |  city                 |>--+
|   POST_TAG    |            |  update_date        |              |  state                |   |
|---------------|            |  enable_comments    |              |  id_country           |   |
|               | N        1 |  status             |              |  auth                 |   |
|  id_post{PPK} |>-----------+                     |              |  status               |   |
|  id_tag{PPK}  |            |                     |              |  website              |   |
|               |            +---------------------+              |                       |   |
+---------------+               V   N                             +-----------------------+   |
       V N                      |                                                             |
       |                        |                 +----------------------+                    |
       |                        |                 |       COMMENTS       |                    |
       |                        |                 |----------------------|                    |
       | 1                      |                 |                      |                    |
+------+-------+                |                 |  id_comment{PK}      |         <-         |
|     TAGS     |                |      ->         |  id_user             | 1     WRITES       |
|--------------|                |   CONTAINS    1 |  id_post             +--------------------+
|              |                +-----------------+  comment             |
|  id_tag{PK}  |                                  |  rating              |
|  name        |                                  |  creation_date       |
|              |                                  |  status              |
+--------------+                                  |                      |
                                                  +----------------------+
+----------------------+
|   BLOG_PREFERENCES   |
|----------------------|
|                      |
|  preference_name{PK} |
|  value               |
|                      |
+----------------------+

Logic Model
-----------

Le tabelle che saranno create sono elencate di seguito.

posts(id_post{PK}, id_user, title, short_description, content, creation_date, update_date, comments_enabled, status)
categories(id_category{PK}, name)
category_post(id_post{PPK}, id_category{PPK})
tags(id_tag{PK}, name)
post_tags(id_post{PPK}, id_tag{PPK})
users(id_user{PK}, name, surname, email, mobile_number, address, city, state, id_country, auth, website, status)
comments(id_comment{PK}, id_user, id_post, comment, rating, creation_date, status)
statistics(id_statistic{PK}, id_post, user_agent, referral, creation_date)
preferences(preference_name{PK}, value)

