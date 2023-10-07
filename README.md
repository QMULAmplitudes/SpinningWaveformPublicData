# SpinningWaveformPublicData (Mathematica Version)
This repository present the gravitational waveform data at tree level for arbitrary spin without expansion the spin parameter.

## Waveform data for finite spin
The file for waveform data with exact result is in "resAllFinalq1New.txt" and "resAllFinalq2New.txt". 

"resAllFinalq1New.txt" is for the $q_1^2$ channel.
"resAllFinalq2New.txt" is for the $q_2^2$ channel. 
To get the full waveform contribution, you need to add them together.

```
integralq1 = Import["resAllFinalq1New.txt"] // ToExpression;
integralq2 = Import["resAllFinalq2New.txt"] // ToExpression;
integralAll = -I (integralq1 + integralq2) /. {a[1] -> -I at[1], S[1] -> -I St[1]} /. e[1] -> - (3/4) /. dot[at[1], v[2]] -> 0;
```
at[1] in the code donotes the $a_1$ in the paper.  St[1] in the code donotes the $i S_1$ in the paper. 

The output terms contains a variable $\omega$ for the frequency. Before substitude the numerical value of retarded time $u$, you need replace $\omega$ to $i\partial_u$ and acting on that term. The Mathematica code for this is 

```
integralNum=integralAll/.{SomeNumericalReplacementExcept_u};
resAllw2 = Coefficient[integralNum, \[Omega]^2];
resAllw1 = Coefficient[integralNum, \[Omega]] /. \[Omega] -> 0;
resAllw0 = integralNum /. \[Omega] -> 0;
resnum0 = resAllw0;
resnum1 = I D[resAllw1, u];
resnum2 = -D[D[resAllw2, u], u];
integralNum = resnum0 + resnum1 + resnum2;
```
Then you can generate the numerical data for use. Enjoy!

## Waveform data for small spin in Taylor expansion
The file for waveform data with exact result is in "resAllFinalq2NewSeries.txt" and "resAllFinalq2NewSeries.txt". Here we only put out the expanded result upto $a^4$ order. If you need the result for higher spin orders, please contact the authors. 

"resAllFinalq1NewSeries.txt" is for the $q_1^2$ channel.
"resAllFinalq2NewSeries.txt" is for the $q_2^2$ channel. 
Similarly, to get the full waveform contribution, you need to add them together and with some simple replacement as following mathematica code

```
integralq1Series = Import["resAllFinalq1NewSeries.txt"] // ToExpression;;
integralq2Series = Import["resAllFinalq2NewSeries.txt"] // ToExpression;
integralq1Seriesa4 =  Sum[Series[ integralq1Series[[ii]] /. {a[1] -> t a[1], S[1] -> t S[1]}, {t,  0, 4}] // Normal, {ii, Length@integralq1Series}];
integralq2Seriesa4 = Sum[Series[integralq2Series[[ii]] /. {a[1] -> t a[1], S[1] -> t S[1]}, {t, 0, 4}] // Normal, {ii, Length@integralq2Series}];
integralAll2 = -I (integralq1Seriesa4 + integralq2Seriesa4) /. t -> 1 /. e[1] -> - (3/4);
```
Similarly, you need replace the frequency to derivertive of retarded time properly 
```
integralNum=integralAll/.{SomeNumericalReplacementExcept_u};
integralNum = 
  CoefficientRules[integralNum, \[Omega]] /. 
    Rule[f1_, f2_] :> (I)^f1[[1]] D[f2, {u, f1[[1]]}] // Total;
```
The above code is a naive example, in practice you need more efficient code instead.



## waveform integrand after rescaling
The file for waveform integrand with exact result is in "rescaledAmpSaS0q1.txt" and "rescaledAmpSaS0q2Simple.txt". If you want to perform the integrand in other integration method, you can use the two code. 

"rescaledAmpSaS0q1.txt" is for the $q_1^2$ channel.
"rescaledAmpSaS0q2Simple.txt" is for the $q_2^2$ channel. 

To used the code, you need some subprogramms to evaluate the entire functions. The minimal subgrogrammes are included in "TestforUse.nb".


## Five point classical spin gravity amplitude
The five point classical spin amplitude is in "intMAllSimple.txt". 

## Four point classical spin gravity amplitude 
The four point classical spin amplitude is in "amp4ClassForUse.txt".  If you want to use the four point Compton amplitude in other gravitational progress, you need to load this file. All the entire function is expanded to $\sinh$ and $\cosh$ function. If you are interested in the form with original entire g-function. Please contact the authors. 
The notation for external kinematic data is shown in [arxiv:2309.11249](https://arxiv.org/abs/2309.11249),  [arxiv:2302.00498](https://arxiv.org/abs/2302.00498).














