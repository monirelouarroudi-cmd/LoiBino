# Séance 3 — Probabilités : reconnaître et calculer avec la loi binomiale

> **Terminale — Spécialité Mathématiques | Durée : 2h**
>
> **Contexte :** l'élève a reçu trois fiches de cours sur la loi binomiale (TG‑20, TG‑21, TG‑22). Cette séance correspond à la première fiche (TG‑20) : reconnaître une situation binomiale, savoir la justifier, et calculer des probabilités (y compris « au moins », « au plus »).
>
> **Prérequis :** notion de probabilité, arbre pondéré, indépendance d'évènements (vus en Première).
>
> **Objectif :** savoir rédiger la justification d'une loi binomiale en quatre points, repérer les pièges (tirage sans remise), calculer $P(X=k)$ à la main pour les cas simples et à la calculatrice/en Python pour les cas généraux, et passer d'un évènement « au moins/au plus » à un évènement du type $P(X\leq k)$.

---

## 1. Déroulement

| Temps | Activité |
|---|---|
| 0–10 min | Diagnostic rapide (prérequis) |
| 10–30 min | Cours : épreuve de Bernoulli, loi binomiale, justification |
| 30–40 min | Méthode : « au moins / au plus », calculatrice, Python |
| 40–55 min | 5 exercices faciles |
| 55–75 min | 5 exercices moyens |
| 75–100 min | 5 exercices difficiles |
| 100–115 min | 5 exercices très difficiles |
| 115–120 min | Mini-évaluation + bilan |

> **Méthode professeur :** avant tout calcul, faire systématiquement rédiger les quatre points de justification de la loi binomiale à l'oral, même quand c'est « évident ». C'est cette rédaction qui est attendue au bac.

---

## 2. Diagnostic — 10 min

### Question 1
Qu'appelle-t-on une « épreuve de Bernoulli » ?

### Question 2
On lance un dé équilibré et on considère l'évènement « obtenir un 6 ». Quelle est la probabilité de succès $p$ de cette épreuve ?

### Question 3
Rappeler la formule donnant le nombre de combinaisons de $k$ éléments choisis parmi $n$.

### Question 4
On tire au hasard une carte dans un jeu de 32 cartes, on la remet, puis on recommence. Les tirages successifs sont-ils indépendants ? Pourquoi ?

### Question 5
Dans une urne de 10 boules (4 rouges, 6 vertes), on tire 2 boules **sans remise**. La probabilité de tirer une boule rouge au 2ᵉ tirage dépend-elle du résultat du 1ᵉʳ tirage ?

<details>
<summary>👉 Correction</summary>

**1.** Une expérience aléatoire qui n'a que deux issues possibles : un « succès » (de probabilité $p$) et un « échec » (de probabilité $1-p$).

**2.** $p=\dfrac16$.

**3.** $$C(n,k)=\binom{n}{k}=\frac{n!}{k!(n-k)!}$$

**4.** Oui : la carte est remise à chaque fois, donc la composition du jeu ne change pas. Chaque tirage a la même probabilité, indépendamment des précédents.

**5.** Oui : si la 1ʳᵉ boule était rouge, il reste 3 rouges sur 9 ; si elle était verte, il reste 4 rouges sur 9. La probabilité change donc selon le résultat précédent : les tirages **ne sont pas indépendants**.

</details>

---

## 3. Résumé de cours

### 3.1 Épreuve et schéma de Bernoulli

Une **épreuve de Bernoulli** de paramètre $p$ est une expérience aléatoire à deux issues : succès (probabilité $p$) et échec (probabilité $1-p$).

Un **schéma de Bernoulli** consiste à répéter $n$ fois, de façon indépendante, la même épreuve de Bernoulli de paramètre $p$.

---

### 3.2 Loi binomiale : définition

Si $X$ compte le nombre de succès obtenus dans un schéma de Bernoulli de paramètres $n$ et $p$, on dit que $X$ suit la **loi binomiale de paramètres $n$ et $p$**, notée :

$$X\sim \mathcal B(n;p)$$

Pour tout entier $k$ tel que $0\leq k\leq n$ :

$$P(X=k)=\binom{n}{k}\,p^{k}\,(1-p)^{n-k}$$

---

### 3.3 Justifier qu'une variable suit une loi binomiale

On rédige en **quatre points** :

