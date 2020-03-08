#Exercice 1 : Variables d'environnement

1. PATH est une variable d'environnement qui définit la liste des répertoire dans lequel chercher un programme à exécuter.
Le contenu de cette variable d'environnement est une liste de répertoires séparés par ":" . Pour afficher son contenu, 
nous avons tapé la commande printenv PATH nous permettant d'afficher où trouver les commandes tapées par l'utilisateur. 
Les commandes tapées par l'utilisateur se trouvent dans les dossiers : /usr/local/sbin, /usr/local/bin, /usr/sbin, /usr/bin
, /sbin, /bin:/usr/games, /usr/local/games,/snap/bin.

2. La variable d'environnement HOME contient le chemin vers le répertoire personnel. Ainsi, pour nous ramener au répertoire
personnel, nous devons taper cd $HOME avec le symbole $ pour récupérer la valeur de cette variable d'environnement.

3. 
    La variable d'environnement LANG contient le paramètre linguistique de base utilisé par les applications du système
    pour communiquer avec l'utilisateur ex: fr_FR.UTF-8.
    
    La variable d'environnement PWD contient le chemin du répertoire de travail courant de l'interpréteur de commande.
    
    La variable d'environnement OLDPWD contient le chemin du dernier répertoire de travail courant avant d'avoir tapé 
    la commande cd pour changer de répertoire courant.
    
    La variable d'environnement SHELL contient le chemin vers l'interpréteur de commande préféré de l'utilisateur tel
    qu'il est défini dans le fichier /etc/passwd. Dans notre cas, sa valeur est /bin/bash car l'interpréteur de commande
    préféré est bash.
    
    La variable d'environnement _ correspond au chemin à emprunter pour trouver le script de la commande appelée.
    Ainsi, lorsqu'on tape printenv _, on obtient /usr/bin/printenv.
    


4. On crée la variable avec la commande export VAR="variable" et on la visualise avec printenv VAR

5. La variable crée n'est pas reconnue en mode root car sa creation est temporaire et effective uniquement pour la 
sessions courante lors de sa creation.

6. En initialisant la variable dans le fichier .bashrc (depuis le repertoire personnel ~/ ) on rend la 
creation/modification d'une variable permanente. En effet ce fichier est lu a chaque demarrage du bash.

7. On initialise la variable NOMS dans .bashrc et on execute la commande source .bashrc pour forcer bash a relire
le fichier .bashrc (reboot du bash)

8. On tape la ligne de commande echo "Bonjour vous deux, $NOMS !"

9. unset permet de supprimer la référence a la variable alors qu'une variable vide a une référence mais qui ne pointe
vers rien.

10. On tape la ligne de commande echo "\$HOME = $HOME"



#Exercice 2

##Consigne: 
Écrivez un scripttestpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au contenu 
d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur 
ne doit pas s’aﬀicher.


    testpwd.sh:
#!/bin/bash 
export pwd="password" 
echo "Saisie mot de passe:" 
read -s mdp 
if [ $mdp = $pwd ]; then 
    echo "Ok" 
else 
    echo "Mot de passe faux!" 
fi


#Excercice 3

##Consigne:
Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre est un nombre
réel. il affichera un message d'erreur dans le cas contraire.

nb_reel.sh: 
#!/bin/bash 

function is_number() { 

re='^[+-]?[0-9]+([.][0-9]+)?$' 
if ! [[ $1 =~ $re ]] ; then 
    return 1 
else 
    return 0 
fi 
}

is_number $1 if [ $? = 0 ]; then 
    echo "Nombre reel" 
else 
    echo "Type non reel"
fi


#Exercice 4

##Consigne :
Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le script 
est appelé sans nom d’utilisateur, il aﬀiche le message : ”Utilisation :nom_du_scriptnom_utilisateur”,où nom_du_script
est le nom de votre script récupéré automatiquement (si vous changez le nom de votre script, le message doit changer 
automatiquement).

Code de users.sh: 
#!/bin/bash

function verifyUser() { 
res=grep $1 /etc/passwd echo $res 
if [ $(#res) -ne 0 ]; then 
    echo "L'utilisateur existe" 
    return 1 
fi 
echo "L'utilisateur n'existe pas" 
return 0 
}

if [ -z $1 ]; then 
    echo "Utilisation de :" $0 ": cmd + nom-utilisateur" 
    exit 
fi 
verifyUser $1

#Exercice 5 

##Consigne : 
Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre 
(on supposera que l’utilisateur saisit toujours un entier naturel).

facto.sh: 
#! /bin/bash 
export res=1 
for (( i = 1; i <= $1; i++ )) do res=$(($res * $i)) 
done echo $res


#Exercice 6

##Consigne :
Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner.
Le programme écrira ”C’est plus!”, ”C’est moins!” ou ”Gagné!” selon les cas (vous utiliserez$RANDOM).


justePrix.sh: 
#! /bin/bash 
num=$RANDOM RANGE=100 let "num %= $RANGE" 
guess=$1

function justePrix() {
if [ $guess -lt $num ]; then
    echo "C'est plus!" 
    read guess justePrix 
elif [ $num -lt $guess ]; then 
    echo "C'est moins!" 
    read guess justePrix 
else 
    echo "C'est gagné!" 
    return 1 
fi 
}

if [ -z $1 ]; then 
    echo "Utilisation de :" $0 ": cmd + numero" 
    exit 
fi 
justePrix $guess


#Exercice 7 

##Consigne :
1.Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et aﬀiche le min, le maxet la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres sont bien des entiers.

2.Généralisez le programme à un nombre quelconque de paramètres (pensez àSHIFT).

3.Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et stockées au fur et
à mesure dans un tableau.


##Question 1

#!/bin/bash



function moyenne {
	moyenne=0
	moyenne=$(echo "( $moyenne + $1 + $2 + $3 ) / 3" | bc -l )
	echo "la moyenne est : $moyenne"
}

function max {
	max=$1
	if [[ $max -lt $2 ]]; then
		max=$2
	fi
	if [[ $max -lt $3 ]]; then
		max=$3
	fi
	echo "la maximum est : $max"
}

function min {
	min=$1
	if [[ $2 -lt $min ]]; then
		min=$2
	fi
	if [[ $3 -lt $min ]]; then
		min=$3
	fi
	echo "le minimum est : $min"
}

nb1=$1

nb2=$2

nb3=$3

moyenne $nb1 $nb2 $nb3
max $nb1 $nb2 $nb3
min $nb1 $nb2 $nb3





