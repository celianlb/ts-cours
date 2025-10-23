---
title: "15 - Pièges et débogage"
description: "Identifie les pièges courants en TypeScript et apprends à utiliser les outils de débogage pour corriger rapidement ton code."
---

❌ **Ne pas utiliser `any`** : Chaque `any` = dette technique

❌ **Oublier les validations** : Toujours valider les données entrantes

❌ **Ignorer les erreurs Mongoose** : Gérez les erreurs de validation

❌ **Pas de séparation** : Controller ≠ Service ≠ Model

❌ **Types trop permissifs** : Soyez aussi strict que possible

❌ **Pas de error handling** : Toujours try/catch dans controllers

❌ **Oublier `.bind()`** : Dans les routes Express pour garder `this`

❌ **Mauvais paths** : Vérifiez les imports avec les alias @

## Astuces gain de temps

✅ **Définissez les types en premier**, codez ensuite

✅ **Utilisez l'autocomplétion** au maximum (Ctrl+Space)

✅ **Testez fréquemment** avec Postman/Thunder Client

✅ **Committez régulièrement** (petits commits atomiques)

✅ **Lisez les erreurs TypeScript** attentivement

✅ **Copiez le tsconfig** de la présentation

✅ **Créez des snippets** pour les patterns répétitifs

## En cas de blocage

1. **Lisez l'erreur TypeScript** : Elle est souvent très claire

2. **Vérifiez vos imports** : Bon chemin ? Bon nom ? Alias correct ?

3. **Consultez la doc** : Mongoose, Express, TypeScript

4. **Simplifiez** : Commentez du code pour isoler le problème

5. **Vérifiez les types** : Hover sur les variables pour voir le type inféré

6. **Utilisez console.log** : Vérifiez les valeurs à runtime

7. **Demandez de l'aide** : N'attendez pas d'être bloqué trop longtemps