a. on répète $n$ fois (nombre fixé à l'avance) une même épreuve à deux issues : succès / échec ;

b. les répétitions sont **indépendantes** (tirage avec remise, lancers successifs, individus pris dans une population très grande...) ;

c. la probabilité du succès est **la même** à chaque répétition : $p=\dots$ ;

d. $X$ compte le nombre de succès, donc $X\sim\mathcal B(n;p)$.

---

### 3.4 Attention : sans remise, ce n'est pas binomial

**Exemple.** Une urne contient 5 boules rouges et 5 boules vertes. On tire successivement 3 boules **sans remise**. Au 1ᵉʳ tirage, $P(\text{rouge})=\dfrac{5}{10}=\dfrac12$. Mais au 2ᵉ tirage, cette probabilité dépend du résultat du 1ᵉʳ : les tirages **ne sont pas indépendants**, donc $X$ n'est pas binomiale.

**Cependant :** lorsque la population est très grande devant la taille de l'échantillon (un sondage, par exemple), on **assimile souvent** un tirage sans remise à un tirage avec remise, et on utilise quand même la loi binomiale.

---

### 3.5 Calculer $P(X=k)$

- **À la main**, pour les cas extrêmes, c'est rapide :

$$P(X=0)=(1-p)^{n}\qquad\qquad P(X=n)=p^{n}$$

- **À la calculatrice**, les fonctions *loi binomiale* (BinomialPD / binomFdp selon la marque) donnent directement $P(X=k)$, et les fonctions *cumulée* (BinomialCD / binomFRép) donnent $P(X\leq k)$.

- **En Python**, avec le module `math` :

```python
from math import comb

def loi(n, p, k):        # renvoie P(X = k)
    return comb(n, k) * p**k * (1 - p)**(n - k)

def cumul(n, p, k):      # renvoie P(X <= k)
    s = 0
    for i in range(k + 1):
        s = s + loi(n, p, i)
    return s
```

---

### 3.6 « Au moins », « au plus » : passer au complémentaire

Pour une variable entière, l'évènement $X\geq k$ est le contraire de $X\leq k-1$ :

$$P(X\geq k)=1-P(X\leq k-1)$$

$$P(X>k)=1-P(X\leq k)$$

---

### 3.7 Se ramener à un évènement $P(X\leq k)$

Pour une variable $X$ à valeurs entières et des entiers $a\leq b$ :

$$P(X\geq a)=1-P(X\leq a-1)$$

$$P(X<a)=P(X\leq a-1)$$

$$P(a\leq X\leq b)=P(X\leq b)-P(X\leq a-1)$$

> **Réflexe :** avant tout calcul de probabilité, toujours se ramener à une somme ou une différence d'évènements du type $P(X\leq k)$, seuls directement lisibles à la calculatrice ou en Python.

---

### 3.8 Pour la culture : la loi géométrique

Si l'on répète l'épreuve **jusqu'au premier succès**, le rang $T$ de ce succès ne suit **pas** une loi binomiale (le nombre de répétitions n'est pas fixé à l'avance). $T$ suit ce qu'on appelle une loi géométrique, étudiée plus tard :

$$P(T=k)=(1-p)^{k-1}p\quad\text{pour }k\geq1$$

---

## 4. Exercices faciles

### Exercice 1
On lance une pièce équilibrée 8 fois. $X$ = nombre de « pile » obtenus. Donner la loi de $X$.

<details>
<summary>👉 Correction</summary>

Succès = « pile », $p=0,5$. On répète 8 fois la même épreuve, de façon indépendante.

$$\boxed{X\sim\mathcal B(8;0,5)}$$

</details>

---

### Exercice 2
Soit $X\sim\mathcal B(5;0,2)$. Calculer $P(X=2)$.

<details>
<summary>👉 Correction</summary>

$$P(X=2)=\binom{5}{2}\times0,2^{2}\times0,8^{3}=10\times0,04\times0,512$$

$$\boxed{P(X=2)=0,2048}$$

</details>

---

### Exercice 3
Une urne contient 4 boules blanches et 6 boules noires. On tire une boule, on note sa couleur, on la remet, et on recommence 6 fois. $X$ = nombre de boules blanches obtenues. Donner $n$ et $p$.

<details>
<summary>👉 Correction</summary>

Il y a remise, donc indépendance des tirages.

$$\boxed{n=6\qquad p=\frac{4}{10}=0,4}$$

</details>

---

### Exercice 4
On interroge, **sans remise**, 15 personnes dans un groupe de 20 pour savoir si elles sont fumeuses ; $X$ = nombre de fumeurs parmi elles. $X$ suit-elle une loi binomiale ?

<details>
<summary>👉 Correction</summary>

Non : le tirage est fait sans remise dans une **petite** population (20 personnes). La probabilité qu'une personne soit fumeuse change selon les personnes déjà interrogées : les essais ne sont **pas indépendants**.

$$\boxed{X\text{ ne suit pas une loi binomiale}}$$

</details>

---

### Exercice 5
Soit $X\sim\mathcal B(10;0,3)$. Calculer $P(X=0)$ et $P(X=10)$ sans calculatrice.

<details>
<summary>👉 Correction</summary>

$$P(X=0)=(1-0,3)^{10}=0,7^{10}\approx\boxed{0,0282}$$

$$P(X=10)=0,3^{10}\approx\boxed{0,0000059}$$

</details>

---

## 5. Exercices moyens

### Exercice 6
Un élève répond au hasard à un QCM de 5 questions, chacune ayant 4 réponses possibles dont une seule correcte. $X$ = nombre de bonnes réponses.

a. Justifier que $X$ suit une loi binomiale et donner ses paramètres.

b. Calculer $P(X=3)$.

<details>
<summary>👉 Correction</summary>

**a.** Succès = « bonne réponse », $p=\dfrac14$. Les 5 réponses sont indépendantes (au hasard), la probabilité de succès est la même à chaque question, $X$ compte les succès.

$$\boxed{X\sim\mathcal B(5;0,25)}$$

**b.**

$$P(X=3)=\binom53\times0,25^{3}\times0,75^{2}=10\times0,015625\times0,5625$$

$$\boxed{P(X=3)\approx0,0879}$$

</details>

---

### Exercice 7
On lance un dé équilibré 4 fois. Quelle est la probabilité d'obtenir au moins un 6 ?

<details>
<summary>👉 Correction</summary>

$X\sim\mathcal B\!\left(4;\dfrac16\right)$.

$$P(X\geq1)=1-P(X=0)=1-\left(\frac56\right)^{4}$$

$$\left(\frac56\right)^4=\frac{625}{1296}\approx0,4823$$

$$\boxed{P(X\geq1)\approx0,5177}$$

</details>

---

### Exercice 8
Soit $X\sim\mathcal B(8;0,4)$. Sachant que $P(X\leq5)\approx0,9502$, calculer $P(X\geq6)$ puis $P(X<6)$.

<details>
<summary>👉 Correction</summary>

$$P(X\geq6)=1-P(X\leq5)=1-0,9502=\boxed{0,0498}$$

Comme $X$ est entière, $X<6$ équivaut à $X\leq5$ :

$$\boxed{P(X<6)=0,9502}$$

</details>

---

### Exercice 9
Dans une ville de 50 000 habitants, 8 % sont vaccinés contre une maladie. On interroge au hasard, **sans remise**, 20 habitants. $X$ = nombre de vaccinés parmi eux.

a. Pourquoi, en toute rigueur, $X$ ne suit-elle pas exactement une loi binomiale ?

b. Pourquoi peut-on malgré tout l'assimiler à une loi binomiale ? Donner ses paramètres.

<details>
<summary>👉 Correction</summary>

**a.** Le tirage se fait sans remise : la probabilité qu'un habitant soit vacciné dépend légèrement des personnes déjà interrogées. Les essais ne sont donc pas rigoureusement indépendants.

**b.** La population (50 000) est immense devant la taille de l'échantillon (20) : retirer 20 personnes ne modifie quasiment pas les proportions restantes. On assimile donc :

$$\boxed{X\sim\mathcal B(20;0,08)}$$

</details>

---

### Exercice 10
Une variable aléatoire $X$ vérifie, pour tout entier $k$ de 0 à 6 :

$$P(X=k)=\binom6k\times0,4^{k}\times0,6^{6-k}$$

a. Quelle est la loi de $X$ ? Donner ses paramètres.

b. Sans calculer, expliquer pourquoi un énoncé du type « on tire 6 cartes sans remise dans un jeu de 32, $X$ = nombre de cœurs obtenus » ne pourrait pas donner cette formule.

<details>
<summary>👉 Correction</summary>

**a.** $$\boxed{X\sim\mathcal B(6;0,4)}$$

**b.** Un tirage sans remise dans un petit jeu de cartes rend les tirages dépendants les uns des autres (la composition du jeu change à chaque carte retirée) : la formule de la loi binomiale ne s'applique pas exactement dans ce cas.

</details>

---

## 6. Exercices difficiles

### Exercice 11
Une usine produit des pièces dont 3 % sont défectueuses, indépendamment les unes des autres. On prélève un échantillon de 25 pièces. $X$ = nombre de pièces défectueuses.

a. Justifier la loi de $X$.

b. Calculer $P(X\geq2)$ sachant que $P(X=0)=0,97^{25}\approx0,467$ et $P(X=1)=25\times0,03\times0,97^{24}\approx0,361$.

<details>
<summary>👉 Correction</summary>

**a.** Succès = « pièce défectueuse », $p=0,03$. Les 25 prélèvements sont indépendants (grande série de production), probabilité de succès constante.

$$\boxed{X\sim\mathcal B(25;0,03)}$$

**b.**

$$P(X\leq1)=P(X=0)+P(X=1)\approx0,467+0,361=0,828$$

$$P(X\geq2)=1-P(X\leq1)\approx1-0,828$$

$$\boxed{P(X\geq2)\approx0,172}$$

</details>

---

### Exercice 12
$X$ suit une loi binomiale de paramètres $n=20$ et $p$ inconnu. On sait que $P(X=0)\approx0,0032$. Déterminer une valeur approchée de $p$.

<details>
<summary>👉 Correction</summary>

$$P(X=0)=(1-p)^{20}=0,0032$$

$$1-p=0,0032^{1/20}\approx0,75$$

$$\boxed{p\approx0,25}$$

</details>

---

### Exercice 13
On considère $X\sim\mathcal B(10;0,5)$ et $Y\sim\mathcal B(20;0,25)$. Calculer $n_X\times p_X$ et $n_Y\times p_Y$. Peut-on en conclure que $X$ et $Y$ suivent « la même loi » ?

<details>
<summary>👉 Correction</summary>

$$n_X\,p_X=10\times0,5=5\qquad\qquad n_Y\,p_Y=20\times0,25=5$$

Les deux produits sont égaux, mais $X$ et $Y$ **n'ont pas la même loi** : leurs paramètres $(n,p)$ sont différents, donc leurs probabilités $P(X=k)$ et $P(Y=k)$ ne coïncident pas en général (on comparera rigoureusement ces deux lois grâce à l'espérance et à l'écart-type dans une prochaine séance).

</details>

---

### Exercice 14
On lance un dé équilibré 12 fois. $X$ = nombre de fois où l'on obtient un multiple de 3 (c'est-à-dire 3 ou 6).

