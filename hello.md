### Striping dans RAID 5
Dans RAID 5, **le striping** consiste à diviser les données en blocs et à les répartir entre tous les disques. Cela améliore les performances car plusieurs disques peuvent être lus ou écrits en même temps. Par exemple, si tu écris une grande quantité de données, RAID 5 peut les distribuer en blocs et les répartir sur plusieurs disques, ce qui rend l’opération plus rapide.

### Parité dans RAID 5
**La parité** est une information de redondance calculée à partir des blocs de données. En cas de panne d'un disque, RAID 5 utilise la parité pour reconstruire les données perdues. La parité elle-même est répartie entre tous les disques, donc aucun disque n’est uniquement dédié à la parité. Cela permet de stocker plus de données par rapport à certains autres niveaux RAID qui utilisent un disque entier pour la parité.

### Exemple : RAID 5 avec 4 Disques
Imaginons un RAID 5 composé de 4 disques, appelés `Disque A`, `Disque B`, `Disque C`, et `Disque D`. Supposons que nous ayons des blocs de données `D1`, `D2`, `D3`, `D4`, etc.

1. **Répartition des données et parité** :
   - RAID 5 stocke les données en **striping**, et la **parité est distribuée** entre les disques. Voici comment cela pourrait être organisé :

      | Bloc sur Disque A | Bloc sur Disque B | Bloc sur Disque C | Bloc sur Disque D |
      |-------------------|-------------------|-------------------|-------------------|
      | D1               | D2               | D3               | P1 (parité)       |
      | D4               | D5               | P2 (parité)      | D6               |
      | D7               | P3 (parité)      | D8               | D9               |
      | P4 (parité)      | D10              | D11              | D12              |

   - Ici :
     - **D1, D2, D3, D4**, etc., sont des blocs de données.
     - **P1, P2, P3, et P4** sont des blocs de parité. Ces blocs sont calculés pour chaque ensemble de données, de manière à permettre la récupération en cas de panne d’un disque.

2. **Comment fonctionne la parité ?**
   - Supposons que chaque bloc de données est simplement un nombre pour simplifier l'exemple.
   - La parité pour chaque ligne est calculée en combinant les blocs de données d’une ligne à l'aide d'une opération comme **l'addition modulaire** ou **l'opération XOR** (opération logique), qui est réversible. Cela signifie que si un des blocs de données est perdu, on peut le recalculer en utilisant les autres blocs et la parité.

3. **Exemple de récupération après une panne** :
   - Si **Disque A** tombe en panne, nous perdons les blocs **D1, D4, D7**, et **P4**.
   - Pour **reconstruire D1**, on utilise les blocs **D2**, **D3**, et **P1**. RAID 5 peut calculer D1 car la parité est une somme de D1, D2, et D3.
   - La même logique s'applique pour les autres blocs perdus. Grâce à la parité, RAID 5 peut restaurer les données perdues d’un seul disque défaillant.

### Pourquoi cette structure est-elle utile ?
- **Performance** : Le striping (répartition des données) améliore la vitesse, car plusieurs disques sont utilisés simultanément.
- **Tolérance aux pannes** : La parité distribuée permet de reconstruire les données si un disque tombe en panne.
- **Espace optimisé** : En RAID 5, un seul disque d’espace est utilisé pour la parité, même dans une configuration avec plusieurs disques.

### Limite de RAID 5
En RAID 5, si **deux disques tombent en panne simultanément**, les données ne pourront pas être récupérées, car la parité permet de reconstruire les données uniquement avec la perte d’un seul disque.
