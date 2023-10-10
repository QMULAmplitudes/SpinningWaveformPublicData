# SpinningWaveformPublicData (Mathematica Version)
This is the companion GitHub repository for [arXiv:2310.04405](https://arxiv.org/abs/2310.04405) .
This repository presents the gravitational waveform data at tree level for arbitrary spin without expansion the spin parameter.

## Waveform data for finite spin
The file for waveform data with exact result is in "resAllFinalq1New.txt" and "resAllFinalq2New.txt". 

"resAllFinalq1New.txt" is for the $q_1^2$ channel.
"resAllFinalq2New.txt" is for the $q_2^2$ channel. 
To get the full waveform contribution, you need to add them together.

```
integralq1 = Import["resAllFinalq1New.txt"] // ToExpression;
integralq2 = Import["resAllFinalq2New.txt"] // ToExpression;
integralAll = I (integralq1 + integralq2) /. {a[1] -> -I at[1], S[1] -> -I St[1]} /. e[1] -> - (3/4) /. dot[at[1], v[2]] -> 0;
```
at[1] in the code donotes the $\tilde{a}_1$ in the paper.  St[1] in the code donotes the $i S_1$ in the paper. The factor e[1] is a numerical coefficient the last contact term in equation (3.9) of the paper, which contributes at order $a^5$ and above. It can be fixed to -(3/4) using spin-shift symmetry, or can be changed if one wanted to relax this symmetry.

In practice, one needs to add a prefactor of ${\kappa^4 m_1 m_2 \over 8\pi}$ to match with eq(5.33), for example.
The output terms contain a variable $\omega$ for the frequency. Before substiting the numerical value of retarded time $u$, you need replace $\omega$ with $i\partial_u$ and acting on that term. The Mathematica code for this is 

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

## Waveform data for small spin Taylor expansion
The file for waveform data expanded in powers of spin is in "resAllFinalq2NewSeries.txt" and "resAllFinalq2NewSeries.txt". Here we only put out the expanded result upto $a^4$ order. If you need the result for higher spin orders, please contact the authors. 

"resAllFinalq1NewSeries.txt" is for the $q_1^2$ channel.
"resAllFinalq2NewSeries.txt" is for the $q_2^2$ channel. 
Similarly, to get the full waveform contribution, you need to add them together and with some simple replacement using the following mathematica code

```
integralq1Series = Import["resAllFinalq1NewSeries.txt"] // ToExpression;
integralq2Series = Import["resAllFinalq2NewSeries.txt"] // ToExpression;
integralq1Seriesa4 =  Sum[Series[ integralq1Series[[ii]] /. {a[1] -> t a[1], S[1] -> t S[1]}, {t,  0, 4}] // Normal, {ii, Length@integralq1Series}];
integralq2Seriesa4 = Sum[Series[integralq2Series[[ii]] /. {a[1] -> t a[1], S[1] -> t S[1]}, {t, 0, 4}] // Normal, {ii, Length@integralq2Series}];
integralAll2 = -I (integralq1Seriesa4 + integralq2Seriesa4) /. t -> 1 /. e[1] -> - (3/4);
```
Similarly to the unexpanded waveform, you need to replace the frequency $\omega$ with a derivative in the retarded time $u$: 
```
integralNum=integralAll/.{SomeNumericalReplacementExcept_u};
integralNum = 
  CoefficientRules[integralNum, \[Omega]] /. 
    Rule[f1_, f2_] :> (I)^f1[[1]] D[f2, {u, f1[[1]]}] // Total;
```
The above code is a naive example, in practice you may do better with more efficient code instead.



## waveform integrand after rescaling
The file for waveform integrand is in "rescaledAmpSaS0q1.txt" and "rescaledAmpSaS0q2Simple.txt". If you want to perform the integration in another way, you can use the two files: 

"rescaledAmpSaS0q1.txt" is for the $q_1^2$ channel.
"rescaledAmpSaS0q2Simple.txt" is for the $q_2^2$ channel. 

To used the code, you need some subprogramms to evaluate the entire functions. The minimal subprogrammes are included in "TestforUse.nb".


## Five-point classical spin gravity amplitude
The five-point classical spin amplitude is in "intMAllSimple.txt". 
To load the five-point result use the following code:
```
intMAll = Import["intMAllSimple.txt"] // ToExpression;
intResHEFTFinal = intMAll[[1]];
props = intMAll[[2]];
fivePointAmplitude = 
  intResHEFTFinal /. 
      j[gr1, f__] :> Times @@ MapThread[Power, {props, -{f}}]  // Expand;
```

## Four-point Compton classical spin gravity amplitude 
The four-point classical spin amplitude is in "amp4ClassForUse.txt".  If you want to use the four-point Compton amplitude in other gravitational processes, you need to load this file. All the entire functions are expanded to $\sinh$ and $\cosh$ functions. The factors e[1], e[2], e[3] are numerical coefficients of the contact terms appearing in equation (3.9) and (3.11) of the paper. To enforce spin shift symmetry you can set $e[1]=-3/4,e[2]=0,e[3]=0$. If you are interested in the form with original entire G-functions, please contact the authors. 
The notation for external kinematic data is shown in [arxiv:2309.11249](https://arxiv.org/abs/2309.11249),  [arxiv:2302.00498](https://arxiv.org/abs/2302.00498).














