# Road2 et Valhalla

_Road2_ est le moteur de calcul d'itinéraire développé par l'IGN utilisé par le service d'itinéraire et d'isochrone `V2` du Géoportail. Plutôt qu'un moteur en tant que tel, il s'agit plutôt d'un _méta-moteur_ dans la meseure où les calculs d'itinérraires et d'isochrones sont fait par d'autre moteurs open source, auxquels _Road2_ fait appel. En l'occurence, les moteurs actuellement implémentés sur la plateforme sont `OSRM`, un moteur extrêmeent performant au prix d'une configurabilité très limitée, et `pgRouting`, un moteur basé sur la technologie de base de données `PostgreSQL` qui permet à l'utilisateur faisant la requête de paramétrer cette dernière avec un haut niveau de personnalisation, au prix de la performance. Ce dernier moteur est également celui sur lequel se base le service d'isochrone de _Road2_.

## Situation actuelle du déploiement sur la plateforme

![Architecture logique résumée](service_card_sis2-sit2_logical_view.png)

### Architecture logique

### Limitations et contournements

Isodistance : 25 km
Isochrone piéton : 6 heures
Isochrone voiture : 11,5 minutes

## Une perspective : Valhalla

### Performances calculées
#### Isochrones avec un coût peu élevé :
PGR :
```
================================================================================
---- Global Information --------------------------------------------------------
> request count                                       1010 (OK=1007   KO=3     )
> min response time                                     40 (OK=43     KO=40    )
> max response time                                  14234 (OK=14234  KO=49    )
> mean response time                                   768 (OK=771    KO=45    )
> std deviation                                       1467 (OK=1469   KO=4     )
> response time 50th percentile                        105 (OK=106    KO=47    )
> response time 75th percentile                        770 (OK=796    KO=48    )
> response time 95th percentile                       3784 (OK=3798   KO=49    )
> response time 99th percentile                       6900 (OK=6901   KO=49    )
> mean requests/sec                                  1.012 (OK=1.009  KO=0.003 )
---- Response Time Distribution ------------------------------------------------
> t < 800 ms                                           755 ( 75%)
> 800 ms < t < 1200 ms                                  34 (  3%)
> t > 1200 ms                                          218 ( 22%)
> failed                                                 3 (  0%)
---- Errors --------------------------------------------------------------------
> status.find.in(200,304,201,202,203,204,205,206,207,208,209), b      3 (100,0%)
ut actually found 404
================================================================================
```

Valhalla :

```
================================================================================
---- Global Information --------------------------------------------------------
> request count                                       1037 (OK=1032   KO=5     )
> min response time                                    173 (OK=173    KO=176   )
> max response time                                   3204 (OK=3204   KO=220   )
> mean response time                                   239 (OK=239    KO=190   )
> std deviation                                        160 (OK=160    KO=16    )
> response time 50th percentile                        203 (OK=204    KO=184   )
> response time 75th percentile                        245 (OK=245    KO=187   )
> response time 95th percentile                        385 (OK=386    KO=213   )
> response time 99th percentile                        566 (OK=567    KO=219   )
> mean requests/sec                                  1.037 (OK=1.032  KO=0.005 )
---- Response Time Distribution ------------------------------------------------
> t < 800 ms                                          1025 ( 99%)
> 800 ms < t < 1200 ms                                   2 (  0%)
> t > 1200 ms                                            5 (  0%)
> failed                                                 5 (  0%)
---- Errors --------------------------------------------------------------------
> status.find.in(200,304,201,202,203,204,205,206,207,208,209), b      5 (100,0%)
ut actually found 404
================================================================================
```

#### Isochrones avec un coût élevé :
PGR :
```
================================================================================
---- Global Information --------------------------------------------------------
> request count                                        987 (OK=15     KO=972   )
> min response time                                      0 (OK=9432   KO=0     )
> max response time                                  58249 (OK=58249  KO=55330 )
> mean response time                                   658 (OK=30819  KO=193   )
> std deviation                                       5251 (OK=17284  KO=3022  )
> response time 50th percentile                          0 (OK=28787  KO=0     )
> response time 75th percentile                          0 (OK=45140  KO=0     )
> response time 95th percentile                          0 (OK=56729  KO=0     )
> response time 99th percentile                      38793 (OK=57945  KO=0     )
> mean requests/sec                                  0.933 (OK=0.014  KO=0.919 )
---- Response Time Distribution ------------------------------------------------
> t < 800 ms                                             0 (  0%)
> 800 ms < t < 1200 ms                                   0 (  0%)
> t > 1200 ms                                           15 (  2%)
> failed                                               972 ( 98%)
---- Errors --------------------------------------------------------------------
> j.u.c.TimeoutException: Request timeout to proxy.ign.fr/192.16    491 (50,51%)
8.4.9:3128 after 60000 ms
> j.u.c.TimeoutException: Request timeout to proxy.ign.fr/192.16    477 (49,07%)
8.4.22:3128 after 60000 ms
> status.find.in(200,304,201,202,203,204,205,206,207,208,209), b      4 ( 0,41%)
ut actually found 500
================================================================================
```

Valhalla :
```
================================================================================
---- Global Information --------------------------------------------------------
> request count                                        963 (OK=963    KO=0     )
> min response time                                    350 (OK=350    KO=-     )
> max response time                                  14102 (OK=14102  KO=-     )
> mean response time                                  2603 (OK=2603   KO=-     )
> std deviation                                       1367 (OK=1367   KO=-     )
> response time 50th percentile                       2304 (OK=2304   KO=-     )
> response time 75th percentile                       3171 (OK=3171   KO=-     )
> response time 95th percentile                       5045 (OK=5045   KO=-     )
> response time 99th percentile                       7346 (OK=7346   KO=-     )
> mean requests/sec                                  0.963 (OK=0.963  KO=-     )
---- Response Time Distribution ------------------------------------------------
> t < 800 ms                                            10 (  1%)
> 800 ms < t < 1200 ms                                  45 (  5%)
> t > 1200 ms                                          908 ( 94%)
> failed                                                 0 (  0%)
================================================================================
```

### Quelques résultats d'isochrones
### Limitations du moteur


## Quelques scénarios
### Scénario 0 : on ne change rien
### Scénario 1 : ajout de Valhalla dans l'architecture logique
#### 1.a : Valhalla sans modification du code source
#### 1.b : Valhalla avec modification du code source
### Scénario 2 : remplacement de pgRouting par Valhalla pour les isochrones
#### 2.a : Valhalla sans modification du code source
#### 2.b : Valhalla avec modification du code source

## Informations complémentaires
### Pourquoi ne pas utiliser Valhalla pour les itinéraires ?