a. Justifier que $X$ suit une loi binomiale et donner ses paramètres.

b. Le programme Python suivant doit calculer $P(X=4)$ :

```python
from math import comb

def loi(n, p, k):
    return comb(n, k) * p**k * (1 - p)**(n - k)

print(loi(12, 1/3, 4))
```

Que doit afficher ce programme, à $10^{-4}$ près ?

<details>
<summary>👉 Correction</summary>

**a.** Succès = « multiple de 3 », $p=\dfrac{2}{6}=\dfrac13$. 12 lancers indépendants, probabilité de succès constante.

$$\boxed{X\sim\mathcal B\!\left(12;\frac13\right)}$$

**b.**

$$P(X=4)=\binom{12}{4}\times\left(\frac13\right)^{4}\times\left(\frac23\right)^{8}=495\times\frac{1}{81}\times\frac{256}{6561}$$

$$\boxed{P(X=4)\approx0,2384}$$

</details>

---

### Exercice 15
Soit $X\sim\mathcal B(15;0,35)$. On donne $P(X\leq6)\approx0,6943$ et $P(X\leq7)\approx0,8402$.

Déterminer le plus petit entier $k$ tel que $P(X\leq k)\geq0,8$, puis en déduire $P(X\geq8)$.

<details>
<summary>👉 Correction</summary>

