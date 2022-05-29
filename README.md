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
