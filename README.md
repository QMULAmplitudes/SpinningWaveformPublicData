# SpinningWaveformPublicData
This repository present the gravitational waveform data at tree level for arbitrary spin without expansion the spin parameter.

## Waveform data with non-perturbative spin
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


## Waveform data with Taylor expansion on the spin parameter "a"
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

## waveform integrand after rescaling











