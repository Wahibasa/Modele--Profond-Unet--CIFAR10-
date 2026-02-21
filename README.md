# Débruitage d’images CIFAR-10 avec U-Net et Grad-CAM

Dans ce projet, nous avons développé un modèle de **débruitage d’images** appliqué au dataset **CIFAR-10**. L’objectif principal était de reconstruire des images dégradées par du **bruit Poivre et Sel (20%)** et d’analyser les zones les plus importantes pour la reconstruction via **Grad-CAM**.

---

## Préparation des données

- Le dataset CIFAR-10 (images couleur 32×32) a été normalisé entre 0 et 1.  
- 10% des données d’entraînement ont été réservées pour la validation.  
- Du **bruit Poivre et Sel** a été ajouté sur les jeux d’entraînement, de validation et de test.  
- Une **augmentation de données** (flip horizontal aléatoire) a été appliquée pour améliorer la généralisation.  
- Les données ont été encapsulées dans un `tf.data.Dataset` avec **shuffle, batching (64), mapping et préfetch** pour optimiser l’entraînement.

---

## Architecture du modèle (U-Net)

- Nous avons utilisé une architecture **U-Net** adaptée aux images 32×32.  
- **Encodeur** : 3 blocs convolutionnels avec MaxPooling pour extraire les caractéristiques, dont une **couche cible** pour Grad-CAM.  
- **Bottleneck** : 1 bloc Conv2D central pour la compression des informations.  
- **Décodeur** : 4 blocs d’upsampling avec concaténation pour reconstruire l’image.  
- Activation finale : **Sigmoid** pour garantir que les valeurs des pixels soient dans [0,1].  
- Compilation du modèle :  
  - Optimiseur : **AdamW** (lr = 0.0005)  
  - Fonction de perte : **MSE** (Erreur quadratique moyenne)  
  - Métrique : **SSIM** (Similarité structurelle)

---

## Entraînement

- Le modèle a été entraîné pendant **20 epochs** sur les images bruitées, avec validation sur le jeu de validation bruité.  
- La **loss MSE** et la **métrique SSIM** ont été suivies pour évaluer la progression de l’apprentissage.  
- Nous avons utilisé **Grad-CAM** pour identifier les zones d’intérêt que le modèle exploite lors de la reconstruction.

---

## Évaluation quantitative

- Le modèle a été évalué sur les jeux **Train, Validation et Test**.  
- Les métriques calculées sont :  
  - **MSE** : erreur quadratique moyenne entre image originale et reconstruite  
  - **PSNR** : rapport signal sur bruit, indicateur de qualité de reconstruction  
  - **SSIM** : indice de similarité structurelle, indicateur perceptuel  
- Les résultats ont été compilés dans un tableau pour comparaison inter-jeux.

---

## Visualisation et Grad-CAM

- Nous avons sélectionné un sous-ensemble de 5 images pour visualisation.  
- Pour chaque image, nous avons présenté :  
  1. Image originale  
  2. Image bruitée (Poivre et Sel 20%)  
  3. Carte Grad-CAM indiquant les zones influentes  
  4. Image reconstruite par le modèle  
- Cette analyse qualitative complète les métriques quantitatives et illustre la capacité du modèle à se concentrer sur les zones pertinentes.

1](https://arxiv.org/abs/1610.02391)  
- TensorFlow / Keras documentation
