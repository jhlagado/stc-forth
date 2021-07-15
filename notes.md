# DOES>

## in firth

2560: 25 54 20
2563: EE 14 ; link to 14EE
2565: 05 63 6F 6E 73 74 ; flags + len "const"
256B: CF ; rst 08 ; enter forth
256C: 04 05 ; CREATE
256E: 44 10 ; COMMA  
2570: E0 05 ; XDOES
2572: CD D0 05 ; call DODOES
2575: 78 0F ; FETCH @ ie. body of "const"
2577: 45 0F ; EXIT
2579: 63 25 ; link to 2563
257A: 01 79 ; flags + len "y"
257C: CD 72 25 ; call 2572 (the body of "const" word)
2580: 64 00 ; body of y = 100 ($64)

xdoes: ; --
ENTER ; enter forth
dw rfrom ; do not return to caller, terminate calling word
dw latest, AT, tcfa ; get body of "y"
dw LIT, call_opcode
dw over, cstore ; compile call to body of "const"
dw ONEP, STORE
EXIT ; exit forth

dodoes: ; -- a-addr
pop HL ; body of "const" 2575
PDUP
pop BC ; TOS = address of body of "y" (ie. rfrom)
push HL ; something for NEXT to pop?  
NEXT ; enters forth with HL = 2575

## in stc

22C0: BC 0B ; backlink 0BBC ; word: const
22C2: 80 ; flags
22C3: 05 63 6F 6E 73 74 ; "const"
22C9: CD 83 00 ; call create
22CC: CD 7D 07 ; call comma
22CF: CD 65 01 ; call xdoes
22D2: CD 83 0A ; call rfrom
22D5: CD 44 07 ; call at
22D8: C9 ; ret
22D9: C0 22 ; backlink 22C0 ; word: y
22DB: 00 ; flags
22DC: 01 79 ; "y"
22DE: C3 C4 00 ; default: create1
22E1: 00 01 ; data $100 (because of base)
