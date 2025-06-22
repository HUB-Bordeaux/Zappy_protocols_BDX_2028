# Protocole de Communication IA – Incantation

**Tous les messages sont encodés en Base64.**

---

##  Messages utilisés

### `prepare_<ID-OF-INCANTATOR>_lvl`
> **Description** : L’IA a toutes les ressources pour s'incanter, elle envoie le message pour indiquer aux autres participants de faire un stock de nourriture avant d'aller vers elle pour l'incantation.  
> **Paramètres** :
- `ID-OF-INCANTATOR` : Identifiant de l’IA qui initie l’incantation.
- `lvl` : Niveau ciblé par l’incantation.

---

### `come_<ID>`
> **Description** : Demande aux autres IA de venir se regrouper pour lancer une incantation.
> **Paramètres** :
- `ID` : Identifiant de l’IA initiatrice de l’incantation.

---

### `go_<ID>_to_<ID-OF-INCANTATOR>`
> **Description** : Message envoyé au participant qui va faire l'incantation pour lui dire qu'on va venir s'élever avec lui
> **Paramètres** :
- `ID` : Identifiant de l’IA destinataire.
- `ID-OF-INCANTATOR` : Identifiant de l’IA qui a envoyé le message `come`.

---

### `no_<ID>_<ID-OF-INCANTATOR>`
> **Description** : Refus ou impossibilité de participer à l’incantation.  
> **Paramètres** :
- `ID` : Identifiant de l’IA destinataire.
- `ID-OF-INCANTATOR` : Identifiant de l’IA initiatrice de l’incantation.

---


### `here_<ID>_<ID-OF-INCANTATOR>`
> **Description** : Message envoyé pour indiquer que l’IA est arrivé sur la même case que l’incantateur.  
> **Paramètres** :
- `ID` : Identifiant de l’IA qui est arrivée.
- `ID-OF-INCANTATOR` : Identifiant de l’IA initiatrice.


---

### `ready_<ID>_<ID-OF-INCANTATOR>`
> **Description** : L’IA est prête et sur le point de partir vers l’incantateur.  
> **Paramètres** :
- `ID` : Identifiant de l’IA prête.
- `ID-OF-INCANTATOR` : Identifiant de l’IA initiatrice.

---

### `finish_<ID-OF-INCANTATOR>`
> **Description** : L’incantation a réussi. Message envoyé aux participants.  
> **Paramètres** :
- `ID-OF-INCANTATOR` : Identifiant de l’IA qui a mené l’incantation.

---

### `failed_<ID-OF-INCANTATOR>`
> **Description** : L’incantation a échoué. Message envoyé aux participants.
> **Paramètres** :
- `ID-OF-INCANTATOR` : Identifiant de l’IA qui a mené l’incantation.

---

### `pls_<ID-OF-INCANTATOR>_ROCK_NBR`
> **Description** : L'IA demande aux participants s'ils ont en stock un type de pierre
> **Paramètres** :
- `ID-OF-INCANTATOR` : Identifiant de l’IA qui demande.
-`ROCK` : La ressource que l'on demande
-`NBR` : Le nombre que l'on souhaite

---

### `have_<ID-OF-INCANTATOR>_ID_ROCK`
> **Description** : L'IA demande à une IA si elle a les ressources qu'elle a demandées.
> **Paramètres** :
- `ID-OF-INCANTATOR` : Identifiant de l’IA qui a demandé.
-`ID` : Identifiant de l’IA qui a les ressources.
-`ROCK` : Nom de la pierre que la personne va poser

---

### `ok_<ID-OF-INCANTATOR>_ID_ROCK`
> **Description** : L'IA dis a l'IA qui lui a proposé ses ressources qu'elle les a déja obtenue par quelqu'un d'autre.
> **Paramètres** :
- `ID-OF-INCANTATOR` : Identifiant de l’IA qui a demandé.
-`ID` : Identifiant de l’IA qui a dit qu'elle avait les ressources necessaire.
-`ROCK` : Nom de la pierre que la personne va poser
---


## Exemple:
**voici un exemple du message qu'on va recevoir:**
Y29tZV8xMjM=

**Une fois décodé, on lit ceci:**
come_123


