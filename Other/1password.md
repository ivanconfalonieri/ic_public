# 1Password Connect API - Cheat Sheet

## 1️⃣ Concetti chiave

* **Vault**: spazio dove sono salvati gli item.
* **Vault ID (`VAULT_ID`)**: identificatore univoco senza spazi, **usato in tutte le API**.
* **Token**: definisce i permessi (`read`, `write`).
* **Item**: ogni record (es. login, note, database credential).

---

## 2️⃣ Ottenere il Vault ID da nome (generico)

```bash
VAULT_ID=$(curl -s -H "Authorization: Bearer $TOKEN" \
  http://<EXTERNAL-IP>:8080/v1/vaults | jq -r '.[] | select(.name=="VAULT_NAME") | .id')
```

* Sostituire `VAULT_NAME` con il nome del vault desiderato.

---

## 3️⃣ Lettura

### 3.1 Elenco dei vault

```bash
curl -s -H "Authorization: Bearer $TOKEN" \
http://<EXTERNAL-IP>:8080/v1/vaults | jq
```

### 3.2 Elenco degli item di un vault

```bash
curl -s -H "Authorization: Bearer $TOKEN" \
http://<EXTERNAL-IP>:8080/v1/vaults/$VAULT_ID/items | jq
```

### 3.3 Leggere un item singolo (include password)

```bash
curl -s -H "Authorization: Bearer $TOKEN" \
http://<EXTERNAL-IP>:8080/v1/vaults/$VAULT_ID/items/ITEM_ID | jq
```

### 3.4 Estrarre solo la password

```bash
curl -s -H "Authorization: Bearer $TOKEN" \
http://<EXTERNAL-IP>:8080/v1/vaults/$VAULT_ID/items/ITEM_ID \
| jq -r '.fields[] | select(.type=="CONCEALED") | .value'
```

---

## 4️⃣ Scrittura

### 4.1 Creare un nuovo item

```bash
curl -s -X POST \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  http://<EXTERNAL-IP>:8080/v1/vaults/$VAULT_ID/items \
  -d '{
    "title": "example-secret",
    "category": "LOGIN",
    "fields": [
      { "id": "username", "type": "STRING", "value": "USERNAME" },
      { "id": "password", "type": "CONCEALED", "value": "PASSWORD" }
    ]
  }' | jq
```

* Sostituire `USERNAME` e `PASSWORD` con i valori desiderati.

### 4.2 Modifica item (PATCH)

```bash
curl -s -X PATCH \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  http://<EXTERNAL-IP>:8080/v1/vaults/$VAULT_ID/items/ITEM_ID \
  -d '{"fields": [{"id": "password", "type": "CONCEALED", "value": "NEW_PASSWORD"}]}' | jq
```

### 4.3 Eliminare un item

```bash
curl -s -X DELETE \
  -H "Authorization: Bearer $TOKEN" \
  http://<EXTERNAL-IP>:8080/v1/vaults/$VAULT_ID/items/ITEM_ID
```

---

## 5️⃣ Errori comuni

| Errore             | Possibile causa                   |
| ------------------ | --------------------------------- |
| 403 Forbidden      | Token senza permessi sul vault    |
| 404 Not Found      | VaultId o ItemId errato           |
| Connection refused | Connect non esposto correttamente |

---

## 6️⃣ Schema rapido

```text
VAULT_ID ← recuperato via nome generico
 |
 ├─ GET /v1/vaults/$VAULT_ID/items           ← elenco item (no password)
 ├─ GET /v1/vaults/$VAULT_ID/items/ITEM_ID  ← legge item completo (password disponibile)
 ├─ POST /v1/vaults/$VAULT_ID/items         ← crea item
 ├─ PATCH /v1/vaults/$VAULT_ID/items/ITEM_ID ← aggiorna item
 └─ DELETE /v1/vaults/$VAULT_ID/items/ITEM_ID ← elimina item
```
