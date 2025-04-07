# GitHubAGL  
# Application de Reservation d'Hotel  

<<<<<<< HEAD
## Description  
- Description L'objectif de ce projet est de développer une application bureau permettant aux utilisateurs(clients) de rechercher,consulter et réserver des chambres d'hôtel en ligne.
- Le projet couvre l'ensemble du processus de réservation, incluant la consultation des disponibilités, la sélection des options(date de reservation,type de chambre etc..), et enfin le paiement ..
- Elle inclut des fonctionnalites telles que :
-Creation de comptes utilisateurs
-l'authentification securisee
-Reservation de chambres en temps reel
-Paiment en ligne
-Gestion des informations clients 
=======
## Description  L'objectif de ce projet est de développer une application bureau  permettant aux utilisateurs(clients) de rechercher,consulter et  réserver des chambres d'hôtel en ligne. Le projet couvre l'ensemble du processus de réservation, incluant la consultation des disponibilités, la sélection des options(date de reservation,type de chambre etc..), et enfin le paiement .. Elle inclut des fonctionnalites telles que :  
- Creation de comptes utilisateurs  
- Connexion securisee  
- Reservation de chambres en temps reel
- Paiment en ligne
- Gestion des informations clients  
>>>>>>> 2576ff31449d85ed858de12030dcd5fba4281b22

## Cas d'Utilisation  

### 1. S'authentifier (Haute priorite)  
**Acteur** : Utilisateur (Client)  
**Priorite** : Haute  
**Pre-condition** : Authentification réussie
**Post-condition** : Session utilisateur active  

#### Scenario principal  
1. L'utilisateur accede a la page de connexion  
2. Il saisit son email et mot de passe  
3. Le systeme verifie les identifiants  
4. Redirection vers le tableau de bord  

#### Table de Decision  
| ID  | Scenario                | Identifiant | Mot de passe | Resultat attendu          |  
|-----|-------------------------|-------------|--------------|---------------------------|  
| T1  | Authentification reussie | Valide      | Valide       | ✅ Connexion reussie      |  
| T2  | Mot de passe incorrect  | Valide      | Invalide     | ❌ "Identifiants incorrects" |  
| T3  | Identifiant inexistant  | Invalide    | Valide       | ❌ "Compte non trouve"     |  
| T4  | Champs vides            | Vide        | Vide         | ❌ "Champs obligatoires"   |  


![Diagramme CU Authentification](Diagrammes/auth_use_case.png)  
@startuml

actor "Utilisateur" as User

rectangle Authentification {
  usecase "S'authentifier" as UC1
  usecase "Valider identifiants" as UC2 <<include>>
  usecase "Recuperer mot de passe" as UC3 <<extend>>
  usecase "Verrouiller compte" as UC4 <<extend>>

  UC1 .> UC2 : include
  UC3 .> UC1 : extend
  UC4 .> UC1 : extend
}

User --> UC1
User --> UC3
User --> UC4

@enduml

---

### 2. Reserver une chambre (Haute priorite)  
**Acteur** : Client authentifie  
**Priorite** : Haute  
**Pre-condition** : Utilisateur connecte  
**Post-condition** : Reservation confirmee et email envoye  

#### Scenario principal  
1. Consultation des chambres disponibles  
2. Selection des dates de sejour  
3. Choix du type de chambre  
4. Paiement en ligne  
5. Confirmation par email  

#### Table de Decision  
| Scenario                | Disponibilite | Dates valides | Authentifie | Paiement valide | Resultat attendu          |  
|-------------------------|---------------|---------------|-------------|-----------------|---------------------------|  
| Reservation reussie     | ✅ Oui        | ✅ Oui        | ✅ Oui      | ✅ Oui          | ✅ Confirmation + email   |  
| Chambre non disponible  | ❌ Non        | ✅ Oui        | ✅ Oui      | ✅ Oui          | ❌ "Chambre indisponible" |  
| Dates invalides         | ✅ Oui        | ❌ Non        | ✅ Oui      | ✅ Oui          | ❌ "Dates invalides"      |  
| Paiement refuse         | ✅ Oui        | ✅ Oui        | ✅ Oui      | ❌ Non          | ❌ "Paiement echoue"      |  
| Utilisateur non connecte | ✅ Oui       | ✅ Oui        | ❌ Non      | ✅ Oui          | ❌ "Connexion requise"    |  

![Diagramme CU Reservation](Diagrammes/reservation_use_case.png)  
@startuml

actor "Client" as Client

rectangle "Systeme de reservation" {
  usecase "Consulter les chambres disponibles" as UC1
  usecase "Reserver une chambre" as UC2
  usecase "Payer en ligne" as UC3
}

