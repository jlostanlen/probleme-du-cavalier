# Problème du cavalier
Implémentation d'un algorithme Python avec interface graphique pour permettre à un cavalier de parcourir un échiquier sans passer deux fois par la même case.
J'ai réalisé ce projet en colaboration avec Erwan Fertray et Marc Blanchet.

## Code

import turtle

def init_echiquier(): 
  echiquier = [[0]*n for i in range(m)] # 0 : case vide; 1 : case visitée; 2 : case occupée par le cavalier
  return echiquier 


# Placer le cavalier
def placer_cavalier(indiceLigne, indiceColonne):
  if (indiceColonne < n and indiceColonne >= 0 and indiceLigne < m and indiceLigne >=0):
    echiquier[indiceLigne][indiceColonne] = 2
  else:
    print("Vous avez fourni de mauvaises coordonnées")

    
# Affichage de l'échiquier
def afficher_echiquier(echiquier):
  for j in range(n):
    print("--", end="")
  print("\n")
  for i in range (m):
    for j in range (n):
      if (echiquier[i][j] == 0):
        print(" |", end="")
      elif (echiquier[i][j] == 1):
        print("x|", end="")
      else:
        print("C|", end="")
    print("\n")
    for j in range(n):
      print("--", end="")
    print("\n")



# Effectue les calculs des coordonnées d'arrivée du cavalier et vérifie si le déplacement est possible
def trouverCoordonnees(indiceLigneDep,indiceColonneDep): 
  global dejaTeste
  tableauCalcul = []  
  tableauCalcul.append((indiceLigneDep + 1, indiceColonneDep + 2))
  tableauCalcul.append((indiceLigneDep + 1, indiceColonneDep - 2))
  tableauCalcul.append((indiceLigneDep + 2, indiceColonneDep + 1))
  tableauCalcul.append((indiceLigneDep + 2, indiceColonneDep - 1))
  tableauCalcul.append((indiceLigneDep - 1, indiceColonneDep + 2))
  tableauCalcul.append((indiceLigneDep - 1, indiceColonneDep - 2))
  tableauCalcul.append((indiceLigneDep - 2, indiceColonneDep + 1))
  tableauCalcul.append((indiceLigneDep - 2, indiceColonneDep - 1))
  
  
  for i in tableauCalcul:
    indiceLigneArrivee = i[0]
    indiceColonneArrivee = i[1]
    # Sortie par le haut ou par le bas
    if (((indiceLigneArrivee) >= m) or ((indiceLigneArrivee ) < 0)):
      continue
    # Sortie par la gauche ou par la droite
    elif (((indiceColonneArrivee) >= n) or ((indiceColonneArrivee) < 0)):
      continue
    # Vérifie si cette possibilité n'a pas déjà étée testée
    elif (i in dejaTeste[n * indiceLigneDep + indiceColonneDep]):
      continue
    # S'il n'y a eu aucune sortie, on vérifie si la case a été visitée ou non
    elif (echiquier[indiceLigneArrivee][indiceColonneArrivee] == 0):   
      return i # On retourne la case d'arrivée trouvée
  # S'il n'y a aucun déplacement possible 
  return False         
  


  
# Parcours de l'échiquier
# Retourne False si aucun déplacement n'est possible dès le départ
def parcourir(indiceLigneDep, indiceColonneDep):
  global echiquier
  global dejaTeste

  for i in range(0,m*n):
    dejaTeste[i] = []
  

  trajet = []
  trajet.append((indiceLigneDep,indiceColonneDep))
  
  while (True):
    coordonneesChoisies = trouverCoordonnees(indiceLigneDep, indiceColonneDep) # On stoque ce que retourne la fonction trouverCoordonnees
    # S'il n'y a pas de case libre
    if (coordonneesChoisies == False): 
      if (len(trajet) >= m*n):           # Si on a visité toutes les cases de l'échiquier
        print("Echiquier parcourus, fin du programme")     # Indique la fin du programme
        return trajet
      elif (len(trajet) == 1):           # Si on est au départ du parcours et qu'il n'y a aucune possibilité
        print("Pas de solution trouvé")
        return False 
      
      else: 
        echiquier[indiceLigneDep][indiceColonneDep] = 0 # on met la case actuelle comme non visitée
        dejaTeste[(n * (indiceLigneDep) + indiceColonneDep)].clear() # on clear la liste des points déjà visités du points qu'on enlève
        dejaTeste[(n * (trajet[len(trajet)-2][0]) + trajet[len(trajet)-2][1])].append((indiceLigneDep,indiceColonneDep)) # Ajoute le dernier élément de la liste "trajet"au point parent (avnt lui) au dictionnaire "dejaTeste" 
        (indiceLigneDep, indiceColonneDep) = (trajet[len(trajet)-2][0],trajet[len(trajet)-2][1]) #on change la position du cavalier, on revient au parent 
        echiquier[trajet[len(trajet)-2][0]][trajet[len(trajet)-2][1]] = 2 #on met a 2 la position du cavalier sur l'échiquier
        trajet.pop() #on retire le dernier point de trajet vu qu'il n'est pas bon

    else :
      echiquier[indiceLigneDep][indiceColonneDep] = 1 # indique que la case est visitée
      (indiceLigneDep,indiceColonneDep) = coordonneesChoisies #change les coordonnées par celle choisie
      echiquier[indiceLigneDep][indiceColonneDep] = 2 #on met a 2 la position du cavalier sur l'échiquier
      trajet.append((indiceLigneDep,indiceColonneDep)) # on ajoute à trajet le point choisiee


