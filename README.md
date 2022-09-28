# Reference de C++ para programación competitiva 
Este repositorio es donde mantengo todas las referencias necesarias para poder competir dentro de ICPC, CodeForces, etc. 
Está en construcción y la idea es hacerlo conforme avanzo con las clases.

Si algo te sirve o te encuentras en un apuro, siéntete libre de forkearlo o imprimirlo :)  
  
# Índice
* [Layout básico](#Plantilla)
## Algoritmos Comunes
* [Binary Search](#BS)
## Matemáticas
* [Precisión de puntos decimales](#decimales)
* [Exponenciación Modular](#exponenciacion_modular)
## Grafos
* [DFS](#dfs)

## Layout Básico 
<a name="Plantilla"></a>
```
#include <bits/stdc++.h>
using namespace std;
//Fast and foriuos typing:
typedef long long ll;
//Constants:
const int MOD = 1000000007;
const ll INF = 1e18;

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    /*Quita los comentarios de las siguientes 
    líneas si estás haciendo problemas USACO: */
    //freopen(".in", "r", stdin);
    //freopen(".out", "w", stdout);
    
}
```
## Precisión de puntos decimales
<a name="decimales"></a>
```
cout<<fixed<<setprecision(decimales)<<n;
```
## Exponenciacón Modular
<a name ="exponenciacion_modular"></a>
Forma de calcular de manera eficiente *x^n mod m* en O(logn)
### Iterativo:
```
ll mod_pow(ll x, ll n, ll m){
    if (n == 0) return 1%m;
    ll u = mod_pow(x,n/2,m);
    u = (u*u)%m;
    if (n%2 == 1) u = (u*x)%m;
    return u;
}
```
### Recursivo:
```
ll mod_powi(ll a, ll b, ll m){
    ll result = 1;
    while(b>0){
        if(b%2 == 1) result = (result*a)%m;
        a = (a*a)%m;
        b /= 2;
    }
    return result;
}
}
```
## Binary Search
<a name="BS"></a>
Esta referencia es básica, recuerda que los parámetros pueden variar y muchas veces tendrás que hacer comparadores para hacer BS the answer.
```
int BinarySearch(vector<int> arreglo, int left, int right, int objetivo){
    while(left<=right){
        int mid = left+(right-left)/2; //Evitamos overflow al calcular el mid de esta manera.
        if(arreglo[mid] == objetivo){ //Encontraste lo que buscabas.
            return mid;
        }else if(arreglo[mid]>objetivo){ //El objetivo está a la derecha (es menor)
            right = mid-1;
        }else{ //El objetivo está a la izquierda (es mayor)
            left = mid+1;
        }
    }
    return -1; //Si llegamos aquí el elemento no existe en el arreglo
}
```

## DFS (Depth-First Search) 
<a name="dfs"></a>
Referencia básica para recorrer un grafo conexo y devolver la cantidad de nodos en este *(nota: recuerda inicializar el vector de visitados así como el vector de adyacencia grafo)*:

```
int dfs(int nodo){
    int numero_de_nodos =1;
    visitado[nodo] = true;
    for(auto vecino : grafo[nodo]){
        if(!visitado[vecino]){
            numero_de_nodos+=dfs(vecino);
        }
    }
    return numero_de_nodos;
}
```