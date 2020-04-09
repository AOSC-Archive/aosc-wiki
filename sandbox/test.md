---
title: Katex Test
description: 
published: true
date: 2020-04-09T07:26:28.766Z
tags: 
---

# Expression Test

$$
P\left(\text{packets }=6\text{ in the first }3s\right)
&=P\left(\text{packets }\geq 7\text{ in the first }3s\right)-P\left(\text{packets }\geq 6\text{ in the first }3s\right)\\
&=R_{W_{2\times 7}}-R_{W_{2\times 6}}\\
&=R_{W_{14}}-R_{W_{12}}\\
&=e^{-\frac{3}{1/2}}\sum^{14-1}_{i=0}{\frac{6^i}{i!}}-e^{-\frac{3}{1/2}}\sum^{12-1}_{i=0}{\frac{6^i}{i!}}\\
&=e^{-6}\left(\frac{6^{12}}{12!}+\frac{6^{13}}{13!}\right)\\
&=\frac{166212}{25025e^6}\approx 0.016463471
$$