$P(X\leq6)\approx0,6943<0,8$ et $P(X\leq7)\approx0,8402\geq0,8$, donc :

$$\boxed{k=7}$$

$$P(X\geq8)=1-P(X\leq7)\approx1-0,8402$$

$$\boxed{P(X\geq8)\approx0,1598}$$

</details>

---

## 7. Exercices très difficiles / réflexion

### Exercice 16 — Réflexion
Soit $X\sim\mathcal B(n;p)$ avec $0<p<1$. Montrer que, pour $0\leq k\leq n-1$ :

$$\frac{P(X=k+1)}{P(X=k)}=\frac{n-k}{k+1}\times\frac{p}{1-p}$$

En déduire que la suite $\big(P(X=k)\big)$ croît tant que $k<(n+1)p-1$, puis décroît.

<details>
<summary>👉 Correction</summary>

$$\frac{P(X=k+1)}{P(X=k)}=\frac{\binom{n}{k+1}}{\binom{n}{k}}\times\frac{p}{1-p}=\frac{n-k}{k+1}\times\frac{p}{1-p}$$

Ce rapport est supérieur à 1 si et seulement si :

$$(n-k)p>(k+1)(1-p)$$

En développant :

$$np-kp>(k+1)-(k+1)p$$

$$np+p-k-1>0$$

