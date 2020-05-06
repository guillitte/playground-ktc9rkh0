# Bonjour!

Ce programme python recherche une racine d'une fonction en utilsant deux méthodes : par dichotomie et par la méthode de Newton

```python runnable
def f(x):
    return x**3+x**2-4*x-5

def df(x):
    return 3*x**2+2*x-4

def zeros(f, a, b, eps=1e-3):
    # Recherche un zéro d'une fonction f sur un intervalle [a,b]. eps est la précision souhaitée (par défaut, 1/1000)
    # f'a) et f(b) doivent être de signes contraires.
    if f(a)*f(b) > 0:
        return None
    while abs(b-a) > eps:    
        c = (a+b)/2
        if f(a)*f(c) > 0:
            a=c
        else:
            b=c
    return (a, b)  


def newton(f, df, a, eps=1e-3, maxIter=1000):
    # Recherche un zéro d'une fonction f par la méthode de Newton. 
    # df doit être la dérivée de f
    # eps est la précision souhaitée (par défaut, 1/1000)
    # maxIter est le nombre maximum d'itération (par défaut, 1000)
    i=0
    while abs(f(a))>eps:
        a = a-f(a)/df(a)
        if i > maxIter:
            return None
    return a


print(zeros(f, 1, 5, eps=1e-9))
print(newton(f, df, 1, eps=1e-9))
```

