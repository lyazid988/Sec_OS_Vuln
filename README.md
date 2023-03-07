# HTTP Request Smuggling
## C'est quoi ?
On peut définir une attaque HTTP Request Smuggling comme celui qui, profitant des écarts dans l'analyse, lorsqu'un ou plusieurs périphériques / entités HTTP sont dans le flux de données entre l'utilisateur et le serveur Web. Nous rencontrons une technique qui interfère avec la façon dont un site Web traite les flux de requêtes HTTP reçus d'un ou plusieurs utilisateurs.

Ces vulnérabilités sont souvent de nature critique, permettant au cybercriminel de contourner les contrôles de sécurité, d'obtenir un accès non autorisé aux données privées et même de compromettre directement d'autres utilisateurs de l'application. Ce trafic illégal de requêtes HTTP permettra diverses attaques telles que l'empoisonnement du cache, le piratage de session et également la possibilité de contourner la protection par pare-feu des applications Web.

Cette attaque HTTP Request Smuggling peut affecter différents ordinateurs et services :

    Serveur Web.
    Pare-feu.
    Serveur proxy.

Quant à l'origine, elle remonte à 2005 lorsque cette attaque a été démontrée par un groupe de chercheurs de Watchfire, dont Klein, Ronen Heled, Chaim Linhart et Steve Orrin. Cependant, un certain nombre d'améliorations ont été apportées ces dernières années qui étendent la surface d'attaque et permettent un accès privilégié aux API internes et à l'empoisonnement du cache.

## Comment cette attaque se produit
Les applications Web d'aujourd'hui utilisent fréquemment des chaînes de serveur HTTP entre les utilisateurs et la logique d'application ultime. De cette façon, les utilisateurs envoient des demandes à un serveur frontal, puis le serveur frontal envoie les demandes à un ou plusieurs serveurs principaux. Il est à noter que cette architecture est de plus en plus fréquente et dans certains cas incontournable, comme les applications basées sur le cloud.

Lorsqu'un serveur frontal transfère des demandes HTTP à un serveur principal, il envoie généralement plusieurs demandes via la même connexion réseau principale. Ceci est fait parce qu'il est plus efficace et offre des performances plus élevées. La façon d'agir est simple, d'abord les requêtes HTTP sont envoyées les unes après les autres. Le serveur de réception analyse ensuite les en-têtes de chaque requête HTTP pour déterminer où se termine une requête et quand commence la suivante.

Un aspect très important est que le systèmes front-end et back-end doivent s'entendre sur les limites des demandes . Ceci est pertinent car, sinon, un cybercriminel pourrait envoyer une demande ambiguë aux systèmes frontaux et principaux et ils pourraient la comprendre différemment.

Ici, l'attaquant fait qu'une partie de sa requête frontale est interprétée par le serveur back-end comme le début de la requête suivante. Ce qui se passe alors, c'est qu'il précède la demande suivante, ce qui provoque des interférences avec la façon dont l'application traite cette demande. Nous rencontrerions une attaque HTTP Request Smuggling et cela peut avoir des résultats désastreux pour le propriétaire de ce serveur.

## Pourquoi ces attaques se produisent

Une grande partie des vulnérabilités HTTP Request Smuggling se produisent parce que la spécification HTTP fournit deux manières différentes de spécifier où se termine une requête. Un en-tête est simple, il spécifie la longueur du corps du message en octets. Cela peut être utilisé pour indiquer que le corps du message utilise un codage de bloc. Cela signifie que le corps de ce message contient une ou plusieurs données. Certains administrateurs de sécurité ne savent pas que le cryptage en bloc peut être utilisé dans les requêtes HTTP et c'est un problème.

Au cas où vous ne le sauriez pas, HTTP fournit deux méthodes différentes pour déterminer la longueur des messages HTTP. Il convient également de noter qu'il est possible pour un même message d'utiliser les deux méthodes en même temps, auquel cas elles entreraient en conflit. Le protocole HTTP tente de résoudre ce problème en indiquant que si les en-têtes Content-Length et Transfer-Encoding sont tous deux présents, alors l'en-tête Content-Length ne doit pas être utilisé. Cette formule peut être suffisante pour éviter toute ambiguïté lorsque nous n'avons qu'un seul serveur actif, mais pas lorsque nous avons deux serveurs ou plus enchaînés.

Par conséquent, si les serveurs frontaux et back-end se comportent différemment par rapport à un en-tête de type Transfer-Encoding qui est éventuellement obscurci, alors ils peuvent être en désaccord sur les limites entre les requêtes successives. Cela pourrait conduire à des vulnérabilités et à une attaque HTTP Request Smuggling.

## Comment empêcher cette attaque dangereuse

Les attaques de trafic de requête HTTP impliquent de placer à la fois l'en-tête Content-Length et l'en-tête Transfer-Encoding dans la même requête HTTP, puis de les manipuler afin que les serveurs frontaux et principaux traitent la requête différemment.

Klein, que nous avons mentionné précédemment, appelle à la normalisation des requêtes HTTP sortantes des serveurs proxy et souligne le besoin d'une solution de pare-feu d'application Web robuste et open source, capable de gérer ces types d'attaques. Pour résoudre ce problème, Klein a publié un projet basé sur C++ qui garantira que toutes les requêtes HTTP entrantes sont pleinement valides, conformes et sans ambiguïté. Ce travail est publié sur GitHub et vous pouvez le consulter ici .

D'un autre côté, voici d'autres choses que nous pourrions faire pour éviter une attaque de trafic de requêtes HTTP :

    Désactivez la réutilisation des connexions principales, de sorte que chaque demande principale soit envoyée via une connexion réseau distincte.
    Utilisez HTTP/2 pour les connexions back-end, car ce protocole évite toute ambiguïté dans les limites entre les demandes.
    Le même et identique logiciel de serveur Web doit être utilisé pour les serveurs frontaux et principaux. Cela garantit qu'ils s'entendent sur les limites entre les demandes.

Comme vous l'avez vu, l'attaque HTTP Request Smuggling est très importante en raison de sa gravité, elle est assez dangereuse, cependant, nous avons différentes manières d'empêcher cette attaque et de l'atténuer correctement.