Client --> UC1 : Verifie la disponibilite
Client --> UC2 : Si chambre disponible
Client --> UC3 : Finalise la transaction

UC2 .> UC1 : Depend de la disponibilite
UC3 .> UC2 : Depend de la reservation

@enduml

---

## Diagrammes UML  

### Diagramme de Cas d'Utilisation Global  
![Diagramme CU Global](Diagrammes/global_use_case.png)  
@startuml
rectangle "Systeme de reservation hotel" {

    actor Administrateur
    actor Client

    usecase "S authentifier" as Auth
    usecase "Consulter les reservations" as CR
    usecase "Gerer les clients" as GC
    usecase "Gerer les chambres" as GCH
    usecase "Reserver" as Res
    usecase "Payer" as Pay
    usecase "Consulter mes reservations" as CMR
    usecase "Annuler reservation" as AR
    usecase "Service Clients" as SC

    Administrateur -- Auth
    Auth <.. CR : <<extends>>
    Auth <.. GC : <<extends>>
    Auth <.. GCH : <<extends>>

    Client -- Res
    Client -- CMR
    Client -- SC

    Res ..> Pay : <<include>>
    CMR <.. AR : <<extends>>
}

@enduml

### Diagramme de sequence -Reserver  

![Diagramme CU Global](Diagrammes/sequence_diagram_reserver.png)  
@startuml
actor "Utilisateur" as User
participant "Systeme" as System

User -> System : Lance une reservation (dates, type de chambre)
activate System

alt Cas 1 : Chambre disponible
    System -> System : Verifie la disponibilite
    activate System
    System --> User : Chambre disponible
    deactivate System

    alt Cas 1.1 : Dates valides
        System -> System : Valide les dates (depart > arrivee)
        activate System
        System --> User : Dates acceptees
        deactivate System

        alt Cas 1.1.1 : Utilisateur authentifie
            System -> System : Verifie l authentification
            activate System
            System --> User : Session valide
            deactivate System

            alt Cas 1.1.1.1 : Paiement reussi
                System -> System : Traite le paiement
                activate System
                System --> User : Paiement accepte
                deactivate System

                System -> System : Enregistre la reservation
                activate System
                System --> User : Confirmation + email
                deactivate System
            else Cas 1.1.1.2 : Paiement refuse
                System --> User : ❌ Paiement echoue
            end

        else Cas 1.1.2 : Utilisateur non authentifie
            System --> User : ❌ Connectez-vous pour reserver
        end

    else Cas 1.2 : Dates invalides
        System --> User : ❌ Veuillez corriger les dates
    end

else Cas 2 : Chambre non disponible
    System --> User : ❌ Aucune chambre disponible
end

deactivate System
@enduml


### Diagramme de Classes  
![Diagramme Classe ](Diagrammes/class_diagram.png)  
@startuml

'#######################
' Classes principales
'#######################

class Utilisateur {
  +id : Integer
  +nom : String
  +email : String
  +motDePasse : String
  +seConnecter()
  +seDeconnecter()
}

class Client {
  +telephone : String
  +historiqueReservations()
}

class Administrateur {
  +ajouterChambre()
  +supprimerChambre()
  +modifierDetailsClient()
  +annulerReservation()
  +genererRapports()
}

class Chambre {
  +numero : Integer
  +type : String
  +prix : Float
  +disponible : Boolean
  +verifierDisponibilite()
  +modifierDetails()
}

class Reservation {
  +idReservation : Integer
  +dateDebut : Date
  +dateFin : Date
  +statut : String
  +creerReservation()
  +annulerReservation()
  +validerReservation()
}

<<<<<<< HEAD
class Paiement {
  +idPaiement : Integer
  +montant : Float
  +methode : String
  +statut : String
  +traiterPaiement()
}

'#######################
' Relations
'#######################

Utilisateur <|-- Client
Utilisateur <|-- Administrateur

Client "1" --> "0..*" Reservation : effectue
Reservation "1" --> "1" Chambre : concerne
Reservation "1" --> "1" Paiement : inclut

Administrateur "1" --> "1..*" Chambre : gere
Administrateur "1" --> "0..*" Client : gere
Administrateur "1" --> "0..*" Reservation : gere

'#######################
' Interfaces supplémentaires
'#######################

interface ServiceClient {
  +gererReclamation()
}

class Conseiller {
  +modifierReservation()
}

ServiceClient <|.. Conseiller

=======
Client --> Reservation  
Reservation --> Chambre  
>>>>>>> 2576ff31449d85ed858de12030dcd5fba4281b22
@enduml
