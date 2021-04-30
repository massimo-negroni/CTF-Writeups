Pretty Ridiculous
=================
50 points
---------

The chall gave as ciphertext an array of integers and the values (n, e) = (627585038806247, 65537).
This is clearly a public key of RSA and it is noted that the module n is particularly short, 
therefore it can most likely be factored in a short time:
```python
#!/usr/bin/python3
import math

def prime_factors(n):
    i = 2
    factors = []
    while i * i <= n:
        if n % i:
            i += 1
        else:
            n //= i
            factors.append(i)
    if n > 1:
        factors.append(n)
    return factors

if __name__ == "__main__":
    n = 627585038806247
    e = 65537
    p, q = prime_factors(n)
    print(p, q)
```
So with the factors p and q it is possible to calculate phi and therefore d:
```python
#!/usr/bin/python3
import math

def egcd(a, b):
    if a == 0:
        return (b, 0, 1)
    else:
        g, y, x = egcd(b % a, a)
        return (g, x - (b // a) * y, y)

def modinv(a, m):
    g, x, y = egcd(a, m)
    if g != 1:
        raise Exception('modular inverse does not exist')
    else:
        return x % m

if __name__ == "__main__":
    p = 13458281
    q = 46631887
    ct = 145213650433152

    e = 65537
    
    phi = (p - 1) * (q - 1)

    d = modinv(e, phi)

    print(d)
```
Finally, only a simple equation remains to be calculated:
m = c ** d% n

```python
#!/usr/bin/python3
import math

def rsadecrypt(x, d, n):
    ch = pow(x, d, n)
    return ch

if __name__ == "__main__":
    d = 119987789848673
    n = 627585038806247
    ct = [145213650433152, 4562349440334, 24272724667960, 598242834066721, 89584939111364, 426756492371444, 511701778613016, 551732685650248, 296367799892003, 63113462897284, 198510931603899, 321201931522255, 401044612595398, 542697603423052, 213898535689643, 275839755798105, 185841409622217, 551732685650248, 121188708737752, 401044612595398, 512808963720303, 275839755798105, 198510931603899, 275839755798105, 401044612595398, 174484844253615, 551732685650248, 174486913717420, 575163265381617, 213898535689643, 401044612595398, 49103824223436, 551732685650248, 401044612595398, 598242834066721, 202722428784490, 306606077829794, 53801100921263, 401044612595398, 184805755675232, 405971446461049, 296367799892003, 275839755798105, 275839755798105, 401044612595398, 358054299396778, 4562349440334, 320837325468842, 401044612595398, 202722428784490, 551732685650248, 321201931522255, 228350651363859]
    for c in ct:
        pc = rsadecrypt(c, d, n)
        print(chr(pc), end='')
    print('\n')
```
The full script:
```python
#!/usr/bin/python3
import math

#function to find prime factors
def prime_factors(n):
    i = 2
    factors = []
    while i * i <= n:
        if n % i:
            i += 1
        else:
            n //= i
            factors.append(i)
    if n > 1:
        factors.append(n)
    return factors

def egcd(a, b):
    if a == 0:
        return (b, 0, 1)
    else:
        g, y, x = egcd(b % a, a)
        return (g, x - (b // a) * y, y)

#function to calculate inverse module
def modinv(a, m):
    g, x, y = egcd(a, m)
    if g != 1:
        raise Exception('modular inverse does not exist')
    else:
        return x % m

#function to calculate plaintext
def rsadecrypt(x, d, n):
    ch = pow(x, d, n)
    return ch

if __name__ == "__main__":
    n = 627585038806247
    e = 65537
    ct = [145213650433152, 4562349440334, 24272724667960, 598242834066721, 89584939111364, 426756492371444, 511701778613016, 551732685650248, 296367799892003, 63113462897284, 198510931603899, 321201931522255, 401044612595398, 542697603423052, 213898535689643, 275839755798105, 185841409622217, 551732685650248, 121188708737752, 401044612595398, 512808963720303, 275839755798105, 198510931603899, 275839755798105, 401044612595398, 174484844253615, 551732685650248, 174486913717420, 575163265381617, 213898535689643, 401044612595398, 49103824223436, 551732685650248, 401044612595398, 598242834066721, 202722428784490, 306606077829794, 53801100921263, 401044612595398, 184805755675232, 405971446461049, 296367799892003, 275839755798105, 275839755798105, 401044612595398, 358054299396778, 4562349440334, 320837325468842, 401044612595398, 202722428784490, 551732685650248, 321201931522255, 228350651363859]
    p, q = prime_factors(n)

    phi = (p - 1) * (q - 1)
    d = modinv(e, phi)
  
    for c in ct:
        pc = rsadecrypt(c, d, n)
        print(chr(pc), end='')
    print('')
```
