# Network

_Czy potrafisz znaleźć co było przesłane w tym ruchu sieciowym?_

---

Dany jest plik **Network.zip**, zawierający plik **Network.pcap**.

![](./imgs/1.png)

W plikach **.pcap** zapisuje się pakiety sieciowe.
Można je otworzyć np. za pomocą programu [Wireshark](https://www.wireshark.org/).

![](./imgs/2.png)

Jak widać powyżej, zarejestrowano ruch sieciowy, w którym ustanawiane jest połączenie TCP, a następnie, za pomocą protokołu HTTP, pobierany jest plik **Flag.pdf**.

Eksportujemy dane z pakietu i zapisujemy jako **Flag.pdf**:

![](./imgs/3.png)

Po otwarciu zapisanego pliku uzyskaliśmy flagę:

![](./imgs/4.png)

Flaga to `KPMG{n3tw0rk_fu_m4st3r}`
