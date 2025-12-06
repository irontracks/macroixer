# 🔥 Configuração das Regras do Firestore

## ⚠️ URGENTE: Configure as Regras de Segurança do Firestore

O erro **"Missing or insufficient permissions"** acontece porque o Firestore está bloqueando o acesso.

---

## 📝 Passo a Passo:

### 1. Acesse o Firebase Console
🔗 https://console.firebase.google.com/

### 2. Selecione seu projeto
- `irontracks-e6344`

### 3. Vá em "Firestore Database" no menu lateral
- Clique em "Firestore Database"

### 4. Vá na aba "Regras" (Rules)

### 5. Cole as regras abaixo:

```javascript
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    
    // Regra para dados dos usuários
    match /users/{userId} {
      // Permite leitura e escrita apenas para o próprio usuário
      allow read, write: if request.auth != null && request.auth.uid == userId;
      
      // Subcoleções do usuário
      match /{document=**} {
        allow read, write: if request.auth != null && request.auth.uid == userId;
      }
    }
  }
}
```

### 6. Clique em "Publicar"

---

## ✅ O que essas regras fazem:

- ✅ **Acesso autenticado**: Apenas usuários logados podem acessar seus dados
- ✅ **Privacidade**: Cada usuário só vê seus próprios dados
- ✅ **Subcoleções**: Inclui `settings`, `daily_logs`, `notifications`, etc.
- ✅ **Seguro**: Ninguém pode ver dados de outros usuários

---

## 🔒 Estrutura de Dados Protegida:

```
/users/{userId}/
  ├── settings/goals (metas do usuário)
  ├── daily_logs/{date} (logs diários)
  ├── notifications/settings (configurações de notificações)
  └── weight_history/{date} (histórico de peso)
```

---

## 🧪 Teste Após Configurar:

1. **Publique as regras** no Firebase Console
2. **Recarregue a página** do app (Ctrl+R ou Cmd+R)
3. **Faça login** com Google
4. **Digite uma refeição** e clique em "Calcular"
5. **Clique em "Confirmar"**
6. **Verifique:** Sem erros de "permission-denied" no console

---

## ⚠️ IMPORTANTE:

**Sem essas regras, o app NÃO funciona!** O Firebase bloqueia todas as operações por segurança.

Configure agora mesmo! 🚀

