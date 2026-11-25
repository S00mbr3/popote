# La popote de l'interne pressé

Application de batch cooking : chaque semaine, une proposition de plats (viande ou poisson, végétarien, végétalien, soupes, kamado, air fryer, petits-déjeuners), la liste de courses calculée à partir des plats choisis et le plan de cuisine du dimanche optimisé, avec minuteurs.

- `index.html` : l'application (un seul fichier).
- `data/sNN.js` : une semaine de recettes par fichier, ajoutée chaque jeudi soir.
- `manifest.webmanifest`, `sw.js`, `icons/` : installation sur l'écran d'accueil et fonctionnement hors ligne.

Les recettes inspirées de livres sont réécrites et adaptées ; aucun texte de livre n'est reproduit ici.

## Synchronisation entre appareils (facultatif, Supabase)

Sans réglage, chaque téléphone garde ses propres choix. Pour partager la sélection, les courses cochées et les notes :

1. Créer un projet gratuit sur [supabase.com](https://supabase.com).
2. Dans **SQL Editor**, exécuter :

```sql
create table if not exists popote_state (
  id text primary key,
  data jsonb not null,
  updated_at timestamptz default now()
);
alter table popote_state enable row level security;
create policy "popote lecture" on popote_state for select to anon using (true);
create policy "popote ajout"   on popote_state for insert to anon with check (true);
create policy "popote maj"     on popote_state for update to anon using (true) with check (true);
```

3. Dans l'appli : **Réglages › Synchronisation Supabase**, coller l'URL du projet et la clé publique `anon` (Project Settings › API), garder ou choisir un code du foyer, puis **Activer**.
4. **Copier le lien pour un autre appareil** et l'ouvrir sur le second téléphone : il est configuré directement.

La clé et le code restent dans le navigateur, jamais dans ce dépôt. Toute personne qui a la clé et le code peut lire et modifier les menus : ne partager le lien qu'avec le foyer.
