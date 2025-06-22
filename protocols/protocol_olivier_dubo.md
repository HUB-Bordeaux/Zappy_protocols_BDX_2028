# 📡 Protocole de Communication Broadcast Zappy

## 🎯 Vue d'ensemble

Le `BroadcastProtocol` fournit une communication **sécurisée** et **structurée** entre les IA d'une même équipe dans Zappy. Il remplace les messages broadcast simples par un système chiffré avec validation.

## 🔒 Sécurité

- **Checksum** : Chaque message a un checksum unique basé sur la clé de l'équipe (hash MD5 de "LA CLE DE FOU INCROYABLE")
- **Équipes isolées** : Les équipes ennemies ne peuvent pas décoder vos messages
- **Anti-replay** : Protection contre les messages dupliqués
- **Time to live** : Les anciens messages (>60s) sont rejetés automatiquement

## Préparation d'un message

### 1 - Préparer les données :
Rassembler le type de message, l’ID de l’expéditeur, le timestamp actuel et les données supplémentaires dans un dictionnaire.

### 2 - Formater les données :
Convertir ces données en une chaîne compacte au format :
t:type|s:sender|ts:timestamp|d:clé1:valeur1|clé2:valeur2

### 3 - Checksum :
Générer un court checksum (empreinte) avec SHA-256 sur la chaîne compacte + la clé secrète de l’équipe (16 premiers caractères du hash MD5 de "LA CLE DE FOU INCROYABLE"). Seul les 8 premiers caractères sont conservés pour le checksum.

### 4 - Message final :
Combiner le checksum et la chaîne compacte entre accolades :
{checksum:compact_data}

### 5 - Vérification de la longueur :
Vérifier que le message ne dépasse pas 1024 caractères.

### Message final

{*Checksum*:t:*Type de message*|s:*Id du joueur envoyant le message*|ts:*Timestamp du message*|d:*Données du message (plus d'explications en bas)*}

Exemple de message : ```{e3c6986a:t:elevation_cancel|s:127453|ts:1750351069|d:reason:low_food}```

## 📋 Types de messages disponibles

### 1. `ELEVATION_REQUEST` - Demande d'aide pour élévation
```python
# Créer le message
elevation_msg = protocol.create_elevation_request(sender_id=123, level=3)
ai.send_command(f"Broadcast {elevation_msg}")

# Décoder à la réception
decoded = protocol.decode_message(server_message)
if protocol.is_elevation_request(decoded):
    level = protocol.get_elevation_level(decoded)
    sender = protocol.get_sender_id(decoded)
    direction = protocol.get_direction(decoded)
```

Exemple de message : ```{6ba375e1:t:elevation_req|s:894804|ts:1750351070|d:level:2}```

### 2. `ELEVATION_CANCEL` - Annulation d'élévation
```python
# Raisons possibles: "low_food", "elevation_complete", "enemy_detected"
cancel_msg = protocol.create_elevation_cancel(sender_id=123, reason="low_food")
ai.send_command(f"Broadcast {cancel_msg}")
```

Exemple de message : ```{23408242:t:elevation_cancel|s:583991|ts:1750350526|d:reason:elevation_complete}```

## 🔧 Intégration dans ai_manager.py

### 1. Initialisation
```python
from broadcast_protocol import BroadcastProtocol

class ZappyAIManager:
    def __init__(self, host, port, team_name):
        self.broadcast_protocol = BroadcastProtocol(team_name)
```

### 2. Envoi de messages
```python
# Ancienne méthode (NON sécurisée)
ai.send_command(f"Broadcast level:{ai.level}|id:{ai_id}")

# Nouvelle méthode (sécurisée)
elevation_msg = self.broadcast_protocol.create_elevation_request(ai_id, ai.level)
ai.send_command(f"Broadcast {elevation_msg}")
```

### 3. Réception de messages
```python
def process_broadcast(self, ai, ai_id, inventory):
    raw_messages = list(ai.message_queue)
    ai.message_queue.clear()
    
    for raw_message in raw_messages:
        # Décoder avec le protocole sécurisé
        decoded_msg = self.broadcast_protocol.decode_message(raw_message)
        
        if decoded_msg is None:
            continue  # Message invalide/ennemi
        
        # Traiter selon le type
        if self.broadcast_protocol.is_elevation_request(decoded_msg):
            level = self.broadcast_protocol.get_elevation_level(decoded_msg)
            sender_id = self.broadcast_protocol.get_sender_id(decoded_msg)
            direction = self.broadcast_protocol.get_direction(decoded_msg)
            ...
        
        elif self.broadcast_protocol.is_elevation_cancel(decoded_msg):
            sender_id = self.broadcast_protocol.get_sender_id(decoded_msg)
            ...
```

## 🛡️ Avantages par rapport à l'ancien système

| Ancien système | Nouveau système |
|----------------|-----------------|
| Texte non structuré | Messages JSON structurés |
| Pas de sécurité | Checksum + clé équipe |
| Parsing manuel fragile | Validation automatique |
| Vulnérable aux ennemis | Protection contre l'espionnage |
| Pas de types | Types de messages clairs |
| Pas de TTL | Expiration automatique |
