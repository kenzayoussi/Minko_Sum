# Minko_Sum

<p align='center'><img width="369" alt="Screen Shot 2022-05-27 at 14 22 09" src="https://user-images.githubusercontent.com/94130783/170708012-2651cd68-5378-4b61-ab9e-f95a1bd95ab4.png">
</img> </p>

# Table du contenu:
<ul>
  <li> <b> Introduction </b> </li> 
  <li> <b> Code séquentiel de la somme de Minkowski </b> </li>
  <li> <b> Code parallélisé de la somme de Minkowski </b> </li>
  <li> <b> Conclusion </b> </li>
  </ul>
  
  # Introduction:
 
  Nous nous sommes des fois posés dans des situations où nous utilisons plusieurs lignes de codes avec plusieurs variables, boucles,fonctions;or le meilleur programme est celui qui est exécuté le plus vite possible et ayant le minimum de lignes possibles. La solution idéale pour éxecuter un tel programme sera de le paralléliser,cela veut-dire le diviser en plusieurs fragments qui, eux mêmes, s’exécutent en même temps. 
 
Tout cela se réalisera à partir d'un Application Programming Interface (API) qui est OpenMp. L’interface permet effectivement de parraléliser un programme séquentiel écrit principalement en C ou C++, et ceci par plusieurs caréctéristique que l’environnement offre.

  Dans notre projet, nous travaillerons sur le parallélisme d’un code basé sur le rendu de la somme de Minkowski. Pour définir ce principe, nous considérons A et B deux espaces euclidiens.La somme de Minkowski est une opération sur les parties d'un espace euclidien, dans ce cas A et B.
 
<p align='center'> <img width="414" alt="Screen Shot 2022-05-27 at 14 38 15" src="https://user-images.githubusercontent.com/94130783/170710625-64316cc9-afda-4db1-bc10-cb2e952ad9ff.png"> </p>

La particularité de ce théorème demeure sur le monde de la géométrie, ou il permet de créer de nouvelles formes géométriques à partir de deux polyèdres.Nous prendrons un exemple pour mieux comprendre le principle. On suppose avoir deux triangles A et B dont les somment ont respectivements les coordonnés suivants:
<i> A=(0,-1), (0,1) et (1,0) et B=(0,0), (1,-1) et (1,1) </i>,représentés par les shémas suivants:

<p align='center'><img width="169" alt="Screen Shot 2022-05-27 at 15 02 44" src="https://user-images.githubusercontent.com/94130783/170715010-769ab7e3-9de0-4642-9147-f7cb0eb070a6.png"> </img> </p>
<p align='center'> <b> Présentation du point A </b> </p>

<p align='center'> <img width="173" alt="Screen Shot 2022-05-27 at 15 02 55" src="https://user-images.githubusercontent.com/94130783/170715576-f3755914-c21b-48c2-8f59-8c39c242507f.png"> </img> </p>
 </p>
 <p align='center'> <b> Représentation du point B </b> </p>
 
 Pour appliquer la somme de Minkowski,nous ferons l'addition de chaqu'un des coordonnées du polyèdre A avec tous ceux du polyèdre B.Nous obtenerons donc 9 sommets dont les coordonnées sont respectivement: <i> A + B = {(1, 0), (2, 1), (2, −1), (0, 1), (1, 2), (1, 0), (0, −1), (1, 0), (1, −2)} </i>. 
 <p align='center'> <img width="173" alt="Screen Shot 2022-05-27 at 15 21 33" src="https://user-images.githubusercontent.com/94130783/170718433-31569344-b6b6-4dfc-a3ef-1bd8950a3992.png"> </img> </p>
 <p align='center'> <b> Représentation du résultat de la somme de Minkowski sur A et B </b> </p>

# Code séquentiel de la somme de Minkowski:
Pour effectuer la somme de Minkowski,nous créons une structure point contenant de variables de type <i>long long</i> afin de créer les points des polygones avec lesquels nous travaillerons.Dans une deuxième étape,nous créerons une fonction au même nom qui prendera comme paramètre x et y entré par l'utilisateur.
  ```cpp
  struct pt{
    long long x, y;
    pt(long long x,long long y){
        this->x = x;
        this->y = y;
    }
  ```
Ensuite, nous créerons des oppérateurs pour simplifier le travail lors des fonctions principales.Ceux-ci sont présentés dans les lignes suivantes:
<ul>
  <li> <b> Opérateur d'adition:</b> </li> permet de faire l'addition entre le x entré comme paramètre et le x donné par l'utilisateur d'une part, et le y entré comme paramètre et de y donné par l'utilisateur d'une autre part.
  
  
  ```cpp
      point operator + (const pt & p) const {
        return pt{x + p.x, y + p.y};
    }
  ```
  <li> <b> Opérateur de soustraction:</b> </li> Comme l'étape précédente, on procède à la soustraction entre le paramétre x et le x donné par l'utilisateur, puis le paramètre y et le y donné par l'utilisateur.
  
  ```cpp
  pt operator - (const pt & p) const {
        return pt{x - p.x, y - p.y};
    }
  ```
 
  <li> <b> Opérateur cross:</b> </li> permet d'effectuer le produit vectoriel entre les x et les y.
  
  
  ```cpp
  long long cross(const pt & p) const {
        return x * p.y - y * p.x;
    }
  ```
  </ul>
  
 Une autre étape principale sera de créer une fonction qui permettera de ordonner les angles du polygones, ce qui va nous faciliter la procédure de la somme de Minkowski.A ce fait le paramétrer tre sera un vecteur de type point.Pour y procéder,nous implémentons une variable <b>position</b> de type <b>size_t</b> pour calculer le résultat en bytes,celle-ci est initialisée par 0.Afin de détérminer la position de chaque point des polygones,nous implémentons une boucle pour un compteur i allant de 1 jusq'à la taille du poolygone, et faire une condition si l'ordre de  l'ordonné du polygone est inférieur à sa position,ou qu'elles sont égales et que l'ordre de l'abscice est inférieur à sa position,donc l'ordre sera égal à la position.En ce fait,nous ferons une rotation entre la première partie du vecteur,puis celle-ci et la position et la dérnière partie partie du polygone.
