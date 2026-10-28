````mermaid
flowchart LR
 subgraph Moodgo["APP ModdfGo"]
    direction TB
        UC_Auth@{ label: "S'authentifier / créer un compte" }
        UC_Chatbot(["Qualifier la recherche via formulaire"])
        UC_Fiche(["Consulter la fiche détaillée"])
        UC_Film(["Rechercher un film / une série"])
        UC_Qualif(["Qualifier la recherche via formulaire"])
        UC_Livre(["Trouver un livre pour ce soir"])
        UC_Groupe(["Créer un groupe"])
        UC_Inviter(["Inviter des participants"])
        UC_Propos(["Visualiser ses groupes et leurspropositions (membres)"])
        UC_Decider(["Prendre une décision collective<br>vote / outils"])
        UC_MonEvent@{ label: "Retrouver les évenements qu'il a créé" }
        UC_MiseAvant(["Demander la mise en avant"])
        UC_Valider(["Valider la demande selon la charte"])
        UC_WE(["Organiser son week-end"])
        UC_BesoinIA@{ label: "Soumettre son besoin à l'IA" }
        UC_Calendrier(["Choisir les propositions dans le calendrier"])
        UC_Partager(["Partager le planning"])
        UC_RS(["Partager sur les réseaux sociaux"])
        UC_GroupeMoodgo(["Partager dans un groupe Moodgo"])
        n3(["Rechercher Jeux de société"])
        n1(["Litses suggerrés selon affinité (films / series/ livres etc...)"])
        n2["Fiche détaillée"]
        n4(["Rechercherr une activité extérieure"])
        n5(["Litses suggerrés selon affinité (films / series/ livres etc...)"])
  end
    Membre(["👤 Membre connecté"]) -. extend .-> Visiteur(["👤 Visiteur"])
    Orga@{ label: "👤 Organisateur d'événement" } -. hérite de .-> Membre
    Visiteur --- UC_Film & UC_Livre & UC_Auth
    Membre --- UC_Groupe & UC_Propos & UC_WE
    Orga --- UC_MonEvent & UC_MiseAvant
    UC_Valider --- Modo(["👤 Modérateur"])
    UC_Film -. «include» .-> UC_Qualif
    UC_Livre -. «include» .-> UC_Qualif
    UC_Groupe -. «include» .-> UC_Inviter
    UC_Decider -. «include» .-> UC_Propos
    UC_MiseAvant -. «include» .-> UC_Valider
    UC_WE -. «include» .-> UC_BesoinIA & UC_Calendrier
    UC_Partager -. «extend» .-> UC_WE
    UC_RS -. extend .-> UC_Partager
    UC_GroupeMoodgo -. extend .-> UC_Partager
    UC_Qualif --> n1
    n1 -. include .-> n2
    Visiteur --> n3 & n4
    n3 -. include .-> UC_Qualif
    n4 --> UC_Chatbot
    UC_Chatbot --> n5
    n5 -. include .-> UC_Fiche
    UC_Inviter --> UC_Decider

    UC_Auth@{ shape: stadium}
    UC_MonEvent@{ shape: stadium}
    UC_BesoinIA@{ shape: stadium}
    Orga@{ shape: stadium}
    ```
````
