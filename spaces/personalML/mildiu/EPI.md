---
alias: [État Potentiel d'Infection, Estat Potencial d'Infecció, Estado Potencial de Infección]
---

El model d'__Estat Potencial d'Infecció (EPI)__ va ser introduït per Serge Strizyk a França el 1983.

## Data
### Input
Es recomana disposar d'una sèrie climàtica de 20 a 30 anys amb granularitat mensual.

S'ha de calcular, per a cada mes $m=1, \dots, 12$:
- La quantitat mitjana de pluja registrada en la sèrie històrica per a aquell mes, $P(m)$ (en mm). 
- Temperatura mitjana mensual en la sèrie històrica per a aquell mes, $T(m)$ (en C).
- Nombre mitjà de dies de pluja per mes, $N(m)$.
- Humitat relativa mitjana en la sèrie per a aquell mes, $H(m)$ (en %, només per als mesos d'estiu).

A més, també cal calcular:
- El nombre total de dies de pluja en la dècada anterior, $N_t$.

Finalment, cal recollir les dades dels darrers 12 mesos:
- La quantitat de pluja registrada en aquell mes $p(m)$ (en mm).
- La temperatura mitjana diària $t(m)$ (en C).
- Humitat relativa mitjana diürna, que és la mitjana de les humitats relatives registrades horàriament entre les 10:00h i les 18:00h, $h(m)$ (en %, només per als mesos d'estiu)


### Output
Un valor numèric $E$, relacionat amb el risc d'infecció.

## Càlcul

Tret de [[Sistema de apoyo a la predicción del hongo mildiu en la vid]].

### Fase hivernal

Sigui:
$$
c(m) = \left\{
\begin{array}{rl}
  2.4,&\quad m = 10, 11 \\
  2,&\quad m = 12 \\
  1.6,&\quad m = 1, 2, 3
\end{array}
\right.
$$

Aleshores, considerem la següent funció de $m$:
$$
E_p = c \cdot \left( \sqrt{p} - \sqrt{0.95 \cdot P} \right) + 0.2 \cdot \left( \sqrt{t p} - \sqrt{0.95 \cdot T P} \right) - \left( \sqrt{\frac{N}{12}} \log\frac{p}{N_t} \right)
$$

### Fase estival

Siguin:
- $U_h$ l'humitat relativa de la progressió històrica per a cada mes (en %).
- $\theta$ la temperatura mitjana de la progressió històrica per a cada mes (en C).
- $U_d$ l'humitat relativa diürna entre les 10:00h i les 18:00h (en %).
- $T$ la temperatura mitjana diària (en C).

Aleshores,
$$
E_c = 0.00012 \cdot \left( \left( \frac{5}{8}H + \frac{3}{8}h \right)^2 \sqrt{t} - h^2 \sqrt{T} \right)
$$

### Càlcul

Amb l'agregació de les fases anteriors:
$$
E = \sum_{m = 10}^3 E_c(m) + \sum_{m = 4}^9 E_p(m)
$$


### Referències
- [[Modelización del Mildiu de la vid]]
- [[Evaluation du risque de développement du mildiou sur le vignoble bordelais par interprétation du modèle Potentiel Système]]
- [[Sistema de apoyo a la predicción del hongo mildiu en la vid]]