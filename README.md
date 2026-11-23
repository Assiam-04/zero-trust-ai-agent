# Détection et isolement de mouvements latéraux (Zero Trust adaptatif)

Projet de Master – module SDN (Réseaux de nouvelle génération) – Sujet 4.

## Idée générale
Un agent IA (LangGraph) surveille les communications entre conteneurs, repère un comportement suspect (scan de ports, brute force SSH, rebond depuis un conteneur compromis) et applique dynamiquement une **micro-segmentation** pour isoler la machine concernée.

## Architecture
- **Réseau** : Mininet + Docker + Open vSwitch. Topologie à plusieurs zones (DMZ, serveurs internes, postes utilisateurs) et un conteneur « compromis » qui joue un scénario d'attaque (nmap, SSH, propagation).
- **Collecte et action** : programme eBPF (tracepoints, TC) qui remonte les connexions par conteneur et applique des règles via des maps eBPF mises à jour à chaud.
- **Intelligence** : graphe LangGraph : collecte → corrélation → classification (scan / brute force / mouvement latéral) → décision (surveiller / limiter / isoler) → vérification post-action. Un nœud « avocat du diable » valide ou rejette la décision avant exécution.

## Défi principal
Distinguer une activité anormale d'une activité légitime (sauvegardes, supervision, déploiements) pour ne pas isoler un service sain.

## Scénarios de test
Scan horizontal, brute force SSH, propagation en plusieurs rebonds, activité légitime ressemblant à une attaque.

## Exigences du prof
1. Baseline sans IA (règles fixes/seuils)
2. Métriques : temps de détection, temps de remédiation, taux de faux positifs, impact sur le trafic légitime, coût en tokens
3. Garde-fous : liste blanche d'actions, mode dry-run, validation humaine, journal d'audit
4. Banc d'essai reproductible (scripts Mininet/Docker + scénarios)
5. Ablation : avec/sans boucle de rétroaction, au moins 2 LLM (local Ollama + API)

## Structure du dépôt
```
docs/         état de l'art, architecture, rapport, glossaire
src/topology/ topologie Mininet + Docker
src/ebpf/     programmes C/eBPF (XDP/TC)
src/agent/    agent LangGraph (nœuds, outils, garde-fous)
scripts/      déploiement du banc d'essai, scénarios d'attaque
experiments/  campagnes d'expériences
results/      journaux et mesures brutes
tests/        tests
configs/      configuration (liste blanche, topologie)
```

## Installation et démo
À compléter (prérequis, installation, lancement en quelques commandes).

## Sécurité et éthique
- Attaques uniquement dans l'environnement émulé.
- Aucune clé d'API dans le dépôt (variables d'environnement / `.env` ignoré).
- Usage d'assistants IA : déclaré ici (à compléter) et compris par l'équipe.

## Équipe
(Noms et rôles de chaque membre)

## Planning (8 semaines)
- [ ] S1 : dépôt Git, topologie « hello world »
- [ ] S2 : topologie complète + scénarios reproductibles
- [ ] S3 : couche système eBPF + métriques
- [ ] S4 : baseline sans IA + résultats de référence
- [ ] S5 : agent LangGraph fonctionnel de bout en bout
- [ ] S6 : garde-fous + campagne d'expériences
- [ ] S7 : évaluation, ablation, rapport, présentation préliminaire
- [ ] S8 : finalisation, soutenance, démo
