```mermaid
flowchart LR
    %% ===== Acteurs =====
    Visiteur(["👤 Visiteur"])
    Membre(["👤 Membre connecté"])
    Orga(["👤 Organisateur d'événement"])
    Modo(["👤 Modérateur"])
    IA(["🤖 IA / Chatbot"])

    Membre -.->|hérite de| Visiteur
    Orga -.->|hérite de| Membre

    subgraph Moodgo["Système Moodgo"]
        direction TB

        %% --- Authentification ---
        UC_Auth(["S'authentifier / créer un compte"])

        %% --- Option sociable ---
        UC_Sortie(["Rechercher une sortie<br/>date / heure / lieu"])
        UC_Chatbot(["Répondre à l'arbre de questions"])
        UC_Fiche(["Consulter la fiche détaillée"])

        %% --- Option casanier ---
        UC_Film(["Rechercher un film / une série"])
        UC_Qualif(["Qualifier la recherche via formulaire"])
        UC_Livre(["Trouver un livre pour ce soir"])

        %% --- Groupe ---
        UC_Groupe(["Créer un groupe"])
        UC_Inviter(["Inviter des participants"])
        UC_Propos(["Visualiser les propositions communes"])
        UC_Decider(["Prendre une décision collective<br/>vote / outils"])

        %% --- Organisateur ---
        UC_MonEvent(["Retrouver son événement dans la liste"])
        UC_MiseAvant(["Demander la mise en avant"])
        UC_Valider(["Valider la demande selon la charte"])

        %% --- Week-end ---
        UC_WE(["Organiser son week-end"])
        UC_BesoinIA(["Soumettre son besoin à l'IA"])
        UC_Calendrier(["Choisir les propositions dans le calendrier"])
        UC_Partager(["Partager le planning"])
        UC_RS(["Partager sur les réseaux sociaux"])
        UC_GroupeMoodgo(["Partager dans un groupe Moodgo"])
    end

    %% ===== Associations acteurs =====
    Visiteur --- UC_Sortie
    Visiteur --- UC_Film
    Visiteur --- UC_Livre
    Visiteur --- UC_Auth

    Membre --- UC_Groupe
    Membre --- UC_Propos
    Membre --- UC_Decider
    Membre --- UC_WE

    Orga --- UC_MonEvent
    Orga --- UC_MiseAvant

    UC_Valider --- Modo
    UC_Chatbot --- IA
    UC_BesoinIA --- IA

    %% ===== Include / Extend =====
    UC_Sortie -.->|«include»| UC_Fiche
    UC_Chatbot -.->|«extend»| UC_Sortie

    UC_Film -.->|«include»| UC_Qualif
    UC_Livre -.->|«include»| UC_Qualif

    UC_Groupe -.->|«include»| UC_Inviter
    UC_Groupe -.->|«include»| UC_Auth
    UC_Decider -.->|«include»| UC_Propos

    UC_MiseAvant -.->|«include»| UC_Valider
    UC_MiseAvant -.->|«include»| UC_Auth

    UC_WE -.->|«include»| UC_Auth
    UC_WE -.->|«include»| UC_BesoinIA
    UC_WE -.->|«include»| UC_Calendrier
    UC_Partager -.->|«extend»| UC_WE
    UC_RS -.->|hérite de| UC_Partager
    UC_GroupeMoodgo -.->|hérite de| UC_Partager
    ```