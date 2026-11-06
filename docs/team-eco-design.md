# Page « Explorer » (liste desktop) : écoconception et sécurité

*Analyse · RGESN 2024 + RGS v2.0*

C’est l’écran le plus chargé de Moodgo, dans sa version Figma : en-tête avec menu Profil et bascule de thème, 3 filtres (5 en mobile), 8 cartes avec affiche et bouton favori, pagination « 1 / 100 », pied de page. Les affiches viennent des API TMDB et Google Books, les sorties de DataTourisme et des membres.

Analyse faite sur la maquette, pas sur le site développé : les mesures réelles (poids, requêtes, en-têtes) seront à refaire sur la version en ligne. Stack visée : Angular + Spring Boot.

## Ce que pèse la page aujourd’hui

| Valeur | Indicateur | Détail |
|---|---|---|
| **8** | affiches venues d’API externes | Servies par TMDB et Google Books : on ne les recompresse pas, mais on choisit la taille demandée à l’API. |
| **5** | fichiers de police, auto-hébergés | Polices téléchargées dans le projet : seulement les graisses utilisées ici (Sora 700 et 800, Plus Jakarta Sans 400, 500 et 700). |
| **≈ 19** | requêtes estimées | 1 HTML, 1 CSS, 1 JS, 5 fichiers de police, 8 affiches, 1 à 2 appels API. À mesurer sur le site. |
| **1 / 100** | pagination déjà prévue | 8 cartes par page avec première, précédente, suivante et dernière page : pas de défilement infini. |

## RGESN — Référentiel général d’écoconception des services numériques (Arcep, Arcom, 2024)

### 17 critères qui concernent cette page

**6 conformes** · **7 à vérifier** · **1 non conforme** · **3 dépendent de l’API**

