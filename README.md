# Minko_Sum

<p align='center'><img width="369" alt="Screen Shot 2022-05-27 at 14 22 09" src="https://user-images.githubusercontent.com/94130783/170708012-2651cd68-5378-4b61-ab9e-f95a1bd95ab4.png">
</img> </p>

# Table du contenu:
<ul>
  <li> <b> Introduction </b> </li> 
  <li> <b> Code séquentiel de la somme de Minkowski </b> </li>
  <li> <b> Code parallélisé de la somme de Minkowski </b> </li>
  <li> <b> Comparaison entre le code parallèle et séquentiel </b> </li>
  </ul>
  
  # Introduction:
 
  <p> Nous nous sommes des fois posés dans des situations où nous utilisons plusieurs lignes de codes avec plusieurs variables, boucles,fonctions;or le meilleur programme est celui qui est exécuté le plus vite possible et ayant le minimum de lignes possibles. La solution idéale pour éxecuter un tel programme sera de le paralléliser,cela veut-dire le diviser en plusieurs fragments qui, eux mêmes, s’exécutent en même temps. </p>
 
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
 
  ```cpp
  void reorder_polygon(vector<point> & P){
    size_t pos = 0;
    for(size_t i = 1; i < P.size(); i++){
        if(P[i].y < P[pos].y || (P[i].y == P[pos].y && P[i].x < P[pos].x))
            pos = i;
    }
    rotate(P.begin(), P.begin() + pos, P.end());
}
   ```

Passons à la fonction principale pour réaliser la somme de Minkowski.Nous créons comme paramèttre deux vecteurs de type point puis les réordonner comme première étape en utilisant la foncntion précédente.En suite,nous remetterons en arrière chaqu'un du premier et du deuxième élément du vecteur.

 ```cpp
 vector<pt> minkowski(vector<point> P, vector<point> Q){
    
    reorder_polygon(P);
    reorder_polygon(Q);
 
    P.push_back(P[0]);
    P.push_back(P[1]);
    Q.push_back(Q[0]);
    Q.push_back(Q[1]);
  ```
Dans la partie principale de la fonction, nous créons un vecteur de type point pour afficher le résultat de notre oppération,puis nous déclarons deux variables compteurs de type <b>size_t</b>.Nous insérront le début du vecteur résultat dans la somme de P et Q tant que les deux compteurs sont succéssivement inférieurs à la taiile de premier vecteur ou celui du deuxième.Pour savoir si on devra incrémenter le premier ou le deuxième compteur,nous créons une variable qui calcul la soustraction entre le premier vecteur à un certain indice et son successeur et le produit verctoriel (<b>cross</b>) du deuxième vecteur à un certain indice moin son successeur.Si celui-ci et supérieur à 0,nous incrémenttons le premier compteur,sinon nous incrémentons le deuxième.En dérnière étape nous retournons le vecteur résultat.

```cpp
 vector<pt> result;
    size_t i = 0, j = 0;
    while(i < P.size() - 2 || j < Q.size() - 2){
        result.insert(result.begin(),P[i] + Q[j]);
        
        int cross = (P[i + 1] - P[i]).cross(Q[j + 1] - Q[j]);
        if(cross >= 0)
            ++i;
        if(cross <= 0)
            ++j;
    }
    

    return result;
}

```

Dans la fonction <b>main()</b> de notre programe,nous essayerons d'effectuer trois fois la somme de Minkowski donc on declare six vecteurs puis nouos essayons d'afficher les coordonnées de 500 variables aléatoires à partir de cette somme. Le code final que nous obtenons est le suivant:

```cpp
int main()
{
    vector<pt> P;
    vector<pt> Q;
    vector<pt> R;
    vector<pt> s;
    vector<pt> t;
    vector<pt> u;
    vector<pt> result;
 
    for (int i=0;i<500;i++){
        pt p1 = pt((rand() % 100) + 1,(rand() % 100) + 1);
        pt p2 = pt((rand() % 100) + 1,(rand() % 100) + 1);
        P.insert(P.begin(),p1) ;
        Q.insert(Q.begin(),p2);
    }
 
    for (int i=0;i<500;i++){
        pt p1 = pt((rand() % 100) + 1,(rand() % 100) + 1);
        pt p2 = pt((rand() % 100) + 1,(rand() % 100) + 1);
        s.insert(s.begin(),p1);
        t.insert(t.begin(),p2);
    }
 

R = minkowski(P,Q);
u = minkowski(s,t);
result=minkowski(R,u);


for(int i=0;i<R.size();i++){
  cout<<"x:"<<result[i].x<<" y:"<<result[i].y<<endl;
}
}
```

