# 🧠 PicoCTF Writeup --- Franklin-Reiter Related Message Attack

## 📌 Challenge Description

We are given an RSA setup where two ciphertexts are generated:

ciphertext = pow(Message, e, N) ciphertext2 = pow(Message_fixed, e, N)

Additionally, we are told:

Message - Message_fixed = -3

And the hint:

Franklin Reiter \_\_\_\_\_\_ \_\_\_\_\_\_ attack

------------------------------------------------------------------------

## 🔍 Observations

-   Same modulus `N`
-   Same exponent `e = 17`
-   Two messages are **linearly related**

From: Message - Message_fixed = -3

We get: Message_fixed = Message + 3

So: - C1 = M\^e mod N - C2 = (M + 3)\^e mod N

------------------------------------------------------------------------

## 💥 Vulnerability

This is a classic case of:

Franklin-Reiter Related Message Attack

RSA becomes insecure when: - Same modulus is reused - Messages are
related linearly

------------------------------------------------------------------------

## ⚙️ Attack Idea

We construct two polynomials:

-   f(x) = x\^e - C1
-   g(x) = (x+3)\^e - C2

Then compute:

gcd(f(x), g(x)) mod N

This reveals:

x - M

Which allows us to recover the original message M.

------------------------------------------------------------------------

## 🛠️ Exploit Code (Pure Python)

```python
from Crypto.Util.number import long_to_bytes

N = 17334845546772507565250479697360218105827285681719530148909779921509619103084219698006014339278818598859177686131922807448182102049966121282308256054696565796008642900453901629937223685292142986689576464581496406676552201407729209985216274086331582917892470955265888718120511814944341755263650688063926284195007148056359887333784052944201212155189546062807573959105963160320187551755272391293705288576724811668369745107148481856135696249862795476376097454818009481550162364943945249601744881676746859305855091288055082626399929893610275614840617858985993338556889612804266896309310999363054134373435198031731045253881

C1 = 3486364849772584627692611749053367200656673358261596068549224442954489368512244047032432842601611650021333218776410522726164792063436874469202000304563253268152374424792827960027328885841727753251809392141585739745846369791063025294100126955644910200403110681150821499366083662061254649865214441429600114378725559898580136692467180690994656443588872905046189428367989340123522629103558929469463071363053880181844717260809141934586548192492448820075030490705363082025344843861901475648208157572346004443100461870519699021342998731173352225724445397168276113254405106732294978648428026500248591322675321980719576323749

C2 = 201982790559548563915678784397933493721879152787419243871599124287434576744055997870874349538398878336345269929647585648144070475012256331468688792105087899416655051702630953882466457932737483198442642588375981620937494661378586614008496182135571457352400128892078765628319466855732569272509655562943410536265866312968101366413636251672211633011159836642751480632253423529271185888171036917413867011031963618529122680143291205470937752671602494831117301480813590683791618751348224964277861127486155552153012612562009905595646626759034581358425916638671884927506025703373056113307665093346439014722219878575598308124

e = 17

# Polynomial helpers
def poly_add(a, b):
    res = [0]*max(len(a), len(b))
    for i in range(len(a)): res[i] += a[i]
    for i in range(len(b)): res[i] += b[i]
    return [x % N for x in res]

def poly_sub(a, b):
    res = [0]*max(len(a), len(b))
    for i in range(len(a)): res[i] += a[i]
    for i in range(len(b)): res[i] -= b[i]
    return [x % N for x in res]

def poly_mul(a, b):
    res = [0]*(len(a)+len(b)-1)
    for i in range(len(a)):
        for j in range(len(b)):
            res[i+j] += a[i]*b[j]
    return [x % N for x in res]

def poly_mod(a, b):
    while len(a) >= len(b):
        coeff = a[-1] * pow(b[-1], -1, N) % N
        shift = len(a) - len(b)
        for i in range(len(b)):
            a[i+shift] -= coeff * b[i]
        a = [x % N for x in a]
        while a and a[-1] == 0:
            a.pop()
    return a

def poly_gcd(a, b):
    while b:
        a, b = b, poly_mod(a, b)
    return a

# f(x) = x^e - C1
f = [(-C1) % N] + [0]*(e-1) + [1]

# g(x) = (x+3)^e - C2
g = [1]
for _ in range(e):
    g = poly_mul(g, [3,1])
g[0] = (g[0] - C2) % N

res = poly_gcd(f, g)

# Extract root
a = res[1]
b = res[0]
M = (-b * pow(a, -1, N)) % N

print(long_to_bytes(M))
```
------------------------------------------------------------------------

## 🚩 Final Flag

picoCTF{m3ssage_w1th_typ0}

------------------------------------------------------------------------

## 🧨 Key Takeaways

-   RSA is not secure if same modulus is reused
-   Related messages break RSA
-   Franklin-Reiter attack recovers plaintext

------------------------------------------------------------------------

## 🏁 Conclusion

Improper use of RSA leads to full plaintext recovery using algebraic
attacks.
