---
title: "Assignment 1 Answer Key"
permalink: /3040/assign1ans/
excerpt: "Sales from different video game consoles - Answer Key"
toc: false
---

------------------------------------------------------------------------

### Question 1

There are several ways to create the subsample, but the one recommended in class is:

```r
sub <- subset(mydata, Platform == "Wii" | Platform == "PS3" | Platform == "X360")
```

### Question 2

Students should use code that looks something like:

```r
plot(sub$Score, sub$Sales)
```

and include the scatterplot in their assignment.

### Question 3

This question is asking students to estimate the $$\beta_1$$ in a model such as:

$$Sales = \beta_0 + \beta_1Score + \epsilon$$

They could estimate the model using something like:

```r
mod1 <- lm(Sales ~ Score, data = sub) 
summary(mod1)
```
Then, using the output:
```
Call:
lm(formula = Sales ~ Score, data = sub)

Residuals:
   Min     1Q Median     3Q    Max 
-2.345 -1.069 -0.563  0.204 81.203 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) -2.50690    0.44308  -5.658 1.84e-08 ***
Score        0.54082    0.06062   8.921  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3.303 on 1445 degrees of freedom
Multiple R-squared:  0.0522,	Adjusted R-squared:  0.05155 
F-statistic: 79.59 on 1 and 1445 DF,  p-value: < 2.2e-16
```

should say something like: "Video games that score 1 point higher, make an additional $540,820 in sales, on average."

### Question 4

The R-square of 0.0522 (or 0.05155) should be interpreted as: "Score explains only 5.2% of the variation in video game sales."

### Question 5

This hypothesis has already been tested in the `summary()` function above from question 3. The t-stat is 8.921, and p-value is 0.000. The null hypothesis is rejected.

### Question 6

There are several ways students might approach this. Here is a possible way:

```r
mod2 <- lm(Sales ~ Score + Platform, data = sub)
summary(mod2)
```

```
Call:
lm(formula = Sales ~ Score + Platform, data = sub)

Residuals:
   Min     1Q Median     3Q    Max 
-2.545 -1.063 -0.496  0.297 80.541 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept)  -2.92520    0.47580  -6.148 1.01e-09 ***
Score         0.57737    0.06111   9.448  < 2e-16 ***
PlatformWii   0.79889    0.23406   3.413  0.00066 ***
PlatformX360 -0.08173    0.19973  -0.409  0.68247    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3.286 on 1443 degrees of freedom
Multiple R-squared:  0.06336,	Adjusted R-squared:  0.06141 
F-statistic: 32.54 on 3 and 1443 DF,  p-value: < 2.2e-16
```

Students may exclude the `Score` variable, that's ok. The important part is that students are able to interpret the coefficients on the dummy variables. For example, because `PS3` is the base group, `Wii` games are selling $798,890 more on average compared to `PS3`, and `X360` games are selling $81,730 less on average compared to `PS3`.

### Question 7

To avoid perfect multicollinearity, or the "dummy variable trap".

### Question 8

Students should use the intitial downloaded dataset, and include the extra variables in the model:

```r
mod3 <- lm(Sales ~ Score + Publisher + Platform + Genre, data = mydata)
summary(mod3)
```

The output is large and is provided below, but the important thing is that students pick out the `0.417665` estimate and interpret it as "an increase in score of 1 leads to an increase in sales of $417,665 on average". For the second part of the question, students need say that the additional variables are included in order to avoid _omitted variable bias_.

