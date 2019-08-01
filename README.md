
```python
import sympy as sym
import re 
from sympy.parsing.latex import parse_latex 
```


```python
def t(fk):
    return re.sub(r"P\((.*?),(.*?)\)",r"(1/(2(\1)+1))((\1-\2+1)P(\1+1,\2) + (\1+\2)P(\1-1,\2))",fk)
```


```python
def ts(fk):
    return re.sub(r"P\((.*?),(.*?)\)",r"(1/(2(\1)+1))(P(\1-1,\2+1)-P(\1+1,\2+1))",fk)
```


```python
def simpler(eks):
    l,m = sym.symbols("l m")
    return sym.simplify((sym.simplify(parse_latex(eks))))
```


```python
ts1= ts("P(l,m)")
(simpler(ts1))
```




$\displaystyle \frac{P{\left(l - 1,m + 1 \right)} - P{\left(l + 1,m + 1 \right)}}{2 l + 1}$




```python
ts2 = ts(ts("P(l,m)"))
(simpler(ts2))
```




$\displaystyle - \frac{\left(2 l - 1\right) \left(P{\left(l,m + 2 \right)} - P{\left(l + 2,m + 2 \right)}\right) + \left(2 l + 3\right) \left(P{\left(l,m + 2 \right)} - P{\left(l - 2,m + 2 \right)}\right)}{\left(2 l - 1\right) \left(2 l + 1\right) \left(2 l + 3\right)}$




```python
t1 = t("P(l,m)")
(simpler(t1))
```




$\displaystyle \frac{\left(l + m\right) P{\left(l - 1,m \right)} + \left(l - m + 1\right) P{\left(l + 1,m \right)}}{2 l + 1}$




```python
t2 = t(t("P(l,m)"))
(simpler(t2))
```




$\displaystyle \frac{\left(l + m\right) \left(2 l + 3\right) \left(\left(l - m\right) P{\left(l,m \right)} + \left(l + m - 1\right) P{\left(l - 2,m \right)}\right) + \left(2 l - 1\right) \left(\left(l - m + 2\right) P{\left(l + 2,m \right)} + \left(l + m + 1\right) P{\left(l,m \right)}\right) \left(l - m + 1\right)}{\left(2 l - 1\right) \left(2 l + 1\right) \left(2 l + 3\right)}$




```python
ts1t1 = t(ts("P(l,m)"))
fortran(simpler(ts1t1))
```




    '(-(2*l - 1)*((l - m + 3)*I1(l + 2, m + 1) + (l + m + 2)*I1(l, m + 1)) + (2*l + 3)*((l + m)*I1(l - 2, m + 1) + (l - m + 1)*I1(l, m + 1)))/((2*l - 1)*(2*l + 1)*(2*l + 3))'




```python
ts2t1 = t(ts(ts("P(l,m)")))
fortran(simpler(ts2t1))
```




    '((2*l - 3)*(2*l - 1)*((2*l + 1)*((l - m + 5)*I1(l + 3, m + 2) + (l + m + 4)*I1(l + 1, m + 2)) - (2*l + 5)*((l - m + 3)*I1(l + 1, m + 2) + (l + m + 2)*I1(l - 1, m + 2))) - (2*l + 3)*(2*l + 5)*((2*l - 3)*((l - m + 3)*I1(l + 1, m + 2) + (l + m + 2)*I1(l - 1, m + 2)) - (2*l + 1)*((l + m)*I1(l - 3, m + 2) + (l - m + 1)*I1(l - 1, m + 2))))/((2*l - 3)*(2*l - 1)*(2*l + 1)**2*(2*l + 3)*(2*l + 5))'




```python
ts2t2 = t(t(ts(ts("P(l,m)"))))
fortran(simpler(ts2t2))
```




    '((2*l - 5)*(2*l - 3)*(2*l - 1)*((2*l - 1)*(2*l + 1)*((2*l + 3)*((l - m + 6)*I1(l + 4, m + 2) + (l + m + 5)*I1(l + 2, m + 2))*(l - m + 5) + (2*l + 7)*((l - m + 4)*I1(l + 2, m + 2) + (l + m + 3)*I1(l, m + 2))*(l + m + 4)) - (2*l + 5)*(2*l + 7)*((2*l - 1)*((l - m + 4)*I1(l + 2, m + 2) + (l + m + 3)*I1(l, m + 2))*(l - m + 3) + (2*l + 3)*((l - m + 2)*I1(l, m + 2) + (l + m + 1)*I1(l - 2, m + 2))*(l + m + 2))) - (2*l + 3)*(2*l + 5)*(2*l + 7)*((2*l - 5)*(2*l - 3)*((2*l - 1)*((l - m + 4)*I1(l + 2, m + 2) + (l + m + 3)*I1(l, m + 2))*(l - m + 3) + (2*l + 3)*((l - m + 2)*I1(l, m + 2) + (l + m + 1)*I1(l - 2, m + 2))*(l + m + 2)) - (2*l + 1)*(2*l + 3)*((l + m)*(2*l - 1)*((l - m)*I1(l - 2, m + 2) + (l + m - 1)*I1(l - 4, m + 2)) + (2*l - 5)*((l - m + 2)*I1(l, m + 2) + (l + m + 1)*I1(l - 2, m + 2))*(l - m + 1))))/((2*l - 5)*(2*l - 3)*(2*l - 1)**2*(2*l + 1)**2*(2*l + 3)**2*(2*l + 5)*(2*l + 7))'




