20 years of attacks on ptmalloc
===
# Introduction
GNU libc (GLIBC):
## Basic interfaces
malloc  
calloc  
free  
## Design Methodology
General purpose  
Fast  
Space-conserving  
Protable  
Tunable  
## Allocation Strategy
`Best fit` `Reduce memory` `fragmentation` `Locality`
## Arenas and Bins
Arena  
Bins(not inuse chunk manage)
Fastbin - LIFO  
Normal Bin:  
unsorted bin  
small bin  
large bin  
## Chunk
Allocated chunk:  
PREV_INUSE | SIZE | N|M|P  
USER DATA  
           | SIZE | N|M|1  
           
Free chunk:  
PREV_INUSE | SIZE | N|M|P   
fd			 | bk			 	  
USER DATA  
           | SIZE | N|M|1  
           
## High-level view
## Bin - fastbin
(不合并)
## Bin - Normal bin
double linked list
unsorted bin  
malloc:
遍历unsorted bin  
有满足则切，不满足则放入small bin 或者 large bin  
## Bin - Large bin
Largebin has fd\_size/bk\_size  
gdb:finish

## atack 1999-2004
### Rise of the great unlink() 
`Smashing the stack for fun and profit`  
Solar Desigher "JPEG COM Marker Processig Vnlnerability"  
`The great unlink()` 2000-2001 rise  
Glibc(2005)  
unlink check the FD->bk != P && BK->fd != P  
another:frontline()  
`only exist in old dlmalloc`  
Other techniques  
"Advanced Doug lea's malloc exploits"  
`Exploiting the Wilderness` focus on the Top Chunk  
## 2005-2014 classic age
`The malloc maleficarum`  
The House of Prime  
The House of Mind  
The House of Force  
The House of Lore  
The House of Spirit  
`The House of Mind` and `The use of set_head to defeat the wilderness`  
_Malloc Des-Maleficarum_  
### The House of Prime
`fastbin index`  
fastbin\_index(8) (((unsighed int)(8))>>3)-2) = -1  
`free(p)
p->size == 8 
free(q)
q->size = big int`  
=>Fastbin attack && Unsorted bin attack  
fixed in GLIVC 2.4 (2006)  
check the length < 8 && make the maxlength global  
### The House of Mind
Exploit the non main arena feature  
Trick the arena_for_chunk() to get a fake heap_info  
malloc a lot to build a addr beginwith 0x081xxxxx which will be changed to 0x08100000  
and control 0x08100000  
Mitigated in Glibc 2.11
### The House of Force(useable)
exploit the top chunk  
Change the size of top chunk to a huge integer  
Malloc huge size chunk to make top pointing to bss/libc/stack  
Malloc a chunk to control the target memory  
No effective mitigations yet  
`print main_arena -> top`  
`x/32gx `  
malloc a large chunk to overwrite bss/got  
0xffffffffffffff
### The House of Lore
Malloc a normal bin chunk on arbitrary address  
Too many preconditions, not practical  
A little mitigation in GLIBC 2.11(2009)  
### The House of Spirit
Free a fake chunk on bss/libc/stack  
And malloc to get it back and control the target memory  
## Linked List attack
### unlink
Make unlink() great again`GLIBC 2.27`  
FD = &p - 0x18  
BK = &p - 0x10  
p = p - 0x18  
towelroot by geohot  
Various CTF challenges  
### fast bin attack
exploit the single linked list  
check:check the size of malloc  
stkof/HITCON CTF 2014  
### Unsorted bin attack
exploit the unsorted bin unlinking process  
The only unlinking code without any checks  
Overwriting target:  
global\_max\_fast  
\_IO\_list\_all FSOP  
Pointers  
Size  
## 2015 - now WARFARE
`The forgotten chunks`  
`Glibc Adventures:The Forgotten Chunks`
### Free Chunk Extending Attack
`Off-by-one scenario`  
Three:V A T  
Free object A  
Trigger off by one in V  
_allocate object B, overlapping formaer A and T_  
Modifying T(size big)
Exploit info leak to defeat ASLR(fd & bk)(size small)  
Examples:  
GHOST,CTF  
`Null byte off-by-one scenario`   
clear the prev_inuse bit of B  
set proper prev_size  
A T B , A B unlink  
Babyheap 0ctf  
`Null byte off-by-one scenario`  
without the control of prev-size  
Free object A  
Trigger null byte off-by-one in V  
Allocate object A1  
Allocate target object T  
Free object A1  
Free object B  
Heap Storm II 0ctf  
### Fastbin triple free attack
Free Chunk A  
Free Chunk B  
Free Chunk A  
(pass the head of the fastbin equal check)  
Malloc get A  
Corrupt FD of A  
Malloc A
Malloc B
Malloc get fake chunk  
### Fastbin misaligned "0x7f" trick
Pointers on x86_64 linux usually startswith 0x7f  
possible targets  
\_\_malloc\_hook  
\_\_realloc\_hoox  
GOT  
`Propagating value into main_arena`  
BabyHeap 0ctf 2018  
### Fastbin attack anywhere
Unsorted bin & Fastbin attack  
任意地址分配0x7f块
### Heap Storm  
State-of-the-art technique  
A resurrection if frontline() technique  
Heap Storm II 0ctf Quals  
## Future mitigations
Unlink can't use in glibc 2.28
### Thread Cache
very few integrity checks when malloc and free  
Tcache pointer poisoning  
Power overwelming version of fastbin attack
House of Spirit powered by tcache  
BabyHeap 18.04 0ctf final