```
Call:
lm(formula = Sales ~ Score + Publisher + Platform + Genre, data = mydata)

Residuals:
    Min      1Q  Median      3Q     Max 
-19.049  -0.824  -0.215   0.386  78.078 

Coefficients:
                                                  Estimate Std. Error t value Pr(>|t|)    
(Intercept)                                      -3.677345   2.497256  -1.473 0.140943    
Score                                             0.417665   0.029722  14.053  < 2e-16 ***
Publisher1C Company                               1.226767   3.035447   0.404 0.686124    
Publisher2K Games                                 1.159050   2.511991   0.461 0.644530    
Publisher2K Sports                                0.346345   2.499503   0.139 0.889800    
Publisher3909 LLC                                 3.348786   3.502051   0.956 0.339006    
Publisher3DO                                      1.233799   2.651379   0.465 0.641709    
Publisher505 Games                                0.370042   2.528232   0.146 0.883641    
Publisher989 Studios                              1.399453   2.682985   0.522 0.601973    
PublisherAcclaim Entertainment                    0.962322   2.507175   0.384 0.701125    
PublisherAccolade                                 0.314771   3.500372   0.090 0.928351    
PublisherActivision                               1.381814   2.482861   0.557 0.577869    
PublisherActivision Value                         2.338334   3.499999   0.668 0.504107    
PublisherAgetec                                   0.571534   2.572720   0.222 0.824206    
PublisherAksys Games                              0.409794   2.530198   0.162 0.871344    
PublisherAmanita Design                           1.511114   3.497616   0.432 0.665733    
PublisherAQ Interactive                           1.615357   3.496149   0.462 0.644076    
PublisherArc System Works                         0.274781   3.483000   0.079 0.937122    
PublisherASCII Entertainment                      0.526510   2.678927   0.197 0.844198    
PublisherAspyr                                    1.172824   2.769422   0.423 0.671958    
PublisherAtari                                    1.253157   2.494429   0.502 0.615424    
PublisherAtlus                                    0.459233   2.488849   0.185 0.853617    
PublisherBAM! Entertainment                       1.378259   2.590568   0.532 0.594732    
PublisherBandai                                   0.971852   2.554172   0.380 0.703596    
PublisherBandai Namco Entertainment               0.175897   2.658268   0.066 0.947246    
PublisherBandai Namco Games                      -0.018041   2.852049  -0.006 0.994953    
PublisherBeatnik Games                            0.717628   3.495952   0.205 0.837368    
PublisherBethesda Softworks                       1.798484   2.501625   0.719 0.472224    
PublisherBig Sandwich Games                       1.413533   3.498062   0.404 0.686166    
PublisherBlizzard Entertainment                   3.622767   2.548326   1.422 0.155205    
PublisherBroderbund                               6.525215   3.497444   1.866 0.062148 .  
PublisherBroken Rules                             0.383311   3.497373   0.110 0.912732    
PublisherBuena Vista                              1.340646   2.580266   0.520 0.603385    
PublisherCapcom                                   0.831288   2.484821   0.335 0.737983    
PublisherCDV Software Entertainment               0.830430   2.765587   0.300 0.763983    
PublisherChucklefish                              4.542484   3.499398   1.298 0.194329    
PublisherCity Interactive                         1.421956   2.770375   0.513 0.607787    
PublisherCodemasters                              0.448051   2.501564   0.179 0.857861    
PublisherConspiracy Entertainment                 0.731792   2.769438   0.264 0.791608    
PublisherCrave Entertainment                      1.598982   2.530268   0.632 0.527458    
PublisherCrystal Dynamics                         0.045444   3.494282   0.013 0.989624    
PublisherD3 Publisher                             0.714431   2.543122   0.281 0.778780    
PublisherD3Publisher                              3.762067   3.516037   1.070 0.284690    
PublisherDaedalic Entertainment                   1.249401   3.497776   0.357 0.720960    
PublisherDark Energy Digital                      1.467659   3.497017   0.420 0.674733    
PublisherDeep Silver                              1.686538   2.554230   0.660 0.509101    
PublisherDestination Software, Inc                1.056737   2.677086   0.395 0.693058    
PublisherDestineer                                0.641930   2.714202   0.237 0.813050    
PublisherDevolver Digital                         1.726632   2.862348   0.603 0.546392    
PublisherDisney Interactive Studios               1.068324   2.504893   0.426 0.669768    
PublisherDotEmu                                   0.382261   3.481762   0.110 0.912581    
PublisherDreamCatcher Interactive                 0.995877   2.861705   0.348 0.727856    
PublisherDSI Games                                1.530949   2.863419   0.535 0.592913    
PublisherEA Sports                                0.843221   2.488567   0.339 0.734748    
PublisherEA Sports BIG                            0.524214   2.602605   0.201 0.840380    
PublisherEdmund McMillen                          7.278196   3.495652   2.082 0.037393 *  
PublisherEidos Interactive                        1.052090   2.497939   0.421 0.673642    
PublisherElectronic Arts                          1.225945   2.482626   0.494 0.621465    
PublisherEmpire Interactive                       2.618539   3.502952   0.748 0.454788    
PublisherEncore                                   1.564239   2.860030   0.547 0.584454    
PublisherEnix                                     1.183697   2.768873   0.428 0.669035    
PublisherESP                                      0.435318   3.129397   0.139 0.889373    
PublisherExcalibur Publishing                     2.858509   3.506462   0.815 0.414995    
PublisherExtend Studio                            0.674510   3.497901   0.193 0.847099    
PublisherFacepunch Studios                       10.299710   3.508551   2.936 0.003346 ** 
PublisherFDG Entertainment                        0.414496   3.491947   0.119 0.905518    
PublisherFocus Home Interactive                   1.669030   2.771944   0.602 0.547128    
PublisherFortyfive                                2.722781   3.525086   0.772 0.439918    
PublisherFox Interactive                          0.897387   2.718248   0.330 0.741314    
PublisherFrom Software                            1.088142   3.037678   0.358 0.720200    
PublisherFrozenbyte                               1.380905   3.505330   0.394 0.693641    
PublisherFuncom                                   0.539915   3.030041   0.178 0.858584    
PublisherGaijin Entertainment                     0.968294   3.033811   0.319 0.749615    
PublisherGame Factory                             2.185052   3.501101   0.624 0.532591    
PublisherGamecock                                 1.048526   2.650603   0.396 0.692434    
PublisherGameMill                                 2.275516   3.499080   0.650 0.515520    
PublisherGameMill Entertainment                   2.655700   3.487825   0.761 0.446447    
PublisherGameTrust                                1.728376   3.482005   0.496 0.619655    
PublisherGarageGames                              0.159778   3.497321   0.046 0.963563    
PublisherGathering of Developers                  0.198610   2.769674   0.072 0.942837    
PublisherGenius Products, Inc.                    2.001430   3.502595   0.571 0.567749    
PublisherGlobal Star Software                     2.711139   2.770774   0.978 0.327892    
PublisherGotham Games                             1.156528   2.770933   0.417 0.676422    
PublisherGraffiti                                 0.737505   2.768744   0.266 0.789968    
PublisherGrey Box                                 0.909682   3.025428   0.301 0.763674    
PublisherGT Interactive                           1.798212   2.652919   0.678 0.497919    
PublisherGungHo Online Entertainment             -1.096298   3.512338  -0.312 0.754958    
PublisherHasbro Interactive                       1.266135   3.038246   0.417 0.676894    
PublisherHello Games                              1.018402   3.512130   0.290 0.771855    
PublisherHemisphere Games                         0.820311   3.498555   0.234 0.814630    
PublisherHidden Path Entertainment                1.524701   3.497864   0.436 0.662934    
PublisherHip Interactive                          1.842365   2.773280   0.664 0.506516    
PublisherHot-B                                   -0.083140   3.491923  -0.024 0.981006    
PublisherHudson Entertainment                     1.439258   2.768962   0.520 0.603241    
PublisherHudson Soft                              1.167299   2.650620   0.440 0.659678    
PublisherHuman Entertainment                     -0.480974   3.500406  -0.137 0.890717    
Publisherid Software                              0.149366   3.495850   0.043 0.965921    
PublisherIgnition Entertainment                   0.632408   2.558683   0.247 0.804795    
PublisherInfogrames                               1.150662   2.573561   0.447 0.654818    
PublisherInterplay                                1.007623   2.609854   0.386 0.699453    
PublisherInti Creates                             0.356021   3.496806   0.102 0.918909    
PublisherInvisible Handlebar                      1.803245   3.498468   0.515 0.606272    
PublisherJaleco                                   1.457101   2.771087   0.526 0.599038    
PublisherJoWood Productions                       1.079362   3.027660   0.357 0.721483    
PublisherKalypso                                  0.953243   2.614531   0.365 0.715432    
PublisherKalypso Media                            0.616432   3.495998   0.176 0.860047    
PublisherKemco                                    1.940703   2.679302   0.724 0.468901    
PublisherKnowledge Adventure                      1.846158   3.032887   0.609 0.542746    
PublisherKoch Media                               0.557031   3.496296   0.159 0.873424    
PublisherKOEI                                     0.822889   2.555949   0.322 0.747505    
PublisherKoei Tecmo                              -0.148050   2.759017  -0.054 0.957208    
PublisherKonami                                   0.917851   2.486494   0.369 0.712045    
PublisherKonami Digital Entertainment             0.468785   2.582010   0.182 0.855938    
PublisherLazy 8 Studios                           1.794685   3.502377   0.512 0.608383    
PublisherLEGO Media                               1.100098   3.497041   0.315 0.753096    
PublisherLionhead Studios                         0.090150   3.499470   0.026 0.979449    
PublisherLucasArts                                1.622012   2.500744   0.649 0.516623    
PublisherMad Catz                                 3.238821   3.501741   0.925 0.355059    
PublisherMajesco                                  1.332584   2.508583   0.531 0.595300    
PublisherMarvelous Interactive                    0.873365   2.857669   0.306 0.759907    
PublisherMastiff                                  1.343759   2.768420   0.485 0.627425    
PublisherMatt Makes Games Inc.                   -0.144354   3.497211  -0.041 0.967077    
PublisherMattel Interactive                      -0.500485   3.500096  -0.143 0.886303    
PublisherMaximum Games                           -0.459800   3.513716  -0.131 0.895893    
PublisherMaxis                                    1.034698   2.861990   0.362 0.717720    
PublisherMerge Games                              0.392730   3.015167   0.130 0.896373    
PublisherMeridian4                                2.655363   3.501215   0.758 0.448245    
PublisherMetro 3D                                 1.062790   3.493962   0.304 0.761006    
PublisherMicroids                                 1.012133   3.019098   0.335 0.737457    
PublisherMicroprose                              -0.118683   3.499402  -0.034 0.972946    
PublisherMicrosoft                                1.710836   2.573548   0.665 0.506228    
PublisherMicrosoft Game Studios                   2.287129   2.500553   0.915 0.360426    
PublisherMicrosoft Studios                        1.459907   2.565610   0.569 0.569365    
PublisherMidway Games                             0.835527   2.498873   0.334 0.738123    
PublisherMindscape                                0.277535   2.716069   0.102 0.918616    
PublisherMojang                                  16.768225   3.035822   5.523 3.52e-08 ***
PublisherMTV Games                                0.590963   2.547540   0.232 0.816569    
PublisherMumbo Jumbo                              1.840876   3.033689   0.607 0.544007    
PublisherNamco                                    1.057273   2.499364   0.423 0.672304    
PublisherNamco Bandai                             0.854485   2.494836   0.343 0.731990    
PublisherNamco Bandai Games                       0.935326   2.560316   0.365 0.714893    
PublisherNatsume                                  0.847665   2.519975   0.336 0.736601    
PublisherNCSoft                                   2.378327   2.857210   0.832 0.405231    
PublisherNEC Interchannel                         1.313337   3.497616   0.375 0.707310    
PublisherNewKidCo                                 1.539893   3.033432   0.508 0.611731    
PublisherNicalis                                  0.290034   2.752394   0.105 0.916083    
PublisherNihon Falcom Corporation                -1.171962   3.500014  -0.335 0.737758    
PublisherNintendo                                 3.812638   2.479230   1.538 0.124161    
PublisherNippon Ichi Software                     1.201515   3.028282   0.397 0.691560    
PublisherNIS America                              0.526492   2.496826   0.211 0.833003    
PublisherNobilis                                  0.031545   3.497346   0.009 0.992804    
PublisherNordic Games                            -0.041346   2.866574  -0.014 0.988493    
PublisherNumber None                              0.739179   3.497224   0.211 0.832615    
PublisherO~3 Entertainment                        0.679312   3.499592   0.194 0.846097    
PublisherO3 Entertainment                         0.488949   3.495740   0.140 0.888769    
PublisherOcean                                    1.604337   3.041881   0.527 0.597931    
PublisherOutright Games                           1.010994   3.492654   0.289 0.772241    
PublisherParadox Interactive                      0.802383   2.649373   0.303 0.762013    
PublisherPerfect World Entertainment              0.282249   3.491890   0.081 0.935581    
PublisherPhantom EFX                              1.088861   3.496132   0.311 0.755475    
PublisherPlaylogic Game Factory                   1.356637   2.714519   0.500 0.617262    
PublisherPlaymates                                0.099283   3.500349   0.028 0.977373    
PublisherPM Studios                               0.625343   2.869883   0.218 0.827518    
PublisherPolykid                                  1.404658   3.483285   0.403 0.686779    
PublisherPopCap Games                             1.339135   2.679553   0.500 0.617269    
PublisherPsygnosis                                0.005561   2.635142   0.002 0.998316    
PublisherPsyonix Studios                         -0.418412   3.497423  -0.120 0.904778    
PublisherPuppy Games                              1.304701   3.497864   0.373 0.709167    
PublisherRare                                     1.252279   2.869600   0.436 0.662572    
PublisherRed Mile Entertainment                   1.294421   3.495470   0.370 0.711166    
PublisherRedOctane                                3.046039   2.653072   1.148 0.250982    
PublisherRising Star                              1.138297   3.482711   0.327 0.743803    
PublisherRising Star Games                        1.141540   3.501049   0.326 0.744397    
PublisherRockstar Games                           3.412626   2.500706   1.365 0.172428    
PublisherSam Barlow                               0.918748   3.497464   0.263 0.792802    
PublisherSammy Corporation                        0.847318   3.498674   0.242 0.808650    
PublisherSCS Software                             5.904850   3.499422   1.687 0.091601 .  
PublisherSega                                     1.007517   2.483926   0.406 0.685046    
PublisherShin'en                                  1.163651   3.505714   0.332 0.739958    
PublisherSidhe Interactive                        1.215253   3.502006   0.347 0.728596    
PublisherSierra Entertainment                     0.914374   2.526001   0.362 0.717381    
PublisherSNK                                      0.637055   3.036961   0.210 0.833859    
PublisherSNK Playmore                             0.858272   3.497529   0.245 0.806163    
PublisherSoedesco                                -0.286004   3.481558  -0.082 0.934533    
PublisherSold Out                                 0.274967   3.484960   0.079 0.937115    
PublisherSony Computer Entertainment              1.339119   2.485397   0.539 0.590056    
PublisherSony Computer Entertainment America      1.919680   2.878287   0.667 0.504837    
PublisherSony Interactive Entertainment           1.951110   2.612454   0.747 0.455194    
PublisherSony Online Entertainment                1.273422   2.635220   0.483 0.628955    
PublisherSouthPeak Interactive                    0.749755   2.542608   0.295 0.768102    
PublisherSquare                                   0.127863   2.570285   0.050 0.960327    
PublisherSquare EA                                1.492163   2.573211   0.580 0.562023    
PublisherSquare Enix                              1.157608   2.486449   0.466 0.641549    
PublisherSSI                                      1.007328   3.502195   0.288 0.773645    
PublisherStardock                                 0.618802   3.497667   0.177 0.859581    
PublisherStrategy First                           1.788265   3.498953   0.511 0.609317    
PublisherStudio Wildcard                          2.125158   3.495096   0.608 0.543192    
PublisherTaito                                   -0.279820   3.495983  -0.080 0.936209    
PublisherTake-Two Interactive                     1.241515   2.588561   0.480 0.631524    
PublisherTDK Core                                 1.525582   3.495975   0.436 0.662581    
 [ reached 'max' / getOption("max.print") -- omitted 93 rows ]
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 2.459 on 4413 degrees of freedom
Multiple R-squared:  0.2548,	Adjusted R-squared:  0.2055 
F-statistic: 5.167 on 292 and 4413 DF,  p-value: < 2.2e-16
```

