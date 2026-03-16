App.js

App.js est le composant principal de l’application. C’est lui qui décide ce qu’on affiche à l’écran. 
Par exemple, si l’utilisateur n’est pas encore connecté, on affiche la page Join. Et quand il est connecté, on affiche la page Chat.
Il garde aussi des informations importantes comme le pseudo, la room choisie et l’état de connexion.
Donc pour moi, App.js sert surtout à contrôler le fonctionnement général de l’application.

Join.js

Join.js sert à faire entrer l’utilisateur dans le chat. Dans cette page, on écrit le pseudo, on choisit une room, et on peut aussi créer une nouvelle room.
Ce composant écoute aussi la liste des rooms envoyée par le serveur. Quand l’utilisateur clique pour rejoindre, il envoie un événement join_room avec le pseudo et le nom de la room.
Donc son rôle principal est de gérer l’entrée dans le système de chat.

Chat.js

Chat.js est la partie principale du chat.C’est dans ce composant qu’on voit les messages, les participants et la zone pour écrire.
Il écoute les événements du serveur pour afficher les nouveaux messages et mettre à jour la liste des utilisateurs.
Il contient aussi la fonction qui permet d’envoyer un message au serveur. Donc ce fichier représente vraiment le cœur du chat.

Message.js

Message.js sert à afficher un seul message. Il regarde si le message vient de l’utilisateur connecté ou d’un autre utilisateur.
S’il vient de moi, le message s’affiche à droite, sinon il s’affiche à gauche.
Il affiche aussi l’auteur, le texte du message et l’heure. Donc son rôle est surtout l’affichage individuel des messages.

Sidebar.js

Sidebar.js sert à afficher les participants qui sont dans la room.
Il montre leur nom et aussi un petit indicateur pour dire qu’ils sont en ligne.
C’est donc le composant qui gère la partie latérale du chat.

server.js

server.js est le serveur backend de l’application. C’est lui qui gère Socket.io, les rooms, les connexions, les déconnexions et l’envoi des messages.
Quand un utilisateur rejoint une room, le serveur l’ajoute dans cette room.
Quand un message est envoyé, c’est le serveur qui le redistribue aux autres utilisateurs de la même room.
Donc sans ce fichier, le chat en temps réel ne fonctionnerait pas.


Question 2

Dans ce projet, le socket est partagé dans l’application avec SocketContext.
Ça veut dire qu’on ne crée pas le socket directement dans chaque composant.
À la place, on utilise useSocket() pour récupérer le socket déjà disponible.
Cette méthode est plus propre et évite des problèmes quand plusieurs composants utilisent la même connexion.

Quand l’utilisateur rejoint une room, le client envoie l’événement join_room au serveur avec deux informations : le username et la room.
Ensuite, le serveur reçoit cet événement et utilise socket.join(room) pour mettre l’utilisateur dans la bonne room.
Après ça, il envoie aussi des mises à jour comme les utilisateurs présents et un message système pour dire qu’une personne a rejoint.

Quand un utilisateur écrit un message, le client envoie l’événement send_message avec les données du message.
Le serveur reçoit ces données puis il les rediffuse à tous les utilisateurs de la même room avec io.to(data.room).emit("receive_message", data).
C’est pour ça que tous les membres de la room voient le message en temps réel.