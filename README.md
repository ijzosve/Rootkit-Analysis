# RootkitAnalysis
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