```python
def fortran(ex):
    ex1 = str(ex)
    return re.sub(r"P",r"I1",ex1)
```


```python
(simpler(ts2t2))
```




$\displaystyle \frac{\left(l + m\right) \left(2 l + 3\right) \left(2 l + 5\right) \left(2 l + 7\right) \left(- \left(l - m\right) \left(2 l - 5\right) \left(2 l - 3\right) \left(\left(2 l - 1\right) \left(P{\left(l,m + 2 \right)} - P{\left(l + 2,m + 2 \right)}\right) + \left(2 l + 3\right) \left(P{\left(l,m + 2 \right)} - P{\left(l - 2,m + 2 \right)}\right)\right) + \left(2 l + 1\right) \left(2 l + 3\right) \left(\left(2 l - 5\right) \left(P{\left(l,m + 2 \right)} - P{\left(l - 2,m + 2 \right)}\right) + \left(2 l - 1\right) \left(P{\left(l - 4,m + 2 \right)} - P{\left(l - 2,m + 2 \right)}\right)\right) \left(l + m - 1\right)\right) - \left(2 l - 5\right) \left(2 l - 3\right) \left(2 l - 1\right) \left(\left(2 l - 1\right) \left(2 l + 1\right) \left(\left(2 l + 3\right) \left(P{\left(l + 2,m + 2 \right)} - P{\left(l + 4,m + 2 \right)}\right) - \left(2 l + 7\right) \left(P{\left(l,m + 2 \right)} - P{\left(l + 2,m + 2 \right)}\right)\right) \left(l - m + 2\right) + \left(2 l + 5\right) \left(2 l + 7\right) \left(\left(2 l - 1\right) \left(P{\left(l,m + 2 \right)} - P{\left(l + 2,m + 2 \right)}\right) + \left(2 l + 3\right) \left(P{\left(l,m + 2 \right)} - P{\left(l - 2,m + 2 \right)}\right)\right) \left(l + m + 1\right)\right) \left(l - m + 1\right)}{\left(2 l - 5\right) \left(2 l - 3\right) \left(2 l - 1\right)^{2} \left(2 l + 1\right)^{2} \left(2 l + 3\right)^{2} \left(2 l + 5\right) \left(2 l + 7\right)}$




```python
parse_latex(t(t("P(l,m)")))
```




$\displaystyle \frac{\frac{\left(m + \left(l - 1\right)\right) P{\left(\left(l - 1\right) - 1,m \right)} + \left(\left(- m + \left(l - 1\right)\right) + 1\right) P{\left(\left(l - 1\right) + 1,m \right)}}{2 \left(l - 1\right) + 1} \left(l + m\right) + \frac{\left(m + \left(l + 1\right)\right) P{\left(\left(l + 1\right) - 1,m \right)} + \left(\left(- m + \left(l + 1\right)\right) + 1\right) P{\left(\left(l + 1\right) + 1,m \right)}}{2 \left(l + 1\right) + 1} \left(\left(l - m\right) + 1\right)}{2 l + 1}$




```python
ts1t2 = ts(t(t("P(l,m)")))
fortran(simpler(ts1t2))
```




    '((l + m)*(2*l + 3)*(2*l + 5)*((l - m)*(2*l - 3)*(I1(l - 1, m + 1) - I1(l + 1, m + 1)) + (2*l + 1)*(I1(l - 3, m + 1) - I1(l - 1, m + 1))*(l + m - 1)) + (2*l - 3)*(2*l - 1)*((2*l + 1)*(I1(l + 1, m + 1) - I1(l + 3, m + 1))*(l - m + 2) + (2*l + 5)*(I1(l - 1, m + 1) - I1(l + 1, m + 1))*(l + m + 1))*(l - m + 1))/((2*l - 3)*(2*l - 1)*(2*l + 1)**2*(2*l + 3)*(2*l + 5))'




```python
ts1t1 = ts(t("P(l,m)"))
fortran(simpler(ts1t1))
```




    '(-(l + m)*(2*l + 3)*(I1(l, m + 1) - I1(l - 2, m + 1)) + (2*l - 1)*(I1(l, m + 1) - I1(l + 2, m + 1))*(l - m + 1))/((2*l - 1)*(2*l + 1)*(2*l + 3))'




```python

```