$$\boxed{k<(n+1)p-1}$$

Donc $P(X=k+1)>P(X=k)$ tant que $k<(n+1)p-1$ : la suite des probabilités croît, atteint son maximum (le mode de la loi), puis décroît.

</details>

---

### Exercice 17 — Fiabilité
Un système de surveillance comporte 6 capteurs identiques et indépendants, chacun fonctionnant correctement avec probabilité 0,9. Le système est jugé fiable si **au moins 4** capteurs fonctionnent.

a. Donner la loi de $X$ = nombre de capteurs qui fonctionnent.

b. Calculer la fiabilité $P(X\geq4)$ sachant que $P(X=0)\approx9\times10^{-7}$, $P(X=1)\approx5,2\times10^{-5}$, $P(X=2)\approx0,00149$ et $P(X=3)\approx0,01785$.

<details>
<summary>👉 Correction</summary>

**a.** $$\boxed{X\sim\mathcal B(6;0,9)}$$

**b.**

$$P(X\leq3)\approx0,0000009+0,000052+0,00149+0,01785\approx0,01939$$

$$P(X\geq4)=1-P(X\leq3)\approx1-0,01939$$

$$\boxed{P(X\geq4)\approx0,9806}$$

</details>

---

### Exercice 18 — Vers la loi de Poisson
Pour $n\geq1$, on considère $X_n\sim\mathcal B\!\left(n;\dfrac1n\right)$.

a. Exprimer $P(X_n=0)$ en fonction de $n$.

b. On admet que $\left(1-\dfrac1n\right)^{n}$ tend vers $e^{-1}\approx0,3679$ quand $n$ devient très grand. Que peut-on dire de $P(X_n=0)$ dans ce cas ?

<details>
<summary>👉 Correction</summary>

**a.**

$$\boxed{P(X_n=0)=\left(1-\frac1n\right)^{n}}$$

**b.** Quand $n\to+\infty$ :

$$\boxed{P(X_n=0)\longrightarrow e^{-1}\approx0,3679}$$

Ce résultat (hors-programme) annonce le lien entre la loi binomiale et la loi de Poisson, vue en études supérieures.

</details>

---

### Exercice 19 — Prise de décision
Une entreprise affirme que 90 % de ses clients sont satisfaits. Un enquêteur interroge 10 clients choisis au hasard et indépendamment les uns des autres. $X$ = nombre de clients satisfaits parmi les 10.

a. Donner la loi de $X$ en justifiant, en supposant l'affirmation vraie.

b. On donne $P(X\leq7)\approx0,0702$. L'enquêteur trouve finalement 7 clients satisfaits sur 10. Doit-il remettre en cause l'affirmation de l'entreprise ? Justifier à l'aide du calcul précédent.

<details>
<summary>👉 Correction</summary>

**a.** Succès = « client satisfait », $p=0,9$ si l'affirmation est vraie ; 10 interrogations indépendantes.

$$\boxed{X\sim\mathcal B(10;0,9)}$$

