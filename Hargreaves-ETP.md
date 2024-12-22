(Source des équations : FAO paper 56)

### **Formule de Hargreaves** : 

$ET_{Ha}=0,0023(T_a+17,8)(T_x-T_n)^0,5R_{g,0}$

$R_{g,0}$ : Rayonnement solaire extra terrestre (mm/j)

$T_a$ : Température moyenne de l'air (°C)

$T_x$ : Température maximale (°C)

$T_n$ : Température minimale (°C)


Pour obtenir le rayonnement solaire extra terrestre : 
$R_a=\frac{24(60)}{\pi}G_{sc}d_r[\omega_s.sin(\phi)+cos(\delta).sin(\omega_s)]$

$R_a$ : radiation extraterrestre (MJ/m²/j)

$G_{sc}$ : Constante solaire : 0,082 MJ/m²/min

$d_r$ : inverse de la distance relative terre / soleil : $1+0,033cos(\frac{2pi}{365}J)$  avec $J$ N° du jour dans l'année

$\omega_s$ : angle heure soleil couchant : $arccos|-tan(\phi)tan(\delta)]$

$\phi$ : latitude (rad) : $\frac{pi}{180}[decimal deg]$




```julia
# Mise en application avec Mauguio : 43°37'01'' N, 4°00'33'' E
# Conversion de la latitude en degrés décimaux 
lat=43+(37/60)+(01/3600)
lat_rounded = round(lat, digits=4)
println(lat_rounded)

```

    43.6169
    


```julia
# Détermination de l'angle phi
phi=lat*(pi/180)
phi_rounded = round(phi, digits=4)
println(phi_rounded)

```

    0.7613
    

Détermination de l'angle $\delta$ : 

$\delta=0,409sin(\frac{2\pi}{365}J-1,39)$



```julia
#Détermination de delta
# # Trouver le numéro du jour : 
using Dates

t = Date(2024, 12, 3)
day_number = Dates.dayofyear(t)
print(day_number)

```

    338


```julia
# Calcul de delta
delta = 0.409 * sin((2 * π) / 365 * day_number - 1.39)

# Afficher delta avec une précision de 6 décimales
delta_rounded = round(delta, digits=4)
println(delta_rounded)




```

    -0.3926
    

Convertir d'abord phi et delta en radians !


```julia
deltaR = delta_rounded*π/180
deltaR_rounded = round(deltaR, digits=4)

phiR = phi_rounded*π/180
phiR_rounded = round(phiR, digits=4)
println(deltaR_rounded)
println(phiR_rounded)
```

    -0.0069
    0.0133
    

Avec $\delta$ et $\phi$ déterminés, on peut trouver $\omega$ : 
$\omega=arccos|-tan(\phi)tan(\delta)]$


```julia
omega = acos(-tan(phiR_rounded)*tan(deltaR_rounded))
omega_rounded = round(omega, digits=4)
println(omega_rounded)
```

    1.5707
    

Avec ça, on peut enfin déterminer Ra comme suit : 

$R_a=\frac{24(60)}{\pi}G_{sc}d_r[\omega_s.sin(\phi)+cos(\delta).sin(\omega_s)]$


```julia
using Printf

# Constantes et variables d'entrée
day_number = 338                      # Numéro du jour (exemple : 3 décembre)
phiR_rounded = 43.6169 * π / 180      # Latitude en radians
deltaR_rounded = -0.409              # Delta (déjà en radians)
omega_rounded = 1.047                # Exemple : omega en radians

# Constante solaire
Gsc = 0.082  # Constante solaire [MJ/m^2/min]

# Calcul de Ra
Ra = ((24 * 60) / π) * Gsc * (1 + 0.033 * cos((2 * π / 365) * day_number)) *  (omega_rounded * sin(phiR_rounded) * sin(deltaR_rounded) + cos(phiR_rounded) * cos(deltaR_rounded) * sin(omega_rounded))

Ra_rounded = round(Ra, digits=4)
print(Ra_rounded)


```

    11.1426

On obtient un $Ra$ de 11,1426 MJ/m²/j

On peut enfin appliquer la  **Formule de Hargreaves** : 

$ET_{Ha}=0,0023(T_a+17,8)(T_x-T_n)^0,5R_{g,0}$

$R_{g,0}$ : Rayonnement solaire extra terrestre (mm/j)

$T_a$ : Température moyenne de l'air (°C)

$T_x$ : Température maximale (°C)

$T_n$ : Température minimale (°C)



```julia
Tx=8.4
Tn=6.2
Ta=(Tx+Tn)/2
Ta_r = round(Ta, digits = 4)

print(Ta_r)
```

    7.3


```julia
ETh = 0.0023 * (Ta_r + 17.8) * (Tx - Tn)^0.5 * Ra_rounded
ETh_r = round(ETh, digits = 3)
print(ETh_r)
```

    0.954

Avec la méthode de Hargreaves, on obtient, avec les données du 3/12/2022, un ETP de 0,954mm/j

La prochaine étape est de faire la même manoeuvre pour toute l'année 2022. 


```julia

```