def trouverCoordonneesHamiltonien(indiceLigneDep,indiceColonneDep,trajet): 
  global dejaTeste
  tableauCalcul = []  
  tableauCalcul.append((indiceLigneDep + 1, indiceColonneDep + 2))
  tableauCalcul.append((indiceLigneDep + 1, indiceColonneDep - 2))
  tableauCalcul.append((indiceLigneDep + 2, indiceColonneDep + 1))
  tableauCalcul.append((indiceLigneDep + 2, indiceColonneDep - 1))
  tableauCalcul.append((indiceLigneDep - 1, indiceColonneDep + 2))
  tableauCalcul.append((indiceLigneDep - 1, indiceColonneDep - 2))
  tableauCalcul.append((indiceLigneDep - 2, indiceColonneDep + 1))
  tableauCalcul.append((indiceLigneDep - 2, indiceColonneDep - 1))
  
  for i in tableauCalcul:
    indiceLigneArrivee = i[0]
    indiceColonneArrivee = i[1] 
    # Sortie par le haut ou par le bas
    if (((indiceLigneArrivee) >= m) or ((indiceLigneArrivee ) < 0)):
      continue
    # Sortie par la gauche ou par la droite
    elif (((indiceColonneArrivee) >= n) or ((indiceColonneArrivee) < 0)):
      continue  
    # Vérifie si cette possibilité n'a pas déjà étée testée
    elif (i in dejaTeste[n * indiceLigneDep + indiceColonneDep]):
      continue
    # S'il n'y a eu aucune sortie, on vérifie si la case a été visitée ou non
    elif (i == trajet[0] and len(trajet) >= m*n ):
      return i
    
    elif (echiquier[indiceLigneArrivee][indiceColonneArrivee] == 0):   
      return i # On retourne la case d'arrivée trouvée
  # S'il n'y a aucun déplacement possible 
  return False

def parcourirHami(indiceLigneDep, indiceColonneDep):
  global echiquier
  global dejaTeste
  

  for i in range(0,m*n):
    dejaTeste[i] = []
  
  trajet = []
  trajet.append((indiceLigneDep,indiceColonneDep))
  alltraj = [] #liste des des trajet déjà fait
  
  while (True):
    coordonneesChoisies = trouverCoordonneesHamiltonien(indiceLigneDep, indiceColonneDep,trajet) # On stoque ce que retourne la fonction trouverCoordonnees
    # S'il n'y a pas de case libre
    if( coordonneesChoisies == trajet[0]): #si le prochain point est égale au premier ppnt de la liste trajet alors c'est un cycle hamiltonien
      print("Chemin hamiltonien : ", trajet)
      return trajet
    elif (coordonneesChoisies == False): 
      if (len(trajet) >= m*n):       # Si on a visité toutes les cases de l'échiquier
        alltraj.append(trajet)
        echiquier[indiceLigneDep][indiceColonneDep] = 0 # on met la case actuelle comme non visitée
        dejaTeste[(n * (indiceLigneDep) + indiceColonneDep)].clear() # on clear la liste des points déjà visités du points qu'on enlève
        dejaTeste[(n * (trajet[len(trajet)-2][0]) + trajet[len(trajet)-2][1])].append((indiceLigneDep,indiceColonneDep)) # Ajoute le dernier élément de la liste "trajet"au point parent (avnt lui) au dictionnaire "dejaTeste" 
        (indiceLigneDep, indiceColonneDep) = (trajet[len(trajet)-2][0],trajet[len(trajet)-2][1]) #on change la position du cavalier, on revient au parent 
        echiquier[trajet[len(trajet)-2][0]][trajet[len(trajet)-2][1]] = 2 #on met a 2 la position du cavalier sur l'échiquier
        trajet.pop() #on retire le dernier point de trajet vu qu'il n'est pas bon
        continue
      elif (len(trajet) == 1): # Si on est au départ du parcours et qu'il n'y a aucune possibilité
        print("Pas de solution trouvé")
        return False 
      
      else:
        echiquier[indiceLigneDep][indiceColonneDep] = 0 # on met la case actuelle comme non visitée
        dejaTeste[(n * (indiceLigneDep) + indiceColonneDep)].clear() # on clear la liste des points déjà visités du points qu'on enlève
        dejaTeste[(n * (trajet[len(trajet)-2][0]) + trajet[len(trajet)-2][1])].append((indiceLigneDep,indiceColonneDep)) # Ajoute le dernier élément de la liste "trajet"au point parent (avnt lui) au dictionnaire "dejaTeste" 
        (indiceLigneDep, indiceColonneDep) = (trajet[len(trajet)-2][0],trajet[len(trajet)-2][1]) #on change la position du cavalier, on revient au parent 
        echiquier[trajet[len(trajet)-2][0]][trajet[len(trajet)-2][1]] = 2 #on met a 2 la position du cavalier sur l'échiquier
        trajet.pop() #on retire le dernier point de trajet vu qu'il n'est pas bon

    else :
      echiquier[indiceLigneDep][indiceColonneDep] = 1 # indique que la case est visitée
      (indiceLigneDep,indiceColonneDep) = coordonneesChoisies #change les coordonnées par celle choisie
      echiquier[indiceLigneDep][indiceColonneDep] = 2 #on met a 2 la position du cavalier sur l'échiquier
      trajet.append((indiceLigneDep,indiceColonneDep)) # on ajoute à trajet le point choisiee

