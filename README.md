# RootkitAnalysis
Formation analyse / pratique déformation de plusieurs Rootkits.
## Qu'est-ce qu'est un rootkit ?<br/><br/>
- Programme qui effectue le raccordement du système ou modifie la fonctionnalité du système d'exploitation
- Masquer les fichiers, processus, autres objets pour masquer sa présence
- Intercepte et modifie le flux d'exécution normal
- Peut contenir des composants en mode utilisateur et en mode noyau
- Certains rootkits peuvent être installés en tant que pilotes de périphérique
- Types: rootkits en mode utilisateur et en mode kernel
## Rootkits en mode utilisateur ?<br/><br/>
- Fonctionne dans le Ring 3
- Accrochage dans l'espace utilisateur ou l'espace application
- Quelques techniques courantes du mode utilisateur Rootkit:
- Accrochage IAT (Import Address Table)
- Accrochage API en ligne
- Rootkit doit effectuer des correctifs dans l'espace mémoire de chaque application en cours d'exécution
## Rootkits en mode noyau ?<br/><br/>
- Fonctionne dans le Ring 0
- Accrochage ou modification du système dans l'espace noyau
- Quelques techniques de rootkit en mode noyau:
- Accrochage SSDT (System Service Descriptor Table)
- DKOM (Direct Kernel Object Manipulation)
- Accrochage IDT (Interrupt Descriptor Table)
- Installation en tant que pilotes de périphérique
- Accrochage IRP du pilote
## Cycle d'appel de fonction sous Windows<br/><br/>
- La capture d'écran ci-dessous montre le cycle de vie de l'API sur le système Windows<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680761993489285163/unknown.png"/><br/>
## Niveaux de hook / modification sur Windows<br/><br/>
- La capture d'écran ci-dessous montre des tableaux et des objets que le Rootkit peut accrocher / modifier pour masquer sa présence<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680762236364390461/unknown.png"/><br/>
## Note<br/><br/>
- La théorie et les techniques du rootkit seront traitées en profondeur dans un autre repo. Cette session se concentre sur l'analyse des rootkits.
# Première Demo
## Exécution du sample mader.exe<br/><br/>
- L'exécution du sample supprime un pilote et le charge en tant que service du kernel<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680764051244187703/unknown.png"/><br/>
## Activité réseau<br/><br/>
- Le malware fait une requête DNS et télécharge des fichiers supplémentaires<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680764247143481344/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680764303841951791/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680764398037762076/unknown.png"/><br/>
## Kernel Callbacks (rappels du kernel)<br/><br/>
- Rootkit utilise des rappels pour surveiller l'activité de création de processus<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680764624932569103/unknown.png"/><br/>
## Accrochage SSDT<br/><br/>
- Rootkit modifie les pointeurs dans le SSDT pour se protéger contre la suppression ou la suppression<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680764759267999751/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680764840847212544/unknown.png"/><br/>
## Soumission de rootkit à VirusTotal<br/><br/>
- VirusTotal confirme le Rootkit après avoir vidé et soumis le pilote de la mémoire<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680765034858807305/unknown.png"/><br/>
# Deuxième Demo
## Exécution du sample prolaco.exe<br/><br/>
- Prolaco.exe dépose deux fichiers sur «Googlxe.exe» et «Rundll45.exe» sur le système de fichiers<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680765278103273484/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680765331739902036/unknown.png"/><br/>
## Désactive les produits de sécurité<br/><br/>
- Empêche les produits de sécurité de fonctionner en recherchant les produits de sécurité et en supprimant leur valeur de clé de Registre<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680765607008010241/unknown.png"/><br/>
## Envoie de spams<br/><br/>
- Le malware envoie des spams d'invitation à certaines organisations<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680765766806667315/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680765835274354698/unknown.png"/><br/>
## Cache le processus<br/><br/>
- L'identifiant de processus 1080 envoie le spam, mais les rootkits cachent ce processus de la liste des processus en utilisant la technique DKOM<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680765949607149624/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680765994846912525/unknown.png"/><br/>
## Masque le processus de l'outil de sécurité<br/><br/>
- Masque le processus à partir de l'explorateur de processus<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680766148416897216/unknown.png"/><br/>
## Détecter le processus caché<br/><br/>
- La comparaison de la liste des processus à l'aide du plugin «pslist» et «psscan» de Volatility montre le processus caché prolaco.exe (pid 1080)<br/><br/>
## Pslist<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680767193084067873/unknown.png"/><br/>
## Psscan<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680767251757924402/unknown.png"/><br/>
## Vider le processus caché<br/><br/>
- Vider le processus caché de la mémoire et le soumettre à VirusTotal confirme la présence d'un processus caché malveillant<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680767440661250070/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680767479676535003/unknown.png"/><br/>
# Troisième Demo
## Exécution du sample darkmegi.exe<br/><br/>
- Le sample supprime un pilote et appelle également les processus rundll32 et iexplore<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680767685927108618/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680767729569103898/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680767783776288782/unknown.png"/><br/>
## Activité réseau<br/><br/>
- Permet au DNS d'interroger et de télécharger des fichiers supplémentaires<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680767905461174303/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680767951749513280/unknown.png"/><br/>
## Sets Callbacks (définit les rappels)<br/><br/>
- Définit les kernel callbacks sur montior pour un chargement d'image / DLL<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680768180179697684/unknown.png"/><br/>
## Examen du conducteur<br/><br/>
- Le pilote crée un périphérique et accepte les codes de contrôle des programmes en mode utilisateur<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680768329992241166/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680768380197929038/unknown.png"/><br/>
## Objet Device et Driver<br/><br/>
- L'examen des objets périphérique et pilote montre l'adresse de base et l'adresse de la routine DriverEntry<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680768523911299119/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680768563291619370/unknown.png"/><br/>
## Routine DriverEntry<br/><br/>
- L'examen de la routine DriverEntry montre la présence d'une DLL «com32.dll»<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680768680274952202/unknown.png"/><br/>
## Vidage de la DLL de la mémoire<br/><br/>
- La DLL vidée de la mémoire, qui a été chargée par rundll32.exe<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680768794578255893/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680768829378527273/unknown.png"/><br/>
## Vidage du pilote de périphérique<br/><br/>
- Dumping du conducteur après avoir déterminé l'adresse de base du conducteur<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680768947506905099/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680768980947828836/unknown.png"/><br/>
## Soumission de DLL et de pilote<br/><br/>
- VT confirme le composant Rootkit après avoir soumis les échantillons<br/><br/>
## Résultats de VT pour le pilote sous-jacent<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680769205641281546/unknown.png"/><br/>
## Résultats de VT pour le sous-programme DLL<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680769292819497000/unknown.png"/><br/>
# Quatrième Demo
## Exécution du sample carberp.exe<br/><br/>
- L'exemple crée des fichiers .tmp et appelle explorer.exe et svchost.exe<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680769494200877079/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680769529584156712/unknown.png"/><br/>
## Activité réseau<br/><br/>
- Un logiciel malveillant effectue des activités DNS et http<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680769656104943651/unknown.png"/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680769697368768522/unknown.png"/>
## Soumet des informations sur le processus<br/><br/>
- Soumet les informations de processus sur le système au serveur de commande et de contrôle<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680769798866731100/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680769833142583357/unknown.png"/><br/>
## Patch SYSCALL<br/><br/>
- Correctifs SYSCALL dans ntdll.dll pour masquer sa présence<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680770192497836097/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680770232763416636/unknown.png"/><br/>
## Crochets API en ligne<br/><br/>
- Hooks API appelle pour voler des données http, les fonctions de hook pointent vers un emplacement inconnu<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680770336354336787/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680770370793504772/unknown.png"/><br/>
## Exécutable intégré<br/><br/>
- Vidage de l'exécutable incorporé qui a été déterminé en examinant la fonction d'accrochage.<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680770488112513053/unknown.png"/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680770516939702275/unknown.png"/><br/>
## Soumission EXE intégrée<br/><br/>
-  VirusTotal confirme l'exécutable en tant que composant de Carberp<br/><br/>
<img src="https://media.discordapp.net/attachments/680760382935007467/680770880053313537/unknown.png"/><br/>

