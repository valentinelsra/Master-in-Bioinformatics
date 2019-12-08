# Projet Assemblage

Grâce à des nouvelles technologies dites de séquençage, il est aujourd'hui possible de lire le génome d’un individu rapidement et à moindre coût. Le séquençage produit un grand nombre de petits segments du génome appelés lectures (_**reads**_ en anglais).

Un _**read**_ est une courte chaîne de bases nucléiques (soit de caractères parmi les quatre suivants : ‘A’, ‘T’, ‘G’, ‘C’). Chaque read est identifié par un identifiant unique numérique. Pour chaque read on connaît la _qualité_ de séquençage des bases nucléiques qui le composent : chaque base nucléique dans un read a une valeur de qualité associée correspondant à un nombre réél compris dans l’intervalle [0, 1].

_Par exemple le read 1 est défini par la séquence de taille 10 « AGGGTCCCGA » et les valeurs de qualité suivantes « 0.3 0.25 0.7 0.9 0.95 0.3 0.91 0.87 0.4 0.35 »_

Par ce programme, nous modélisons en langage Python un ensemble de tels reads de tailles L (comprise entre _20_ et _100)_, produits suite au séquençage d’une séquence génomique.


# Programme

Dans cette section, vous trouvez tout ce qu'il y a savoir ce programme : comment le faire démarrer sur votre ordinateur, son fonctionnement ainsi que le détail des fonctions utilisées.

## Pré-requis pour le démarrage

Vous nécessiterez un fichier contenant la séquence nucléique de  votre gène à séquencer. Cette séquence devra être conforme dans son intégralité à l'exemple du read fourni en introduction.

## Fonctionnement

Dans un premier temps, chargeons la séquence nucléique.

    sequence = readFASTA("gene.fasta")

Nous générons ensuite des reads. C'est une simulation du séquençage de cette séquence nucléique.

    generer(sequence)

Une liste d'adapteurs est mise au point. Ces motifs correspondent à des adaptateurs que nous aurions pu ajouter aux extrémités de notre séquence nucléique lors d'une réelle expérience de séquençage.

    Adaptateurs = ["DLKZEF","OEFNZEI","AZEA","KEFOJZEFAA"]

Les reads sont filtrés en fonction des adaptateurs.  Il est possible de modifier ces adaptateurs selon le protocole utilisé pendant le séquençage. Les résultats du filtrage sont affichés à l'écran.

    Filtrage = FiltreAdaptateurs(a, 		  				
    Adaptateurs)
    print(Filtrage)

Le seuil de qualités des nucléotides est fixé à 0,6. Il est également possible de modifier cette valeur.

    SeuilQualite = 0.6


## Fonctions utilisées

Nous créons une fonction permettant le stockage de la séquence nucléique du gène. Si cette séquence respecte un format Fasta, elle est nettoyée des éventuels espaces et incluse sous la forme d'une chaîne de caractère dans une liste elle-même incluse dans un dictionnaire.

    def  readFASTA():
	    filename = input("Selectionner un fichier")
		dic={}
	    f=open(filename,"r")
	    a=f.readline()
	    a=a.strip()
	    while a !=  "" :
		    li=[]
		    ch=""
		    if a[0] ==  ">":
			    li.append(a)
			    a=f.readline()
			    a=a.strip()
			    while a !=  ""  and a[0] !=  ">" :
				    ch=ch+a
				    a=f.readline()
				    a=a.strip()
				li.append(ch)
				dic[li[0]]=li[1]
			a=f.readline()
			a=a.strip()
		f.close()
		return dic
				    

La séquence est découpée en reads de taille fixe (100nt) qui seront ensuite enregistrés dans un fichier.

    def  generer(dic):
	    seq=dic.values()
		seq=seq[0]
		filename = input("Selectionner un nom de  fichier")
	    f=open("readsset","w")
		for i in  range (10):
			pos=random.randint(0,len(seq))
			reads=""
			qual=""
			if pos>len(seq)-100 :
				fin =  len(seq)
			else:
				fin = pos+100
			for j in  range(pos,fin):
				reads=reads+seq[j]
				qual=qual+str(round(random.uniform(0,1),2))+" "
			f.write(reads)
			f.write(" /")
			f.write(qual)
			f.write("\n")
		f.close()
		return

				
Le traitement des données commence. Les reads sont récupérés d'un fichier et stockés dans un dictionnaire. 

    def  LoadReadSet():
	    filename = input("Selectionner un fichier de reads")
	    f=open(fichier,"r")
	    dic={}
	    line=f.readline()
	    i=1
	    while line !=  "":
		    line=line.strip()
		    line=line.split(" /")
		    qual=line[1].split(" ")
		    for j in  range (len(qual)):
			    qual[j]=  float (qual[j])
			dic["R"+str(i)]=[line[0],qual]
		    i+=1
		    line=f.readline()
		f.close()
		return dic

 
Filtrage :

    def  FiltreAdaptateurs(ReadSet, Adaptateurs):
	    for val in ReadSet.values():
		    read_qual=val
		    read=read_qual[0]
		    for i in  range (len(Adaptateurs)):
			    fin =  len(Adaptateurs[i])-1
			    a=read[0:fin]
			    if a == Adaptateurs[i] :
				    a=read[0:fin]
				    read=read.replace(a,"")
				elif read[-fin:len(read)] == Adaptateurs[i] :
					b=read[-fin:len(read)]
					read=read.replace(b,"")
		return ReadSet

THEN

    def  FiltreAdaptateurs(ReadSet, Adaptateurs):


THEN

    def  Filtre (ReadSet, SeuilQualite):

THEN

    def  ReverseComplement (Read):

THEN

    def  BestReadChevauchant (Read, ReadSet):


## Conçu avec :

 - Emacs (programmation en Python3)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI1NTc3MTYzMiwxNzA5MjkzNzA3XX0=
-->