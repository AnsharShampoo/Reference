# Reference de C++ para programación competitiva 
Este repositorio es donde mantengo todas las referencias necesarias para poder competir dentro de ICPC, CodeForces, etc. 
Está en construcción y la idea es hacerlo conforme avanzo con las clases.

Si algo te sirve o te encuentras en un apuro, siéntete libre de forkearlo o imprimirlo :)  
  
# Índice
* [Layout básico](#Plantilla)
## Matemáticas
* [Precisión de puntos decimales](#decimales)
* [Exponenciación Modular](#exponenciacion_modular)
* [Criba de Eratóstenes](#criba)
* [Criba en rangos](#criba_ab)
* [Criba Segmentada](#criba_segmentada)
* [Criba Lineal](#criba_lineal)
* [Algoritmo de Euclides (extendido)](#euclides_extendido)
* [Ecuaciones Diofánticas](#diofanticas)
## Algoritmos Comunes
* [Binary Search](#BS)
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
# Matemáticas
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
```

## Criba De Eratóstenes básica
<a name = "criba"></a>
Dado un número n , devuelve los números primos desde 0 hasta n en un vector de long long.

Implementación en O(n*log(log(n))):
```
vector<ll> criba(ll n){
    vector<ll> primos;
    vector<bool> no_primo(n+1, false);
    no_primo[0] = no_primo[1] = 1;
    for(int i=3; i*i<=n;i+=2){
        if(!no_primo[i]){
            for(int j = i*i; j<=n; j+=2*i){
                no_primo[j] = true;
            }
        }
    }
    primos.push_back(2);
    for(int i=3;i<n;i+=2){
        if(!no_primo[i])primos.push_back(i);
    }
    return primos;
}
```
## Criba para un rango [A,B]
<a name = "criba_ab"></a>
Dado un rango [A,B], encontrar todos los números primos en dicho rango.
```
vector<ll> criba_rango(ll a, ll b){
    
    vector<ll> primos = criba(sqrt(b)+1);
    vector<bool> no_primo(b-a+1);
    
    for(ll p: primos){
        ll primer_multiplo = p*max((ll)p, ll((a+p-1)/p));
        for(ll j = primer_multiplo; j<=b; j+=p){
            no_primo[j-a] = true;
        }
    }

    vector<ll> primos_en_rango;
    for(ll i = a;i<=b;i++){
        if(!no_primo[i-a]){
            primos_en_rango.push_back(i);
        }
    }
    return primos_en_rango;
}
```
## Criba Segmentada
<a name = "criba_segmentada"></a>
Dado un número n, obtener la cantidad de primos menores o iguales a n, aquí utilizamos espacio en memoria O(sqrt(n)) en lugar de la implementación normal con espacio en memoria O(n).
```
ll criba_segmentada(ll n){
    //Este número determinará la cantidad de segmentos(n/s), CUIDADO:
    const ll S = 1000;

    vector<ll> primos = criba(sqrt(n)+1);

    ll result = 0;
    vector<char> bloque(S);
    for(int k=0; k*S<=n;k++){
        fill(bloque.begin(), bloque.end(), true);
        ll start = k*S;
        for(auto p: primos){
            ll start_indx = (start+p-1)/p;
            ll j = max(start_indx, p) * p- start;
            for(;j<S;j+=p){
                bloque[j] = false;
            }
        }
        if(k == 0){
            bloque[0] = bloque[1] = false;
        }
        for(ll i =0; i<S && start+i<=n;i++){
            if(bloque[i]){
                result++;
            }
        }
    }
    return result;
}
```
## Criba Lineal
<a name = "criba_lineal"></a>
Dado un número n devuelve todos los primos menores o iguales a n en O(n)
```
vector<ll> criba_lineal(ll n){
    vector<ll> lp(n+1);
    vector<ll> primos;

    for(int i=2;i<n;i++){
        if(lp[i] == 0){
            lp[i] = i;
            primos.push_back(i);
        }

        for(int j=0;i*primos[j]<=n;j++){
            lp[i*primos[j]] = primos[j];
            if(primos[j] == lp[i]){
                break;
            }
        }
    }
    return primos;
}
```
## Algoritmo Extendido de Euclides
<a name = "euclides_extendido"></a>
Algoritmo de euclides para encontrar el gcd de dos números (a,b) y el par(x,y) tal que ax+by=gcd(a,b)
```
ll euclides_extendido(ll a, ll b, ll& x, ll& y){
    if(b == 0){
        x = 1;
        y = 0;
        return a;
    }
    ll x1, y1;
    ll d = euclides_extendido(b, a%b, x1, y1);
    x = y1;
    y = x1-y1*(a/b);
    return d;
}
```
## Ecuaciones Diofánticas
<a name = "diofanticas"></a>
Encontrar la solución/conjunto solución para la expresión ax+by = c. PRECISA DEL ALGORITMO DE EUCLIDES EXTENDIDO.
```
bool solucion_diofantica(ll a, ll b, ll c, ll &x0, ll &y0, ll &g){
    g = euclides_extendido(abs(a),abs(b),x0,y0);
    if(c%g){
        return false;
    }
    x0 += c/g;
    y0 += c/g;
    if(a<0)x0=-x0;
    if(b<0)y0=-y0;
    return true;
}
```
# Algoritmos Comunes
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

# Grafos
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