# Code Parallèle de la somme de Minkowski:
  Notre objectif principale dans ce projet est de pouvoir paralléliser le code présenté danns les lignes preécédentes.La première des choses qu'on puisse faire est de s'attaquer au boucles <i>for</i> et les parraléliser en utilisant <b>#pragma omp for</b>.En effet,sur la fonction du réerdonnement du polygone nous ajouterons dans la commande précédente la directive <b>shared(pos)</b> pour laisser la variable partagée. En efet, nous executerons notre nouveau code ayant un problème de faut partage,donc notre solution sera de controler les <b>threads </b> se trouvant dans la section parralèlle dans la fonction <b> reorder_polygon </b>. Le code sera donc le suivant:
  
 ```cpp
 void reorder_polygon(vector<point> & P){
    size_t pos = 0;
    int N = omp_get_thread_num();
    omp_set_num_threads(N);
    
    double s_priv[N * 8];

    #pragma omp parallel num_threads(N)
    {
        int t = omp_get_thread_num() ;
        #pragma omp parallel for shared(pos)
        for(size_t i = 1; i < P.size(); i++){
            if(P[i].y < P[pos].y || (P[i].y == P[pos].y && P[i].x < P[pos].x))
                s_priv[t * 8] = i;
    }
    rotate(P.begin(), P.begin() + pos, P.end());
    }
    for(size_t i = 0 ;i < N; i++){
        pos=s_priv[i * 8];
    }
 ```
Pour la fonction Minkowski,on specifie sur le <b> #pragma omp num_threads </b> le nombre de thread qu'on utilisera,puis le P et les Q en tant que variables partagés et les conteu puis l'oppérateur comme variables privés. Juste au dessus, nous considerons les lignes suivantes en tant que section critique en utilisant <b> #pragma omp critical. </b>

 ```cpp
     vector<point> result;
    size_t i = 0, j = 0;
    #pragma omp num_threads(N) default(none) shared(P,Q) private(i,j,cross)
     while(i < P.size() - 2 || j < Q.size() - 2){
 ```
 
 
  ```cpp
        #pragma omp critical
        {
            result.insert(result.begin(),P[i] + Q[j]);
        
            int cross = (P[i + 1] - P[i]).cross(Q[j + 1] - Q[j]);
        if(cross >= 0)
            ++i;
        if(cross <= 0)
            ++j;
        }
       
      }
   ```
   
Pour la fonction principale main,nous avons ajouter les <b> pragma omp for </b> afin de parelléliser les boucles.

 ```cpp
 #pragma omp for
    for (int i=0;i<500;i++){
        point p1 = point((rand() % 100) + 1,(rand() % 100) + 1);
        point p2 = point((rand() % 100) + 1,(rand() % 100) + 1);
        P.insert(P.begin(),p1) ;
        Q.insert(Q.begin(),p2);
 
  ```

 ```cpp
 #pragma omp for
    for (int i=0;i<500;i++){
        point p1 = point((rand() % 100) + 1,(rand() % 100) + 1);
        point p2 = point((rand() % 100) + 1,(rand() % 100) + 1);
        s.insert(s.begin(),p1);
        t.insert(t.begin(),p2);
    }
  ```
# Comparaison entre le code parallèle et séquentiel

### Pour 500 variables aléatoires:
Temps d'execussion séquentiel:1.623
<p align='center'> <img width="605" alt="Screen Shot 2022-06-04 at 03 09 59" src="https://user-images.githubusercontent.com/94130783/171973095-415e06f2-9f34-433b-91ac-37d990be1c40.png"> </p>

### Pour 2000 variables aléatoires:
Temps d'execussion séquentiel:7.542
<p align='center'> <img width="605" alt="Screen Shot 2022-06-04 at 03 11 58" src="https://user-images.githubusercontent.com/94130783/171973823-c2205c12-cd7a-47c6-93c7-5b20e49f095c.png"> </p>