**b.** $P(X\leq7)\approx0,0702$, soit environ 7 %. Si $p=0,9$ était exact, obtenir 7 clients satisfaits ou moins serait un évènement assez rare (moins d'une chance sur dix). Ce résultat est donc surprenant et **peut légitimement amener à douter** de l'affirmation de l'entreprise, sans pour autant la réfuter avec certitude (ce principe de décision sera approfondi avec les intervalles de fluctuation).

</details>

---

### Exercice 20 — Problème de synthèse
Une maladie touche 2 % d'une population très grande. On choisit au hasard 50 personnes dans cette population ; $X$ = nombre de personnes malades parmi elles.

a. Justifier que l'on peut modéliser $X$ par une loi binomiale et donner ses paramètres.

b. En reprenant les fonctions `loi` et `cumul` du cours, écrire une fonction `proba_seuil(n, p, k)` qui renvoie $P(X\geq k)$.

c. On obtient `proba_seuil(50, 0.02, 3)` $\approx0,0784$. Interpréter ce résultat dans le contexte de l'énoncé.

<details>
<summary>👉 Correction</summary>

**a.** La population est immense devant l'échantillon (50 personnes) : on assimile le tirage à un tirage avec remise. Succès = « personne malade », $p=0,02$, 50 répétitions indépendantes.

$$\boxed{X\sim\mathcal B(50;0,02)}$$

**b.**

```python
def proba_seuil(n, p, k):
    return 1 - cumul(n, p, k - 1)
```

**c.** La probabilité de trouver **au moins 3** personnes malades parmi les 50 personnes choisies est d'environ **7,8 %**.

</details>

---

## 8. Mini-évaluation de sortie — 5 minutes

À faire sans aide.

### A
On interroge 10 personnes, sans remise, dans un groupe de 12. $X$ = nombre de gauchers. $X$ suit-elle une loi binomiale ? Pourquoi ?

### B
Soit $X\sim\mathcal B(6;0,5)$. Calculer $P(X=0)$.

### C
Donner la formule permettant de calculer $P(X\geq3)$ à partir de $P(X\leq2)$.

### D
Rappeler les quatre points à rédiger pour justifier qu'une variable suit une loi binomiale.

<details>
<summary>👉 Correction</summary>

**A.** Non : tirage sans remise dans une petite population (12 personnes), les essais ne sont pas indépendants.

**B.** $$P(X=0)=0,5^{6}\approx\boxed{0,0156}$$

**C.** $$\boxed{P(X\geq3)=1-P(X\leq2)}$$

**D.** a. on répète $n$ fois une même épreuve à deux issues ; b. les répétitions sont indépendantes ; c. la probabilité de succès est constante ; d. $X$ compte les succès.

</details>

---

## 9. Bilan de fin de séance

### Les 4 réflexes à retenir

#### Réflexe 1 — Avant de parler de loi binomiale

**Je rédige les quatre points de justification (répétition, indépendance, probabilité constante, $X$ compte les succès).**

#### Réflexe 2 — Avec un tirage sans remise

**Je vérifie si la population est petite (pas binomiale) ou très grande devant l'échantillon (on assimile à une loi binomiale).**

#### Réflexe 3 — Devant « au moins » / « au plus »

**Je me ramène systématiquement à un évènement du type $P(X\leq k)$.**

#### Réflexe 4 — Pour les cas extrêmes

$$\boxed{P(X=0)=(1-p)^n\qquad P(X=n)=p^n}$$

se calculent directement, sans calculatrice.

---

## 10. Travail personnel avant la séance 4 (TG‑21)

Si l'élève a besoin de consolider :

- refaire les exercices 4, 9, 10 et 19 (reconnaissance et pièges) ;
- refaire les exercices 7, 8 et 15 (passage au complémentaire) ;
- revoir la formule de $P(X=k)$ et savoir l'utiliser sans erreur de signe dans $(1-p)^{n-k}$ ;
- relire le code Python des fonctions `loi` et `cumul`, et être capable de l'expliquer ligne par ligne.

### Pour le professeur

Noter à la fin de la séance :

- aisance à rédiger les quatre points de justification ;
- repérage correct des situations « sans remise » ;
- maîtrise du passage « au moins / au plus » → $P(X\leq k)$ ;
- lecture correcte de la formule de la loi binomiale ;
- compréhension du code Python ;
- autonomie face à un énoncé nouveau.

Ces observations serviront à préparer les séances TG‑21 (représenter, simuler, encadrer) et TG‑22 (espérance, variance, écart-type).

---

## 11. Suite des séances sur la loi binomiale

| Séance | Thème principal (fiche source) |
|---|---|
| **3** | **Reconnaître et calculer avec la loi binomiale** *(TG‑20)* |
| **4** | **Représenter, simuler et encadrer une loi binomiale** *(TG‑21)* |
| **5** | **Espérance, variance et écart-type de la loi binomiale** *(TG‑22)* |

> **Principe :** la séance 4 (TG‑21) s'appuie directement sur les fonctions Python `loi` et `cumul` vues aujourd'hui pour simuler la loi et déterminer des seuils/intervalles ; la séance 5 (TG‑22) reprend la formule de $P(X=k)$ pour construire l'espérance $E(X)=np$ et la variance $V(X)=np(1-p)$. Adapter le rythme selon l'aisance observée aujourd'hui sur la justification et le passage au complémentaire.
