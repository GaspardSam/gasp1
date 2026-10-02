Écris un script Python complet (OpenCV + numpy uniquement) qui redresse une image d'un objet quelconque (ex. un avion) puis zoome autour de deux points choisis par l'utilisateur.

Fonctionnement :
1. Le chemin de l'image est lu depuis sys.argv, sinon demandé avec input(). Gère l'erreur si l'image est introuvable ou illisible.
2. L'image s'affiche, redimensionnée pour tenir à l'écran si elle est grande. L'utilisateur clique sur 2 points définissant l'axe de l'objet. Le PREMIER clic est le point qui doit se retrouver à GAUCHE après redressement, le SECOND celui qui doit se retrouver à DROITE (ex. queue puis nez de l'avion). Les clics sont convertis en coordonnées de l'image d'origine (attention au facteur d'échelle de l'affichage). Chaque point est dessiné (cercle rouge + numéro) et une ligne relie les deux points une fois les 2 clics faits.
3. Un message dans la fenêtre guide l'utilisateur ("Clic 1/2 : point gauche", "Clic 2/2 : point droit", puis "Entrée : valider | r : recommencer | q : quitter"). Entrée valide, r efface les points, q quitte proprement.
4. Calcule l'angle de la droite entre les 2 points avec atan2(dy, dx). Fais tourner l'image (cv2.getRotationMatrix2D + cv2.warpAffine, INTER_CUBIC) pour que cette droite devienne parfaitement HORIZONTALE, avec le premier point à gauche du second. Le canevas doit être agrandi (nouvelle largeur/hauteur calculées avec cos et sin de l'angle, translation ajoutée à la matrice) pour que rien ne soit coupé, quel que soit l'angle, y compris 90° ou 180°.
5. Transforme les 2 points avec la MÊME matrice de rotation pour obtenir leurs positions dans l'image redressée.
6. Zoom : recadre une zone centrée sur le milieu des 2 points, de largeur 2 × la distance entre les points et de hauteur = largeur × 3/4. La zone doit toujours rester dans les limites de l'image (clamp) et avoir une taille minimale pour éviter une zone vide si les points sont très proches. Agrandis le recadrage à une largeur de sortie fixe (800 px) avec cv2.INTER_CUBIC.
7. Affiche l'image redressée (avec les 2 points marqués) et le zoom, sauvegarde le zoom dans "resultat_zoom.png" et affiche dans la console l'angle d'inclinaison initial de l'objet en degrés (positif = penché dans le sens horaire sur l'écran).

Cas limites à gérer : les deux clics au même endroit (distance nulle) → message d'erreur et redemander les points ; échec de cv2.imwrite ; fermeture des fenêtres avec cv2.destroyAllWindows dans un bloc finally.

Contraintes de code :
- Fonctions séparées : select_points, rotate_image, zoom_on_points, main, avec if __name__ == "__main__": sys.exit(main()).
- Chaque fonction a une docstring complète en français : rôle, description de chaque paramètre (type et signification), valeur de retour.
- Commentaires en français sur les étapes importantes (conversion des coordonnées, agrandissement du canevas, clamp du recadrage).
- Constantes en haut du fichier pour les tailles d'affichage, la marge du zoom et la largeur de sortie.
- Code simple, lisible, sans autre bibliothèque qu'OpenCV et numpy.
