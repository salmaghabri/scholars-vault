# MVC
• **Model** • Données de l’application• Méthodes manipulant ces données • Stockage et extraction de la BD 
• **View** • Représentation visuelle du modèle• Gère les interactions utilisateur 
• **Contrôleur** • Accès aux données à partir du modèle • Affichage des données dans les vues • Intermédiaire entre plusieurs vues et modèles

le moins rigide le moins maintenable Observe les changements du modèle et les transmet à la vue
![[MVW-4.png]]
#### flux:
• Les entrées utilisateur sont interceptées par le contrôleur • Un contrôleur peut faire appel à plusieurs vues (erreur, succès…) • Une vue n’a pas de visibilité sur son contrôleur • Le contrôleur passe le modèle à la vue • Le modèle a en général peu (ou pas) de méthodes (comportement)
#### ref
• La vue fait référence au modèle, mais pas le contraire • La vue «observe » les changements du modèle, et s’y adapte • Le contrôleur fait référence au modèle, le remplit et le passe à la vue • La vue n’a pas de visibilité sur le contrôleur, mais fait référence et s’attend à un modèle particulier
# MVVM
1 viewmodel plusieurs view -> moins de separation -> moins testablité
cross model
![[MVW-2.png]]
##### flux:
• L’ entrée utilisateur commence par la vue et peut provoquer l’exécution d’un comportement ViewModel • La vue et le modèle ne communiquent jamais • ViewModel est un modèle fortement typé de la vue, qui est une réflexion exacte ou une abstraction de cette vue • ViewModel et Vue sont toujours synchronisés • Le modèle n’a aucune idée que le VM ou la vue existent
##### ref
• Un VM n’a pas besoin de référencer la vue • La vue est reliée à des propriétés du VM • La vue n’a aucune idée sur l’existence du modèle M  • Le VM et le modèle n’ont pas de référence ou visibilité sur la vue • Le modèle n’a aucune idée de l’existence de la vue et du VM
# MVP
![[MVW-3.png]]
• L’ entrée utilisateur commence par la vue, pas par le Presenter • La vue invoque les commandes du Presenter, et le Presenter modifie la vue • La vue et le modèle ne communiquent jamais • Le Presenter est une couche d’abstraction de la vue • Chaque vue a un Presenter qui lui est associé (1-to-1)
##### ref
• Le Presenter a besoin d’une référence vers la vue • La vue a également une référence vers son Presenter 1• C’est le Presenter qui va charger la vue à partir du modèle, pas le modèle
