# Crypto

_Przechwyciliśmy rozmowę pomiędzy dwoma uczestnikami H@ckademy. Czy umiesz odczytać ich sekret?_

---

Dane są 2 pliki:

1. **chat.txt**

```
Alicja: Hej, udało mi się zdobyć kolejną flagę z H@ckademy
Bob: O, podeślij plis
Alicja: Spoko, tylko na wszelki wypadek ją zaszyfruję, kto wie kto może podsłuchiwać.
Bob: Ok
Alicja: Użyjmy naszego niezawodnego skryptu do wymiany sekretów. No ten Diffie Helman.
Bob: Ah, nasz pierwszy projekt w Pytongu.
Alicja: Dobra wysyłam dane:
	g=2
	p=10332921861938291919377635159012636040519117927041835671194203494937679183911345052843111512544303969800681115505917911462916407940308340306260755239268943
	A=8370337962458643162004582468469045984889816058567658904788530882468973454873284491037710219222503893094363658486261941098330951794393018216763327572120126
Bob: B=9755909033513767641159594933585734179714892615169429957597029280980531443144704341694474385957669949989090202320232433789032328934018623049865998847328154
Alicja: Łap zaszyfrowaną flagę.
MN41dUQl7I8/BtOmwVAiJrj8FutJ7Y+PhDVavJ7NTeWbQCOEbPg3lDhByds2DHvR
Bob: Dzięki wielkie. Dzięki naszej bezbłędnej implementacji Diffie Helmana nie ma szans żeby ktoś ją rozszyfrował hehe
```

2. **DH shared secret generation.py**

```python
from hashlib import sha256
from base64 import b64decode
from base64 import b64encode

from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad


def generate_shared_secret_DH():
    print("Get DH parameters")
    g = int(input('g= '))
    p = int(input('p= '))
    your_private_key = int(input('Your private key= '))
    print('Your public key A is :',generate_public_int(g,your_private_key,p))
    other_public_key = int(input("Other public key= "))
    return generate_shared_secret(other_public_key,your_private_key,p)

def generate_public_int(g, a, p):
    return g ^ a % p


def generate_shared_secret(A, b, p):
    return A ^ b % p

def encrypt(secret, data):
    secret = sha256(secret.encode('utf8')).digest()
    iv = get_random_bytes(AES.block_size)
    cipher = AES.new(secret, AES.MODE_CBC, iv)
    return b64encode(iv + cipher.encrypt(pad(data.encode('utf-8'),AES.block_size)))

def decrypt(secret, data):
    secret = sha256(secret.encode('utf8')).digest()
    raw = b64decode(data)
    cipher = AES.new(secret, AES.MODE_CBC, raw[:AES.block_size])
    return unpad(cipher.decrypt(raw[AES.block_size:]), AES.block_size)


if __name__ == '__main__':

    shared_secret = str(generate_shared_secret_DH())


    while True:
        print ("""
        1.Encrypt
        2.Decrypt
        3.Exit
        """)
        ans=input("Option=")
        if ans=="1":
            print('ENCRYPTION')
            msg = input('Text to encrypt')
            print('Ciphertext:', encrypt(shared_secret,msg).decode('utf-8'))
        elif ans=="2":
            print('\nDECRYPTION')
            cte = input('Ciphertext: ')
            print('Decryped text:', decrypt(shared_secret,cte).decode('utf-8'))
        elif ans=="3":
            exit()
```

## Rozwiązanie

Protokół (vel algorytm) Diffiego-Hellmana. Od razu wiedziałem, że służy do ustalania tajnych kluczy mając do dyspozycji jedynie nieszyfrowane kanały.
Nawet miałem kiedyś na studiach okazję go zaimplementować.
Stąd też przebłyski w pamięci, że wykorzystuje liczby pierwsze, potęgowanie i działanie modulo.
Ale do brzegu.

Pierwsza myśl: słaba wartość inicjalizująca `g=2`.
Rzut oka na fragment kodu, gdzie jest ona używana i... bingo.
Niejako przy okazji, ale znaleźliśmy błędną linię kodu.

```python
def generate_public_int(g, a, p):
    return g ^ a % p
```

Znak `^` w Pythonie nie oznacza potęgowania tylko operację XOR.
Klucz publiczny Alicji to `A = g^a % p`, gdzie znamy `g` i `p`, nie znamy tylko `a`.
I o ile w przypadku potęgowania odwrócenie takiego działania byłoby bardzo trudne, to w przypadku operacji XOR jest trywialne.

Jako że XOR jest przemienny, to `g^a` jest równe `a^g`.
XORowanie `a` wykonujemy z liczbą 2 (binarnie 10), więc `a` może się różnić od `A` jedynie na dwóch ostatnich bitach (na razie pomijając modulo p), stąd `a = A^2`.
Dwa ostatnie bity klucza to publicznego Alicji `A` to 10, zXORowane z 2 (binarnie 10), dają binarnie 00, czyli o 2 mniej niż klucz publiczny.
Zatem, pomijając modulo `p`, klucz prywatny Alicji `a` to `A-2`. Uwzględniając modulo `p`, mamy `a = (A-2 + K*p) % p`, gdzie `K` jest całkowite.
W tym przypadku jednak ostatecznie okazało się, że `K=0`.

Teraz wystarczy już tylko uruchomić program podając odpowiednie wartości inicjalizujące `g` oraz `p`, klucz prywatny Alicji `a`, klucz publiczny Boba `B` i odszyfrować wiadomość.

Flaga to `KPMG{zn4j0m0sc_skl4dn1_jest_0p}`
