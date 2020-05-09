# Bonjour!

Ce programme python recherche une racine d'une fonction en utilsant différentes méthodes : 
* dicho : la méthode de dichotomie
* zero : une méthode personnelle basée sur la méthode de la sécante
* brent : méthode de Brent
* newton : la méthode de Newton

La fonction f est définie par f(x) = x³ + x² - 4x - 5 et sa dérivée est f'(x) = 3x² + 2x - 4

```python runnable
def f(x):
    return x**3+x**2-4*x-5

def df(x):
    return 3*x**2+2*x-4

def dicho(f, a, b, eps=1e-6):
    # Recherche un zéro d'une fonction f sur un intervalle [a,b]. eps est la précision souhaitée (par défaut, 1/1000000)
    # f(a) et f(b) doivent être de signes contraires.
    if f(a)*f(b) > 0:
        return None
    while abs(b-a) > eps:    
        c = (a+b)/2
        if f(a)*f(c) > 0:
            a=c
        else:
            b=c
    return a, b  

def zero(f, a, b, alpha= 0.99,eps=1e-6, maxIter=1000):
    # Recherche un zéro d'une fonction f sur un intervalle [a,b]. 
    # eps est la précision souhaitée (par défaut, 1/1000000)
    # alterne la méthode de la sécante en partant de a et b pour resserer l'encadrement
    # f(a) et f(b) doivent être de signes contraires.
    ya = f(a)
    yb = f(b)
    if ya*yb > 0:
        return None
    dx = b-a
    i=0
    flag = True    
    while abs(dx) > eps:            
        dy = yb-ya
        if flag:
            #pied de la sécante AB à 99% depuis a
            c = a - alpha*ya*dx/dy   
        else:
            #pied de la sécante AB à 99% depuis b
            c = b - alpha*yb*dx/dy   
        yc = f(c)  
        if ya*yc > 0:
            a=c
            ya=yc
            flag=False            
        else:
            b=c
            yb=yc
            flag=True
        
        dx = b-a
        i += 1
        #print(i, a, b)
        if i > maxIter:
            return None
    return a, b, i  

def brent(f, a, b, eps=1e-6, maxIter=1000):	
    # Recherche un zéro d'une fonction f sur un intervalle [a,b] par la méthode de Brent. 
    # eps est la précision souhaitée (par défaut, 1/1000000)
    # maxIter est le nombre maximum d'itérations (par défaut, 1000)
    # f(a) et f(b) doivent être de signes contraires.
    fa=f(a)
    fb=f(b)
    if fa*fb >0:
        return None
    if fa < fb: # échanger a et b
        c=a
        a=b
        b=c
        fc=fa
        fa=fb
        fb=fc
    c=a
    fc=fa
    i=0
    while abs(b-a)>eps and fb!=0:
        if fa != fc and fb != fc and (b-a)<eps/2*b:
            # Interpolation Quadratique Inverse  
            s=((fb*fa)/((fc-fb)*(fc-fa))*c)+((fc*fa)/((fb-fc)*(fb-fa))*b)+((fc*fb)/((fa-fc)*(fa-fb))*a)
        else:
            # Sécante
            s=a-fa*(b-a)/(fb-fa)
        if (s-a)*(s-b)>0: # s sort de [a,b]
            # Bisection
            s=(a+b)/2
        fs=f(s)
        c=b
        if fs*fb<0:
            a=b
            fa=fb
        else:
            fa=fa/2
        b=s
        fb=fs
        i+=1
        if i > maxIter:
            return None
	 
    return a, b, i


def newton(f, df, a, eps=1e-6, maxIter=1000):
    # Recherche un zéro d'une fonction f par la méthode de Newton. 
    # df doit être la dérivée de f
    # a est une valeur de départ
    # eps est la précision souhaitée (par défaut, 1/100000)
    # maxIter est le nombre maximum d'itérations (par défaut, 1000)
    i=0
    delta=1
    while abs(delta)>eps:
        try:
            delta = f(a)/df(a)
        except:
            return None
        a = a - delta
        i += 1
        if i > maxIter:
            return None
    return a,i

print(dicho(f, 1, 5, eps=1e-9))
print(zero(f, 1, 5, eps=1e-9))
print(brent(f, 1, 5, eps=1e-9))
print(newton(f, df, 1, eps=1e-9))
```

# Exercice

Rechercher une racine de f(x) = x⁴- 5x + 2 comprise entre 0 et 1 avec une précision de 6 décimales