| Critère | Ce que demande le critère | Constat sur la page | À faire en dev | Priorité | Statut |
|---|---|---|---|---|---|
| 4.1 | Pas de lecture automatique de vidéo, son ou animation | Aucune vidéo ni animation sur la page. | Garder ce principe pour les bandes-annonces des fiches films : lecture au clic seulement. | Basse | Conforme |
| 4.2 | Pas de défilement infini | Pagination 1 / 100 avec première, précédente, suivante et dernière page. | Pagination côté serveur : ne charger que les 8 cartes de la page, et garder le numéro de page dans l’URL. | Basse | Conforme |
| 4.3 | Parcours optimisé pour la fonction principale | Filtres sur la même page, la fiche est à 1 clic. | Garder les filtres dans l’URL pour revenir à la liste sans relancer la recherche. | Basse | Conforme |
| 4.4 | L’utilisateur décide d’activer un service tiers | Polices téléchargées dans le projet : aucun service tiers appelé pour l’interface. | Garder ce principe : aucun script tiers (stats, pub, vidéo intégrée) sans consentement. | Basse | Conforme |
| 4.8 | Nombre de polices téléchargées limité | 2 familles, seules les graisses utilisées sont téléchargées. | Format woff2, sous-ensemble latin ; revoir la liste des graisses si un écran en ajoute une. | Basse | Conforme |
| 4.9 | Requêtes serveur limitées pendant la saisie | Chaque clic sur un filtre peut relancer une requête. | Filtrer en mémoire quand les résultats sont déjà chargés, sinon une seule requête par changement, avec un délai (debounce). | Moyenne | À vérifier |
| 4.14 | Pas de procédé manipulatoire | Pas de faux compteur, pas d’urgence artificielle, pas de case pré-cochée. | Rester vigilant sur les mises en avant payantes (#41) : les signaler clairement comme telles. | Basse | Conforme |
| 5.1 | Format de fichier adapté au contenu | Les affiches viennent de TMDB et Google Books : on ne choisit pas leur format. | Rien à faire sur le format ; le noter comme limite connue dans la déclaration d’écoconception. Pour les images des événements créés par les membres, convertir en WebP à l’import. | Moyenne | Dépend de l’API |
| 5.2 | Compression des images adaptée | Compression fixée par TMDB et Google Books. | Demander la plus petite taille proposée par l’API : TMDB w185 ou w342 plutôt que « original », Google Books la vignette « thumbnail ». | Haute | Dépend de l’API |
| 6.1 | Poids maximum et limite de requêtes par écran | Aucun budget défini. | Fixer un budget (proposition : 500 Ko et 25 requêtes pour cet écran) et le contrôler à chaque build (Lighthouse CI, EcoIndex). | Haute | Non conforme |
| 6.2 | Mise en cache des contenus | Rien de prévu. | Fichiers statiques nommés avec une empreinte et mis en cache un an ; ETag sur les réponses de l’API. | Haute | À vérifier |
| 6.3 | Compression des ressources transférées | Rien de prévu. | Activer la compression Brotli ou gzip pour HTML, CSS, JS et JSON (serveur ou proxy). | Moyenne | À vérifier |
| 6.4 | Images aux dimensions de leur affichage | Selon l’URL demandée à l’API, l’affiche peut être bien plus grande que la vignette (environ 220 × 125 px). | Construire l’URL d’image avec la bonne taille (srcset avec TMDB w185 et w342) et fixer width et height pour éviter les sauts de mise en page. | Haute | À vérifier |
| 6.5 | Pas de chargement de ressources inutilisées | Les affiches de la 2e rangée se chargent avant d’être visibles. | loading="lazy" sur les affiches sous la ligne de flottaison, @defer et routes chargées à la demande dans Angular. | Moyenne | À vérifier |
| 6.7 | Ressources statiques hébergées par le service | Polices, icônes et styles servis par Moodgo ; affiches servies par TMDB et Google Books. | Les affiches restent sur les serveurs des API (leurs conditions l’imposent) : exception à noter dans la déclaration d’écoconception. | Basse | Dépend de l’API |
| 7.1 | Cache serveur pour les données les plus utilisées | Chaque page de résultats peut interroger TMDB, Google Books et DataTourisme. | Mettre en cache les réponses JSON des API (@Cacheable avec Caffeine ou Redis), par page et par filtre, durée à fixer selon la source (de 1 h à 24 h). | Haute | À vérifier |
| 7.2 | Durées de conservation des données | Critères de recherche, favoris, logs : pas de durée fixée. | Ne rien stocker pour un visiteur sans compte (#22 INV-3) ; fixer une durée pour les logs et les comptes inactifs. | Moyenne | À vérifier |

## RGS v2.0 — Référentiel général de sécurité (ANSSI)

### Sécurité : les risques de la page et les mesures à prévoir

Le RGS est obligatoire pour les autorités administratives, pas pour Moodgo. On suit sa démarche comme bonne pratique : analyse de risques, objectifs (disponibilité, intégrité, confidentialité, authentification, traçabilité), mesures, homologation interne, puis suivi. Les règles sur les mots de passe s’appuient sur la recommandation de la CNIL.

1. **Analyse de risques** : Version courte d’EBIOS RM : biens à protéger = comptes, planning, groupes, favoris.
2. **Objectifs** : Disponibilité, intégrité, confidentialité, authentification, traçabilité.
3. **Mesures** : Le tableau ci-dessous, à transformer en tickets.
4. **Homologation interne** : L’équipe valide les risques restants avant la mise en ligne.
5. **Suivi** : Logs, mises à jour des dépendances, revue à chaque version.

| Objectif | Repère RGS | Risque sur cette page | À faire en dev (Spring Boot / Angular) | Priorité |
|---|---|---|---|---|
| Confidentialité | Annexe B1 | Données de compte et cookies de session interceptés sur un réseau public. | HTTPS partout (TLS 1.2 minimum, 1.3 de préférence), redirection du HTTP, en-tête HSTS. | Haute |
| Authentification | Annexe B3 | Vol de session via le menu Profil laissé ouvert, ou cookie lisible par un script. | Cookie de session HttpOnly, Secure et SameSite=Lax ; nouvel identifiant de session après la connexion ; expiration à 24 h (#4 INV-2) ; « Déconnexion » invalide la session côté serveur. | Haute |
| Authentification | B3 + CNIL | Mots de passe devinés ou volés en base. | Hachage BCrypt ou Argon2 (Spring Security PasswordEncoder) ; 12 caractères minimum (#5) ; limite de tentatives (#5 CN-3c). | Haute |
| Confidentialité | Contrôle d’accès | Lire les favoris ou le planning d’un autre membre en changeant un identifiant dans l’URL. | Vérifier côté serveur, à chaque requête, que l’objet appartient bien au membre connecté ; ne jamais faire confiance à l’identifiant envoyé par le front. | Haute |
| Intégrité | Contenus affichés | Titre ou description piégés (script) dans un événement créé par un membre (#8) ou reçu de DataTourisme. | Laisser Angular échapper le texte ; interdire [innerHTML] et bypassSecurityTrust sur ces champs ; nettoyer à l’enregistrement côté serveur. | Haute |
| Intégrité | Paramètres | Filtre, catégorie ou numéro de page modifiés à la main (page=-1, page=999999, injection SQL). | Liste blanche des catégories (#22 INV-2), numéro de page borné et taille de page fixe côté serveur ; requêtes paramétrées via JPA, jamais de concaténation. | Haute |
| Authentification | Contrôle d’accès | Bouton favori sur chaque carte : un site piège pourrait le déclencher à la place du membre connecté. | Protection CSRF de Spring Security (jeton lu par Angular via HttpClient XSRF) ; visiteur non connecté renvoyé vers E6 ; vérification du compte côté serveur. | Haute |
| Confidentialité | Tiers | Affiches chargées depuis TMDB et Google Books, parfois en http:// : contenu mixte et fuite de l’URL de la page. | Forcer https:// sur les URL d’images ; CSP img-src limitée à image.tmdb.org et books.google.com ; Referrer-Policy strict-origin-when-cross-origin. | Moyenne |
| Intégrité | En-têtes HTTP | Page intégrée dans un site piège (clickjacking), scripts injectés. | Content-Security-Policy (sources autorisées uniquement), X-Content-Type-Options nosniff, Referrer-Policy, frame-ancestors none : via Spring Security headers(). | Moyenne |
| Confidentialité | Secrets | Clés des API DataTourisme / films visibles dans le code Angular. | Toutes les clés restent côté Spring Boot (variables d’environnement), le front passe par l’API Moodgo. | Haute |
| Disponibilité | Analyse de risques | Recherche appelée en boucle, API externe lente ou hors service. | Limite de requêtes par IP sur la recherche (Bucket4j) ; délai maximum sur les appels externes ; cache (RGESN 7.1) ; état E5 côté front. | Moyenne |
| Traçabilité | Objectifs RGS | Impossible de savoir qui a fait quoi après un incident. | Journaliser connexions, échecs et actions des modérateurs (#39 POST-3), sans mot de passe ni donnée sensible (#5 INV-1), avec une durée de conservation fixée. | Moyenne |
| Intégrité | Suivi | Faille connue dans une dépendance Angular ou Spring. | Dependabot ou npm audit + OWASP Dependency-Check en CI ; mises à jour de sécurité séparées des nouvelles fonctionnalités (RGESN 3.5). | Moyenne |

## Plan d’action pour l’équipe

### Bloquant avant la mise en ligne

*À faire dès les premiers sprints*

- HTTPS, HSTS et cookies de session sécurisés
- Hachage des mots de passe et limite de tentatives
- Contrôle d’accès côté serveur sur favoris, planning et groupes
- Clés d’API uniquement côté Spring Boot
- Échappement des contenus venus des membres et de DataTourisme
- Protection CSRF sur le bouton favori, pagination bornée côté serveur

### Pendant le développement

*Dans la définition de « terminé » de chaque ticket*

- Affiches : plus petite taille d’API utile (w185 / w342), srcset, lazy loading, width et height fixés
- Cache des API externes et compression Brotli / gzip
- En-têtes de sécurité (CSP, nosniff, frame-ancestors)
- Filtres en mémoire ou avec debounce
- Budget poids et requêtes contrôlé à chaque build

### Après la première version

*Suivi et amélioration continue*

- Mesure EcoIndex et Lighthouse sur la page en ligne, comparée au budget
- Revue des logs et des dépendances à chaque version
- Durées de conservation appliquées (logs, comptes inactifs)
- Homologation interne refaite à chaque évolution majeure
- Déclaration d’écoconception RGESN publiée avec la déclaration d’accessibilité

---

Sources : RGESN version 2024 (ecoresponsable.numerique.gouv.fr, Arcep) · RGS v2.0 et annexes B1 à B3 (cyber.gouv.fr, ANSSI) · Recommandation mots de passe et fiche « Authentifier les utilisateurs » (cnil.fr). Les budgets chiffrés (poids, requêtes, durées de cache) sont des propositions à valider en équipe.