# Dessine l'échiquier de dimension m*n a l'aide de turtle
def dessine_echiquier(m,n):
    for i in range(m):
        t.up()
        t.speed(0) # La vitesse la plus rapide
        t.setpos(0, 33.5*i) # Il s'agit du positionnement sur la ligne. On monte d'une case a chaque fois qu'on a terminé une ligne et on revient a gauche de l'échiquier
        t.down()    
        for j in range(n):
            if ((i+j)% 2 == 0): # Une case sur 2 doit être noire, et on alterne selon la ligne
                couleur = 'black'
            else:
                couleur = 'white'
            t.fillcolor(couleur)
            t.begin_fill()
            for k in range(4): # Dessin d'une case
                t.fd(33.5)
                t.left(90)
            t.fd(33.5)
            t.end_fill()
        t.hideturtle()

def dessine_trajet(e): # Dessine le trajet de la tortue
    t.hideturtle()
    t.up()
    t.speed(0)
    t.setpos((e[0][1]+1)*33.5-(33.5/2),(m-e[0][0])*33.5-(33.5/1.25)) #On place la tortue a la position initiale du cavalier
    t.color("#0000ff") # On dessine un cercle bleu a cet endroit
    t.begin_fill()
    t.circle(33.5/4)
    t.end_fill()
    t.up()
    t.setpos((e[0][1]+1)*33.5-(33.5/2),(m-e[0][0])*33.5-(33.5/2))
    t.down()
    for i in range(len(e)):
        t.goto((e[i][1]+1)*33.5-(33.5/2),(m-e[i][0])*33.5-(33.5/2)) # On va vers les directions des cases
    t.up()
    t.goto((e[i][1]+1)*33.5-(33.5/2),(m-e[i][0])*33.5-(33.5/1.25)) # On positionne la tortue a la position finale du cavalier mais légèrement plus bas sinon ça fait n'importe quoi et les cercles ne sont pas centrés
    t.color("#00ff00") # Le cercle final est vert
    t.begin_fill()
    t.circle(33.5/4)
    t.end_fill()


# PROGRAMME PRINCIPAL
dejaTeste = {}     # Dictionnaire qui stoque chaque possibilité déjà testée pour un emplacement de cavalier donné
m = int(input("Hauteur de l'échiquier : "))
n = int (input("Largeur de l'échiquier : "))
while ((m < 1) or (n < 1)):
  print("La hauteur et la largeur doivent être supérieurs à 0")
  m = int(input("Hauteur de l'échiquier : "))
  n = int (input("Largeur de l'échiquier : "))
echiquier = init_echiquier()
ecran = turtle.getscreen() # Création de l'écran
t= turtle.Turtle() # Création de la tortue
# Dimensions de l'échiquier
nbCases = m * n 

placer_cavalier(m-1,0)
afficher_echiquier(echiquier)
a = parcourirHami(m-1,0)
print(a)
dessine_echiquier(m, n)
if (a == False ):
    print("Dessin impossible")
else :
    dessine_trajet(a)

