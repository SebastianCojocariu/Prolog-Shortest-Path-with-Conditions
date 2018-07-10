        				                TEMA 2
				                         -PP-

Ideea centrala pentru rezolvarea problemei consta in a efectua un bfs.
Vom construi bfs-ul dupa urmatoarea observatie:pornim initial cu o coada(lista)
in care avem introdusa perechea [[Source],F1],unde F1 este formula ce rezulta
dupa aplicarea formulei initiale F pe nodul Source.La fiecare pas,scoatem
o pereche de tipul acesta de la inceputul cozii:sa zicem ca am scos perechea
[[a(j),...,a(1)],F_j],insemnanad ca am parcurs deja a(1),a(2),...,a(j) si am 
actualizat la fiecare pas Formula F_j.Alegem acum toate nodurile diferite de 
a(j-1),...,a(1) care au muchie cu a(j),fie ele V={v(1),...,v(k)} si generam acum
lista [ [[v1,a(j),...,a(1)],F_j1] , ... ,[[v(k),a(j),...a(1)],F_jk],unde F_ji
este formula obtinuta aplicand formula initiala F_j pe nodul v(i).Modul cum e 
implementat predicatul propaga_regula_vecini ne ofera garantia ca,in cazul in care
o formula F_ji = false,perechea corespunzatoare nu va mai fi adaugata in lista,
deci mereu se vor pastra drumuri pentru care inca exista sansa ca ele sa ajunga
respectand regula la destinatie.Astfel,dupa generarea noilor Path-uri de lungime 
len(Path_initial)+1,acestea vor fi adaugate la sfarsitul cozii,iar Path_initial
va fi scos,simuland chiar ideea de bfs.


In acest sens,functia getPath va verifica,inainte de a apela bfs,daca formula
nu genereaza direct false pentru nodul sursa(de ex and(rosu,negru)
pentru o sursa de culoare albastra ar duce direct la terminarea programului,fara a mai apela
bfs).Daca nu se genereaza false,inaintam cu predicatul bfs.Daca obtinem un Path[Lista,Formula]
diferit de [] trebuie sa inversam Lista si sa o returnam,altfel returnam false.

Optimizari:
	Din modul in care folosim bfs,nu vom genera nicioadata un drum de lungime n+1 daca
va exista un drum valid de lungime n (a se vedea explicatiile din cadrul codului de la bfs)
	Mai mult,la fiecare pas,daca un Path va avea o formula care nu mai poate fi satisfacuta
(anume e false),el nu va fi introdus in Lista generata de perechea Path=[L,F] prin 
predicatul adauga_vecini_la_pathuri.Astfel se va reduce Coada cu un numar drastic de
cazuri si programul se va termina mai rapid(De ex,daca Path = [[2,1],False],nu ar fi scos
toate drumurile de la 1 spre 2 spre destinatie vor fi generate,lucru exponential in raport 
cu nr de noduri ale grafului).
	Prin modul in care e definit predicatul schimba_formula,la fiecare pas,daca o conditie
poate fi redusa,ea va fi redusa (de exemplu,daca aplic pe nodul 5 de culoare albastru
F=and(future(negru),albastru) o sa obtin formula F_new = future(negru),iar pe formula
F=or(negru,future(rosu)) vom obtine F_new = future(rosu),deoarece negru /= albastru)
