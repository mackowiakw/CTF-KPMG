# Cyber H@ckademy 2023 Write-up

Firma KPMG przygotowała mini CTFa w ramach rekrutacji do [Cyber H@ckademy 2023](https://kpmg.com/pl/pl/home/careers/dla-studentow-i-absolwentow/programy-dla-studentow.html#3).
Choć osobiście nie wziąłem w niej udziału, to zadania rozwiązałem z ciekawości i dla zabawy.

W ramach CTFa przygotowano dwie ścieżki:

- Ścieżka bezpieczeństwa aplikacji
- Ścieżka bezpieczeństwa infrastruktury

Zadania polegały na znalezieniu ukrytych flag w formacie **KPMG{flaga}**.
Liczba flag była jawnie określona, a do ich znalezienia nie było konieczne wykorzystanie metod siłowych (z ang. _brute-force_).

| Zadanie              | Opis                                             | Liczba flag | Flaga                                                                     |
| -------------------- | :----------------------------------------------- | :---------: | ------------------------------------------------------------------------- |
| [Crypto](./Crytpo)   | Błąd w implementacji protokołu Diffiego-Hellmana |      1      | KPMG{zn4j0m0sc_skl4dn1_jest_0p}                                           |
| [MISC](./MISC)       | Zakodowany ciąg ukryty w pliku                   |      1      | KPMG{4rt3f4ct_0f_thr33_d0ts}                                              |
| [Mobilka](./Mobilka) | Analiza wsteczna aplikacji mobilnej              |      3      | KPMG{Enc0ded_res0urc3} <br> KPMG{Do_y0u_3v3n_SMALI_bro?} <br> KPMG{flaga} |
| [Network](./Network) | Zrzut ruchu sieciowego                           |      1      | KPMG{n3tw0rk_fu_m4st3r}                                                   |
| [OSINT](./OSINT)     | Zakodowane wpisy na Twitterze i metadane w pliku |      2      | KPMG{D0n7_l3@k_0Ur_d474} <br> KPMG{Metad@ta_eXpert}                       |
| [Web](./Web)         | Plik js w źródłach strony                        |      1      | KPMG{JS_1s_fUn_116_r1gHt?}                                                |

W każdym z podfolderów repozytorium znajduje się opis rozwiązania i kopia zadania.
