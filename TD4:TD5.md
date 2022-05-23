# **Algorithmique et programmation avancée**
# Série de TD N°4 /TD N°5
<br>


##  Objectif : Les Piles/ Les Files
<br>

**TD 4**

**Exercice 1**


> **Définir la structure PILE qui implémente une pile de caractères à l'aide d'une liste chainée.**
> <br>

```java script

    typedef struct maillon{
       int valeur;
       struct maillon * suivant;
    } Maillon;
    
    
/* type PILE */
   typedef Maillon* PILE;

```
<br>

> **Donner le contenu de la pile P à chaque étape du programme suivant :**
> main(){
Pile P;
char C;
      P=creerPile();
      empiler(&P,'A');
      depiler(&P); 
      empiler(&P,'B'); 
      sommet(P,&C); 
      empiler(&P,'a'); 
      empiler(&P,C);
}
> <br>

 * La pile aura comme resultat: P->B->A->B
<br>

>  **On suppose que les fonctions : initPile, pileVide, empiler, sommet et depiler sont définies, Ecrire une fonction pileCopy qui reçoit une pile P de caractères comme paramètre et retourne une copie de P. Noter que la pile P doit être conservée.**
> <br>
>   

```java script
Pile PileCopy(Pile P){
    Pile temp=initPile();
    Pile C=initPile();
    char a;
    while(!PileVide(P)){
        Sommet(P,&a);
        empiler(&P,a);
        empiler(&C,a);
        depiler(&P);

    }
    while(!PileVide(temp)){
        Sommet(temp,&a);
        empiler(&P,a);
        empiler(&C,a);
        depiler(&P);
    }return C;
}
```
<br>
** Exercice 2:

>  **En utilisant une pile, écrire un programme qui permet de saisir un entier et d'afficher sa représentation binaire (On suppose que les fonctions : initPile, pileVide, empiler, sommet et depiler sont définies)**
> <br>

```java script
Pile RepBinaire(int n){
    Pile P= initPile();
    int a;
    while(n>0){
        empiler(&P,n%2);
        n/=2;

    }
    while(!PileVide(P)){
        Sommet(P,&a);
        printf("%d",a);
        depiler(&P);
    }
}
```
<br>
** Exercice 3:

>  **Ecrire une fonction taillePile qui calcule le nombre d’éléments d’une pile**
> <br>

```java script
int TaillePile(Pile P){
    Pile temp;
    int count =0;
    int a;
    while (!PileVide(P)){
        Sommet(P,&a);
        empiler(&temp,a);
        depiler(&P);
        count ++;

    }
    while (!PileVide(temp)){
        Sommet(temp,&a);
        empiler(&P,a);
        depiler(&temp);
        count ++;

    }
return count;
}
```
<br>

**TD 5**
> **Définir les structures nécessaires**
> <br>

```java script

    typedef struct{
	int jour;
    int mois;
	int annee;
}Date;

typedef struct {
	int NPASS;
	char *nom;
	Date datre_r;
}Passager;

typedef struct Maillon {
	Passager passager;
	struct Maillon* suivant;
}Maillon;

typedef struct {
	Maillon *tete;
	Maillon *queue;
}File;



typedef struct Maillon * Liste;

typedef struct {
	int NV;
	int CAPV;
	Liste LPRN;
	File LATT;
}Vol;

```
<br>

> **Ecrire les fonctions nécessaires pour initialiser des structures**

```java script

File initFile(){
	File F;
	F.tete =NULL;
	F.queue=NULL;
	
	return F;
}

Liste initListe(){
	return NULL;
}

int FileVide(File F){
	if(F.queue==NULL && F.tete==NULL)
	 return 1;
	 
	return 0;
}

int ListeVide(Liste L){
	if(L==NULL)
	 return 1;
	 
	return 0;
}

Vol initVol(int nv,int cap){
	
	Vol V;
	V.CAPV=cap;
	V.NV=nv;
	V.LATT=initFile();
	V.LPRN=initListe();
	
	return V;
	}
```


>  **Ecrire une fonction reserverPlace pour effectuer la réservation**
> <br>
>   

```java script
void reserverPlace(Vol V, Passager P){
  if(placeDisponible(V))
    ajouterFinListe(V,P);
  else 
     ajouterFinFile(V,P);
}
```
<br>


>  **Ecrire les fonctions élémentaires suivantes :
- supprimerDebutFile
- supprimerEltDansListe
**
> <br>

```java script
Passager supprimerDebutFile(File *F){
   Maillon* m; 
   Passager p;
    if (!FileVide(*F)){
      m=F->tete;
      if(F->tete==F->queue){
       F->tete =NULL; F->queue=NULL; }
      else{
      	p=F->tete->passager;
	   F->tete=F->tete->suivant; 
  }
       free(m); 

}
 return p;
} 

int  supprimerEltDansListe(Liste L , Passager p){
	  Maillon* m; 
     if (!ListeVide(L)){
        m=L;
  
    if(m->passager.NPASS==p.NPASS){
       L=m->suivant;
       free(m);
   }
	while(m->suivant->passager.NPASS!=p.NPASS){
	
	m=m->suivant;	
    }
    m->suivant=m->suivant->suivant;
    return 1;
}
return 0;
}
```
<br>


>  **Ecrire une fonction annulerReservation pour annuler la réservation**
> <br>

```java script
int annulerReservation(Vol V, Passager p){
   supprimerEltDansListe(V.LPRN,p);
   Maillon *m;
   m=V.LPRN;
   while(m!=NULL){
   	m=m->suivant;
   }
   m->passager=supprimerDebutFile(&V.LATT);
   
}
```
<br>

>  **Ecrire une fonction affichePass qui permet d’afficher les passagers de la liste principale d’un vol qui ont
effectué une réservation dans une date donnée.**
> <br>

```java script
int comparerDate(Date d1,Date d2){
	if(d1.annee==d2.annee&&d1.mois==d2.mois && d1.jour==d2.jour)
	return 1;
	
	return 0;
}

void affichePass(Vol V, Date d){
	Maillon *m;
	m=V.LPRN;
	while(m!=NULL){
		if(comparerDate(m->passager.datre_r,d)){
			printf("%d /n",m->passager.NPASS);
			printf("***");
		}
	}
}
```
<br>


## **Réalisé par : Tadlaoui Zineb (CS)**
## **Encadré par : Pr.AMAMOU Ahmed**

<br>

---

# **Merci**
