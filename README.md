# GitHubAGL  
# Application de Reservation d'Hotel  

## Description  
L'application permet a un hotel de gerer les reservations des chambres, les clients, et les chambres elles-memes. Elle inclut des fonctionnalites telles que :  
- Creation de comptes utilisateurs  
- Connexion securisee  
- Reservation de chambres en temps reel  
- Gestion des informations clients  

## Cas d'Utilisation  

### 1. S'authentifier (Haute priorite)  
**Acteur** : Utilisateur (Client)  
**Priorite** : Haute  
**Pre-condition** : Aucune (acces libre a la page de connexion)  
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

---

## Diagrammes UML  

### Diagramme de Cas d'Utilisation Global  
![Diagramme CU Global](Diagrammes/global_use_case.png)  

### Diagramme de Classes  
```plantuml
@startuml
class Client {
  +id : Integer
  +nom : String
  +email : String
  +reserverChambre()
}

class Chambre {
  +numero : Integer
  +type : String
  +verifierDisponibilite()
}

class Reservation {
  +idReservation : Integer
  +confirmerPaiement()
}

Client --> Reservation  
Reservation --> Chambre  
@enduml