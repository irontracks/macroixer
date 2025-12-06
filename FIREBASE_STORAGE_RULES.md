# 🔥 Configuração do Firebase Storage

## ⚠️ IMPORTANTE: Configure as Regras de Segurança

Para que as fotos funcionem, você precisa configurar as regras do Firebase Storage.

---

## 📝 Passo a Passo:

### 1. Acesse o Firebase Console
🔗 https://console.firebase.google.com/

### 2. Selecione seu projeto
- `irontracks-e6344`

### 3. Vá em "Storage" no menu lateral
- Clique em "Storage"
- Se ainda não ativou, clique em "Começar"

### 4. Vá na aba "Rules" (Regras)

### 5. Cole as regras abaixo:

```javascript
rules_version = '2';

service firebase.storage {
  match /b/{bucket}/o {
    // Regras para fotos de refeições
    match /meal_photos/{fileName} {
      // Permite leitura para todos (fotos públicas)
      allow read: if true;
      
      // Permite upload apenas para usuários autenticados
      allow write: if request.auth != null
                   && request.resource.size < 12 * 1024 * 1024  // Max 12MB (iPhone)
                   && request.resource.contentType.matches('image/.*'); // Apenas imagens
    }
  }
}
```

### 6. Clique em "Publicar"

---

## ✅ O que essas regras fazem:

- ✅ **Leitura pública**: Qualquer pessoa pode ver as fotos (necessário para carregar)
- ✅ **Upload autenticado**: Apenas usuários logados podem enviar fotos
- ✅ **Limite de 5MB**: Previne uploads muito grandes
- ✅ **Apenas imagens**: Bloqueia outros tipos de arquivo

---

## 🧪 Teste:

1. Faça login no app
2. Digite uma refeição
3. Clique em "Adicionar Foto"
4. Tire/selecione uma foto
5. Clique em "Confirmar"
6. Verifique no Firebase Console → Storage se a foto foi enviada

---

## 📊 Limites Gratuitos do Firebase Storage:

| Recurso | Limite Gratuito |
|---------|-----------------|
| **Armazenamento** | 5 GB |
| **Download/dia** | 1 GB |
| **Uploads/dia** | 20.000 |
| **Downloads/dia** | 50.000 |

**Isso é MUITO para uso pessoal!** 🎉

---

## 🔒 Segurança Adicional (Opcional):

Se quiser que cada usuário só possa fazer upload com seu próprio UID no nome do arquivo:

```javascript
match /meal_photos/meal_{userId}_{timestamp}.jpg {
  allow read: if true;
  allow write: if request.auth != null 
               && request.auth.uid == userId
               && request.resource.size < 5 * 1024 * 1024
               && request.resource.contentType.matches('image/.*');
}
```

Mas a regra básica já é segura o suficiente! ✅

