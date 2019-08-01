

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
def fortran(ex):
    ex1 = str(ex)
    return re.sub(r"P",r"I1",ex1)
```


```python
ts1= ts("P(l,m)")
fortran(simpler(ts1))
```




    '(I1(l - 1, m + 1) - I1(l + 1, m + 1))/(2*l + 1)'




```python
ts2 = ts(ts("P(l,m)"))
fortran(simpler(ts2))
```




    '-((2*l - 1)*(I1(l, m + 2) - I1(l + 2, m + 2)) + (2*l + 3)*(I1(l, m + 2) - I1(l - 2, m + 2)))/((2*l - 1)*(2*l + 1)*(2*l + 3))'




```python
t1 = t("P(l,m)")
fortran(simpler(t1))
```




    '((l + m)*I1(l - 1, m) + (l - m + 1)*I1(l + 1, m))/(2*l + 1)'




```python
t2 = t(t("P(l,m)"))
fortran(simpler(t2))
```





    '((l + m)*(2*l + 3)*((l - m)*I1(l, m) + (l + m - 1)*I1(l - 2, m)) + (2*l - 1)*((l - m + 2)*I1(l + 2, m) + (l + m + 1)*I1(l, m))*(l - m + 1))/((2*l - 1)*(2*l + 1)*(2*l + 3))'




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
