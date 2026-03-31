# Cahier des charges — ESO Qualiopi

**Module d'auto-audit et de conformite continue**

| | |
|---|---|
| **Projet** | ESO Qualiopi |
| **Plateforme** | ESO (Epitech SalesOps) |
| **Destinataire** | Luc BOURRAT — Responsable Affaires Academiques |
| **Redacteur** | Sebastien ROTH — DGA Operations |
| **Version** | 1.0 |
| **Date** | 31 mars 2026 |
| **Statut** | Draft — En attente validation Luc |

---

## 1. Vision et objectifs

### 1.1 Contexte

Epitech est un organisme de formation certifie Qualiopi, operant sur **15 campus en France** avec un perimetre couvrant :
- **CFA** (Centre de Formation d'Apprentis)
- **Actions de formation**
- **VAE** (Validation des Acquis de l'Experience)

La certification Qualiopi repose sur le **Referentiel National Qualite (RNQ) Version 9** (8 janvier 2024), structure en **7 criteres** et **32 indicateurs**. Chaque campus est susceptible d'etre audite individuellement lors des cycles de surveillance.

Le pilotage national de la conformite repose aujourd'hui quasi-integralement sur **une seule personne** (Luc Bourrat), a l'aide de fichiers Excel disperses, de verifications manuelles et de coordination par email/Teams.

### 1.2 Problematique

| Probleme | Impact |
|---|---|
| Pilotage centralise sur une seule personne | Risque humain, surcharge, single point of failure |
| Pas de visibilite campus en temps reel | Les campus decouvrent leurs manques a J-7 de l'audit |
| Verification manuelle de coherence | Croiser nb apprenants / bulletins / evaluations prend des heures |
| Donnees dispersees dans 5+ systemes | EdSquare, IONIS Alternance, Gandalf, SharePoint, Excel |
| Preparation en mode "sprint de derniere minute" | Stress, erreurs, risque de non-conformite |

### 1.3 Proposition de valeur

**Passer d'un suivi ponctuel pre-audit a un systeme de conformite continue.**

L'outil ESO Qualiopi doit :
- **Decentraliser l'execution** vers les campus (chaque campus gere ses indicateurs locaux)
- **Centraliser la visibilite** pour le pilote national (heatmap, alertes, scores)
- **Automatiser la verification** en allant chercher les donnees dans EdSquare et IONIS Alternance
- **Eliminer le travail a faible valeur** (compilation manuelle, relances, generation de dossiers)

**Objectif mesurable :** le jour de l'audit, le dossier de preuves d'un campus est generable en **un clic**, et la preparation passe de **plusieurs semaines de travail manuel** a un **suivi continu sans effort supplementaire**.

---

## 2. Perimetre fonctionnel

### 2.1 Les 32 indicateurs (7 criteres)

L'outil couvre l'integralite des 32 indicateurs du RNQ V9, organises par critere :

| Critere | Intitule | Indicateurs |
|---|---|---|
| C1 | Information du public | I01 — I03 |
| C2 | Objectifs et adaptation des prestations | I04 — I08 |
| C3 | Adaptation aux beneficiaires | I09 — I16 |
| C4 | Moyens pedagogiques et techniques | I17 — I20 |
| C5 | Qualification et developpement des competences des personnels | I21 — I22 |
| C6 | Investissement dans son environnement professionnel | I23 — I29 |
| C7 | Recueil et prise en compte des appreciations et reclamations | I30 — I32 |

**Specificites par categorie :** Chaque indicateur est tague selon son applicabilite (OF, CFA, VAE, CBC, Certifiantes). Les specificites CFA et les indicateurs "Nouveaux entrants" (audites en surveillance) sont clairement identifies.

### 2.2 Modele composite NATIONAL / LOCAL

Chaque indicateur est decompose en **sous-items de preuves**. Chaque sous-item porte un scope independant :

| Scope | Definition | Qui le voit | Qui le gere |
|---|---|---|---|
| `NATIONAL` | Preuve fournie par la fonction centrale | Luc uniquement | Luc / equipe centrale |
| `LOCAL` | Preuve produite par chaque campus | Le campus concerne + Luc | Directeur campus / Resp. pedago |
| `BOTH` | Composante nationale ET locale | Tous | Selon le sous-item |

**Regle de visibilite :** Un utilisateur campus ne voit **que les sous-items LOCAL** de ses indicateurs. Il ne voit jamais les sous-items NATIONAL (geres par Luc). Luc voit tout, tous campus.

**Exemple concret — I13 (Coordination apprentis / Suivi entreprise) :**

| Sous-item | Scope | Source |
|---|---|---|
| Grille d'evaluation tuteur liee aux competences RNCP | NATIONAL | Manuel |
| Processus de suivi entreprise formalise | NATIONAL | Manuel |
| Fichier de suivi entreprise rempli (par apprenant) | LOCAL | IONIS Alternance |
| Evaluations tuteur completees | LOCAL | EdSquare |
| Visites entreprise realisees | LOCAL | IONIS Alternance |

### 2.3 Les 14 missions CFA (Article L6231-2)

L'outil integre le suivi des 14 missions obligatoires des CFA (Code du travail, art. L6231-2), mappees aux indicateurs Qualiopi :

| # | Mission | Indicateur |
|---|---|---|
| 1 | Accompagnement handicap + referent PSH | I26 |
| 2 | Appui recherche employeur | I14 |
| 3 | Coherence CFA/entreprise | I13 |
| 4 | Information droits et devoirs | I15 |
| 5 | Poursuite formation en cas de rupture | I14 |
| 6 | Accompagnement social, missions locales | I14 |
| 7 | Mixite, prevention harcelement | I14 |
| 8 | Actions mixite metiers | I14 |
| 9 | Diversite, lutte discriminations | I14 |
| 10 | Mobilite nationale/internationale | I14 |
| 11 | Suivi formation a distance | I11 |
| 12 | Evaluation competences | I13 |
| 13 | Accompagnement post-formation sans diplome | I14 |
| 14 | Acces aux aides | I14 |

Chaque mission est tracee dans l'outil avec un lien vers les preuves de l'indicateur associe.

### 2.4 Systeme de scoring

**Score par indicateur par campus :** 4 niveaux calcules automatiquement.

| Niveau | Couleur | Condition |
|---|---|---|
| Conforme | 🟢 Vert | 100% des sous-items LOCAL + NATIONAL valides |
| Observation | 🟡 Jaune | ≥ 75% des sous-items valides, reste en cours |
| NC Mineure | 🟠 Orange | ≥ 50% des sous-items valides, manques identifies |
| NC Majeure | 🔴 Rouge | < 50% ou sous-item critique manquant |

**Score Readiness campus :** Moyenne ponderee des 32 indicateurs, exprimee en pourcentage (0-100%). Les indicateurs a risque historiquement eleve (I02, I13, I14, I20, I27) ont un coefficient de ponderation superieur.

### 2.5 Checklist de preuves

Chaque sous-item de preuve suit un cycle de statuts :

```
NOT_STARTED → IN_PROGRESS → PROVIDED → VALIDATED
                                    ↘ REJECTED → IN_PROGRESS (boucle)
```

| Statut | Signification | Qui le change |
|---|---|---|
| `NOT_STARTED` | Rien n'a ete fait | Etat initial |
| `IN_PROGRESS` | Travail en cours | Campus (local) ou Luc (national) |
| `PROVIDED` | Preuve fournie, en attente de validation | Campus uploade / API synchronise |
| `VALIDATED` | Preuve validee par le pilote national | Luc uniquement |
| `REJECTED` | Preuve insuffisante, a reprendre | Luc (avec commentaire obligatoire) |

**Preuves automatiques :** Les sous-items connectes a EdSquare ou IONIS Alternance passent automatiquement a `PROVIDED` lorsque les donnees sont presentes et coherentes. Luc n'a plus qu'a valider.

### 2.6 Verification de coherence numerique

Le systeme croise automatiquement les donnees des API pour detecter les incoherences :

| Verification | Sources | Alerte si |
|---|---|---|
| Nb apprenants vs nb bulletins | Gandalf / IONIS Alt. | Ecart > 5% |
| Nb apprentis vs nb evaluations tuteur | IONIS Alt. / EdSquare | Evaluations manquantes |
| Nb contrats vs nb suivis entreprise | IONIS Alt. | Suivis non renseignes |
| Nb intervenants externes vs nb CV/contrats | Manuel | CV ou contrat absent |
| Nb apprenants PSH vs nb amenagements | Manuel / EdSquare | Amenagement non documente |

**Format de l'alerte :** "Campus Lyon — I13 : 92 apprentis, 89 evaluations tuteur completees. **3 evaluations manquantes.** [Voir le detail →]"

### 2.7 Mode Audit Blanc (simulation)

Un campus ou le pilote national peut lancer une **simulation d'audit** sur un campus donne :

- Le systeme parcourt les 32 indicateurs (ou uniquement les LOCAL pour le campus)
- Pour chaque indicateur : verification des preuves, coherence numerique, completude
- Pose les **questions types de l'auditeur** (base de donnees de questions par indicateur)
- Genere un **rapport de simulation** avec :
  - Indicateurs prets (vert)
  - Indicateurs a risque (orange/rouge) avec actions correctives
  - Score Readiness simule
  - Temps estime pour atteindre 100%

### 2.8 Generation du dossier de preuves

**Fonctionnalite cle pour Luc.** En un clic, l'outil genere un dossier PDF structure par indicateur pour un campus donne :

- Page de garde (campus, date, perimetre)
- Pour chaque indicateur audite :
  - Description de l'indicateur (RNQ V9)
  - Liste des preuves avec statut
  - Donnees quantitatives (issues des API)
  - Documents joints (PDF, images)
  - Tableau de coherence numerique
- Synthese globale avec scoring

**Format :** PDF professionnel, utilisable tel quel face a l'auditeur.

### 2.9 Timeline de preparation

Le systeme propose un calendrier de preparation structure :

| Jalon | Delai | Actions |
|---|---|---|
| J-30 | 30 jours avant audit | Lancement audit blanc, identification des manques majeurs |
| J-14 | 14 jours | Toutes les preuves documentaires doivent etre uploadees |
| J-7 | 7 jours | Verification de coherence numerique, relances automatiques |
| J-3 | 3 jours | Gel du dossier, generation du dossier de preuves |
| J-1 | Veille | Briefing final, check-list derniere minute |
| Jour J | Audit | Consultation du dossier, export en temps reel si besoin |

Chaque jalon declenche des **notifications automatiques** aux personnes concernees.

### 2.10 Notifications Teams

Integration Microsoft Teams (via Azure AD, deja en place sur ESO) :

| Declencheur | Destinataire | Message type |
|---|---|---|
| Sous-item passe en rouge | Directeur campus | "I13 — 44 evaluations manquantes. Score : 64%" |
| Jalon J-30 / J-7 / J-3 | Campus audite + Luc | "Audit dans X jours. Score Readiness : Y%. Z actions restantes." |
| Sous-item rejete par Luc | Campus | "I27 — CV intervenant rejete. Motif : [commentaire]. A reprendre." |
| Score campus < 50% | Luc | "Alerte — Campus Lyon sous le seuil critique (48%)" |
| Rapport hebdomadaire | Luc | Synthese multi-campus chaque lundi matin |

---

## 3. Parcours utilisateurs

### 3.1 Luc — Pilote national

**Contexte :** Luc ouvre ESO Qualiopi le lundi matin. Il a 15 campus a superviser.

**Ecran principal :** Heatmap multi-campus.

```
           C1    C2    C3    C4    C5    C6    C7    Score
Paris      🟢    🟢    🟡    🟢    🟢    🟢    🟢    94%
Lyon       🟢    🟢    🔴    🟢    🟡    🟠    🟢    72%
Nantes     🟢    🟢    🟢    🟢    🟢    🟡    🟢    89%
Strasbourg 🟢    🟡    🟡    🟢    🟢    🟢    🟢    85%
...        ...   ...   ...   ...   ...   ...   ...   ...
```

**Actions :**
1. Identifie les zones rouges/oranges en un coup d'oeil
2. Clique sur une cellule → voit les indicateurs concernes et les sous-items en retard
3. Envoie une notification Teams au campus en un clic
4. Valide ou rejette les preuves soumises par les campus
5. Lance un audit blanc sur un campus
6. Genere un dossier de preuves PDF

**Temps passe :** 10-15 min/jour au lieu de plusieurs heures.

### 3.2 Directeur de campus — Vue locale

**Contexte :** Le directeur du campus Lyon recoit une notification Teams : "3 actions en retard — Score : 64%".

**Ecran principal :** Dashboard simplifie, actions urgentes en premier.

```
┌─────────────────────────────────────────┐
│  Score Readiness : 64%                  │
│  🟢 12   🟡 3   🟠 1   🔴 1           │
├─────────────────────────────────────────┤
│  ⚠️ ACTIONS URGENTES                    │
│                                         │
│  🔴 I13 — 44 evaluations tuteur         │
│          manquantes sur EdSquare         │
│          [Voir detail →]                │
│                                         │
│  🟠 I27 — 2 CV intervenants externes    │
│          a uploader                     │
│          [Uploader →]                   │
│                                         │
│  ✅ INDICATEURS OK (12)                 │
│  I04 ✓  I10 ✓  I11 ✓  I16 ✓  ...      │
└─────────────────────────────────────────┘
```

**Regles UX :**
- Le campus ne voit QUE ses indicateurs LOCAL
- Les indicateurs verts sont replies par defaut
- Les actions automatiques (EdSquare/IONIS) n'apparaissent que si un probleme est detecte
- Le campus n'intervient manuellement que pour les uploads documentaires

### 3.3 Preparation audit (J-7)

**Contexte :** Luc lance le mode Audit Blanc sur le campus qui sera audite.

**Deroulement :**
1. Selection du campus et du perimetre (tous indicateurs ou selection)
2. Le systeme verifie chaque indicateur :
   - Preuves presentes et validees ?
   - Donnees numeriques coherentes ?
   - Documents a jour ?
3. Pour chaque indicateur, affichage des **questions types de l'auditeur**
4. Generation du rapport de simulation :
   - Indicateurs prets / a risque
   - Actions correctives restantes avec delai estime
   - Score Readiness projete

### 3.4 Jour J — Export dossier preuves

**Contexte :** L'auditeur demande les preuves de I13 pour le campus Strasbourg.

**Actions Luc :**
1. Clic sur "Exporter dossier" → choix campus + indicateur(s)
2. Le systeme compile automatiquement :
   - Donnees EdSquare (evaluations tuteur)
   - Donnees IONIS Alternance (contrats, suivi)
   - Documents uploades par le campus
   - Tableau de coherence numerique
3. Generation PDF instantanee
4. Presentation a l'auditeur

---

## 4. Architecture technique

### 4.1 Integration dans ESO

ESO Qualiopi est une **nouvelle application** dans le monorepo ESO, basee sur `@eso/core` :

```
ESO_Project/
  apps/
    qualiopi/                    → Nouvelle app, port 3008
      src/
        app/
          layout.tsx             → Via @eso/core root-layout
          (auth)/
            dashboard/
              page.tsx           → Dashboard (national ou campus selon role)
            indicateurs/
              page.tsx           → Liste des 32 indicateurs
              [id]/
                page.tsx         → Detail indicateur + sous-items
            audit-blanc/
              page.tsx           → Lancement simulation
              [campusId]/
                page.tsx         → Resultat simulation campus
            campus/
              [campusId]/
                page.tsx         → Vue detaillee d'un campus
            export/
              page.tsx           → Generation dossier preuves
          api/
            sync/
              edsquare/
                route.ts         → Endpoint sync EdSquare
              ionis-alternance/
                route.ts         → Endpoint sync IONIS Alternance
            readiness/
              route.ts           → Calcul score readiness
              [campusId]/
                route.ts         → Score d'un campus specifique
            audit-blanc/
              route.ts           → Execution simulation
            export/
              route.ts           → Generation PDF
            notifications/
              route.ts           → Envoi notifications Teams
            cron/
              sync/
                route.ts         → Cron sync quotidien
              alerts/
                route.ts         → Cron alertes hebdomadaires
      package.json
      next.config.ts             → Via @eso/core
      middleware.ts              → Via @eso/core (auth)

  packages/
    qualiopi-connectors/         → Nouveau package partage
      src/
        adapters/
          edsquare.ts            → Adapter API EdSquare
          ionis-alternance.ts    → Adapter API IONIS Alternance
          gandalf.ts             → Adapter Gandalf (futur)
          manual.ts              → Fallback saisie manuelle
        types/
          evidence.ts            → Interface unifiee EvidenceItem
          indicator.ts           → Types indicateurs
          campus.ts              → Types campus
        sync/
          scheduler.ts           → Orchestrateur de synchronisation
          reconciliation.ts      → Verification de coherence
        scoring/
          readiness.ts           → Calcul Score Readiness
          weights.ts             → Coefficients de ponderation
      package.json
```

### 4.2 Stack technique

| Composant | Technologie | Version |
|---|---|---|
| Framework | Next.js (App Router, Server Components) | 16.x |
| Langage | TypeScript | 5.9 |
| ORM | Prisma | 7.5 |
| Base de donnees | PostgreSQL | 16 |
| Auth | NextAuth v5 + Microsoft Entra ID | SSO cross-subdomain |
| UI | Tailwind CSS v4 + shadcn/ui | Latest |
| Tests | Vitest | 4.x |
| Monorepo | Turborepo + pnpm workspaces | - |
| Infra | Docker, Traefik, Helm/K8s | - |
| Monitoring | Sentry + Loki/Grafana | - |
| Notifications | Microsoft Graph API (Teams) | - |
| PDF | @react-pdf/renderer ou puppeteer | - |

### 4.3 Pattern Adapter (connecteurs API)

Le systeme utilise un **pattern Adapter** pour isoler l'application des sources de donnees externes :

```typescript
// packages/qualiopi-connectors/src/types/evidence.ts

interface EvidenceData {
  sourceId: string
  source: DataSource           // EDSQUARE | IONIS_ALT | GANDALF | MANUAL
  indicatorId: string          // "I01" a "I32"
  campusId: string
  label: string
  value: EvidenceValue | null  // Donnees quantitatives
  documents: DocumentRef[]     // Pieces jointes
  lastSyncAt: Date
}

interface EvidenceValue {
  expected: number             // Ex: 92 apprentis
  actual: number               // Ex: 89 evaluations
  unit: string                 // Ex: "evaluations tuteur"
  isCoherent: boolean          // actual/expected >= seuil
}

// Chaque adapter implemente cette interface
interface EvidenceAdapter {
  name: DataSource
  sync(campusId: string): Promise<EvidenceData[]>
  healthCheck(): Promise<boolean>
}
```

**Avantage :** Si un endpoint API n'est pas disponible, l'adapter bascule sur le mode `MANUAL` pour ce sous-item. L'application ne sait jamais d'ou viennent les donnees.

### 4.4 RBAC

Integration avec le systeme RBAC existant de `@eso/shared` :

| Role | Scope | Permissions |
|---|---|---|
| `qualiopi:admin` | National | Tout voir, tout editer, valider/rejeter, exporter, configurer |
| `qualiopi:pilot` | National | Tout voir, valider/rejeter, exporter, envoyer notifications |
| `qualiopi:campus_director` | Campus | Voir indicateurs LOCAL de son campus, uploader preuves |
| `qualiopi:campus_staff` | Campus | Voir indicateurs LOCAL de son campus (lecture seule) |

Le mapping des roles se fait via les groupes Azure AD existants, enrichis avec les scopes Qualiopi.

---

## 5. Integrations API

### 5.1 EdSquare

**Objectif :** Recuperer automatiquement les evaluations tuteur, enquetes de satisfaction, et suivi pedagogique par apprenant.

**Donnees necessaires :**

| Donnee | Endpoint attendu | Indicateur | Priorite |
|---|---|---|---|
| Evaluations tuteur par apprenant | GET /api/evaluations/tuteur | I13 | P0 |
| Lien evaluation ↔ competences RNCP | GET /api/evaluations/tuteur?include=rncp | I13 | P1 (evolution) |
| Enquetes de satisfaction apprenants | GET /api/surveys/satisfaction | I30 | P0 |
| Enquetes de satisfaction entreprises | GET /api/surveys/entreprises | I30 | P0 |
| Suivi pedagogique individuel | GET /api/students/{id}/progress | I11 | P1 |
| Resultats evaluations competences | GET /api/evaluations/competences | I13 | P0 |

**Filtrage :** Toutes les requetes sont filtrees par `campus_id` et `academic_year`.

**Frequence de synchronisation :** Quotidienne (cron a 6h).

### 5.2 IONIS Alternance

**Objectif :** Recuperer automatiquement le suivi des contrats d'alternance, missions, et suivi individuel apprenant/tuteur.

**Donnees necessaires :**

| Donnee | Endpoint attendu | Indicateur | Priorite |
|---|---|---|---|
| Liste des contrats actifs | GET /api/contracts | I13 | P0 |
| Detail contrat (entreprise, tuteur, dates) | GET /api/contracts/{id} | I13, I27 | P0 |
| Suivi des missions par apprenant | GET /api/missions/{studentId} | I13 | P0 |
| Visites entreprise (dates, CR) | GET /api/visits | I13 | P0 |
| Progression par bloc de competences | GET /api/students/{id}/blocks | I13 | P1 (evolution) |
| Intervenants externes (profils) | GET /api/trainers/external | I27 | P1 |
| Ruptures de contrat | GET /api/contracts/terminated | I14 | P0 |

**Filtrage :** Par `campus_id` et `contract_year`.

**Frequence de synchronisation :** Quotidienne (cron a 6h).

### 5.3 Cartographie des gaps

| Donnee necessaire | API source | Disponible | Action si absent |
|---|---|---|---|
| Evaluations tuteur (note, date, apprenant) | EdSquare | ✅ Oui | - |
| Mapping evaluation ↔ competences RNCP | EdSquare | ⚠️ A verifier | Demander evolution endpoint |
| Enquetes satisfaction | EdSquare | ✅ Oui | - |
| Contrats alternance | IONIS Alt. | ✅ Oui | - |
| Suivi missions | IONIS Alt. | ✅ Oui | - |
| Visites entreprise | IONIS Alt. | ⚠️ A verifier | Demander evolution endpoint |
| Progression par bloc RNCP | IONIS Alt. | ⚠️ A verifier | Demander evolution endpoint |
| Intervenants externes avec CV | IONIS Alt. | ❌ Probable non | Saisie manuelle Phase 1 |

### 5.4 Strategie d'integration

**Niveau 1 — On demarre avec ce qui existe (Phase 2)**
- Connecter tous les endpoints disponibles
- Le score Readiness est deja plus fiable que le declaratif humain
- Les sous-items sans endpoint restent en mode MANUAL

**Niveau 2 — On demande des evolutions ciblees (Phase 3+)**
- Une fois le MVP en production avec 15 campus connectes, on dispose de donnees concretes
- On presente aux editeurs les gaps identifies avec des volumes reels
- Negociation d'evolutions sur la base de l'usage, pas de la theorie

---

## 6. Modele de donnees

### 6.1 Schema Prisma

```prisma
// packages/db/prisma/schema.prisma (ajouts au schema ESO existant)

// ===== QUALIOPI =====

model QualiopiCriterion {
  id          Int                @id          // 1 a 7
  title       String
  indicators  QualiopiIndicator[]
}

model QualiopiIndicator {
  id            String             @id          // "I01" a "I32"
  criterionId   Int
  criterion     QualiopiCriterion  @relation(fields: [criterionId], references: [id])
  title         String
  description   String             @db.Text     // Description RNQ V9
  applicability String[]           // ["OF", "CFA", "VAE", "CBC", "CERT"]
  isNewEntrant  Boolean            @default(false) // Audite en surveillance
  weightFactor  Float              @default(1.0)   // Coefficient ponderation score
  items         QualiopiEvidenceItem[]
  auditQuestions QualiopiAuditQuestion[]
  cfaMissions   QualiopiCfaMission[]
}

model QualiopiEvidenceItem {
  id            String             @id @default(cuid())
  indicatorId   String
  indicator     QualiopiIndicator  @relation(fields: [indicatorId], references: [id])
  label         String
  description   String?
  scope         QualiopiScope
  dataSource    QualiopiDataSource
  isCritical    Boolean            @default(false) // Sous-item bloquant
  sortOrder     Int                @default(0)

  // Donnees par campus
  campusData    QualiopiEvidenceCampus[]

  createdAt     DateTime           @default(now())
  updatedAt     DateTime           @updatedAt
}

model QualiopiEvidenceCampus {
  id              String               @id @default(cuid())
  evidenceItemId  String
  evidenceItem    QualiopiEvidenceItem  @relation(fields: [evidenceItemId], references: [id])
  campusId        String
  campus          Campus               @relation(fields: [campusId], references: [id])

  status          QualiopiEvidenceStatus @default(NOT_STARTED)
  autoValue       Json?                // {expected: 92, actual: 89, unit: "evaluations"}
  comment         String?              @db.Text
  rejectionReason String?              @db.Text
  validatedBy     String?              // userId de Luc
  validatedAt     DateTime?

  documents       QualiopiDocument[]

  lastSyncAt      DateTime?
  createdAt       DateTime             @default(now())
  updatedAt       DateTime             @updatedAt

  @@unique([evidenceItemId, campusId])
  @@index([campusId, status])
}

model QualiopiDocument {
  id                String                  @id @default(cuid())
  evidenceCampusId  String
  evidenceCampus    QualiopiEvidenceCampus   @relation(fields: [evidenceCampusId], references: [id])
  filename          String
  fileUrl           String
  fileSize          Int
  mimeType          String
  uploadedBy        String                  // userId
  uploadedAt        DateTime                @default(now())
}

model QualiopiAuditQuestion {
  id            String              @id @default(cuid())
  indicatorId   String
  indicator     QualiopiIndicator   @relation(fields: [indicatorId], references: [id])
  question      String              @db.Text
  expectedAnswer String?            @db.Text
  sortOrder     Int                 @default(0)
}

model QualiopiCfaMission {
  id            String              @id @default(cuid())
  number        Int                 // 1 a 14
  title         String
  indicatorId   String
  indicator     QualiopiIndicator   @relation(fields: [indicatorId], references: [id])
}

model QualiopiAuditSession {
  id          String             @id @default(cuid())
  campusId    String
  campus      Campus             @relation(fields: [campusId], references: [id])
  type        QualiopiAuditType  // BLANC | REEL
  auditDate   DateTime?
  status      QualiopiAuditSessionStatus @default(PLANNED)
  reportUrl   String?
  score       Float?
  createdBy   String             // userId
  createdAt   DateTime           @default(now())
  updatedAt   DateTime           @updatedAt
}

model QualiopiSyncLog {
  id          String             @id @default(cuid())
  source      QualiopiDataSource
  campusId    String?
  status      QualiopiSyncStatus
  itemsFound  Int                @default(0)
  itemsSync   Int                @default(0)
  errors      Json?
  startedAt   DateTime           @default(now())
  completedAt DateTime?
}

// ===== ENUMS =====

enum QualiopiScope {
  NATIONAL
  LOCAL
  BOTH
}

enum QualiopiDataSource {
  EDSQUARE
  IONIS_ALT
  GANDALF
  MANUAL
}

enum QualiopiEvidenceStatus {
  NOT_STARTED
  IN_PROGRESS
  PROVIDED
  VALIDATED
  REJECTED
}

enum QualiopiAuditType {
  BLANC
  REEL
}

enum QualiopiAuditSessionStatus {
  PLANNED
  IN_PROGRESS
  COMPLETED
}

enum QualiopiSyncStatus {
  RUNNING
  SUCCESS
  PARTIAL
  FAILED
}
```

### 6.2 Seed data

Les 32 indicateurs, leurs sous-items, les 14 missions CFA et les questions types de l'auditeur seront charges via un **script de seed** (`prisma/seed-qualiopi.ts`) base sur les donnees du referentiel Epiliopi existant.

---

## 7. User stories prioritaires

### Phase 1 — Fondations

| ID | User Story | Priorite |
|---|---|---|
| QP-01 | En tant que Luc, je veux voir un heatmap multi-campus x criteres pour identifier les zones a risque en un coup d'oeil | P0 |
| QP-02 | En tant que directeur campus, je veux voir uniquement mes indicateurs LOCAL avec les actions urgentes en premier | P0 |
| QP-03 | En tant que Luc, je veux pouvoir valider ou rejeter une preuve soumise par un campus | P0 |
| QP-04 | En tant que directeur campus, je veux uploader un document comme preuve pour un sous-item | P0 |
| QP-05 | En tant que Luc, je veux voir le Score Readiness de chaque campus | P0 |
| QP-06 | En tant que directeur campus, je veux voir mon Score Readiness et sa progression | P1 |
| QP-07 | En tant que Luc, je veux filtrer les indicateurs par critere, scope et statut | P1 |
| QP-08 | En tant que directeur campus, je veux voir les 14 missions CFA et leur couverture | P1 |

### Phase 2 — Intelligence API

| ID | User Story | Priorite |
|---|---|---|
| QP-09 | En tant que systeme, je synchronise quotidiennement les evaluations tuteur depuis EdSquare | P0 |
| QP-10 | En tant que systeme, je synchronise quotidiennement les contrats et suivis depuis IONIS Alternance | P0 |
| QP-11 | En tant que Luc, je veux voir les incoherences numeriques detectees automatiquement | P0 |
| QP-12 | En tant que directeur campus, je veux que mes indicateurs connectes a EdSquare/IONIS se mettent a jour automatiquement | P0 |
| QP-13 | En tant que Luc, je veux recevoir une alerte quand un campus passe sous le seuil critique (50%) | P1 |
| QP-14 | En tant que systeme, je bascule en mode MANUAL si un endpoint API est indisponible | P1 |

### Phase 3 — Acceleration

| ID | User Story | Priorite |
|---|---|---|
| QP-15 | En tant que Luc, je veux lancer un audit blanc sur un campus et obtenir un rapport de simulation | P0 |
| QP-16 | En tant que Luc, je veux generer un dossier de preuves PDF pour un campus/indicateur en un clic | P0 |
| QP-17 | En tant que systeme, j'envoie des notifications Teams aux campus selon les jalons de preparation | P1 |
| QP-18 | En tant que directeur campus, je veux voir la timeline de preparation (J-30 a Jour-J) | P1 |
| QP-19 | En tant que Luc, je veux voir l'historique de progression du Score Readiness par campus | P2 |
| QP-20 | En tant que Luc, je veux comparer les scores de tous les campus sur un classement | P2 |

---

## 8. Contraintes et risques

### 8.1 Risques identifies

| # | Risque | Probabilite | Impact | Mitigation |
|---|---|---|---|---|
| R1 | **Adoption campus faible** — Les directeurs ne remplissent pas l'outil | Elevee | Critique | UX ultra-simple, valeur immediate visible (score), notifications push |
| R2 | **Donnees API incompletes** — Endpoints manquants pour certains sous-items | Moyenne | Moyen | Pattern Adapter avec fallback MANUAL, strategie Niveau 1/2 |
| R3 | **Surcharge Luc pendant la construction** — Luc doit valider le CDC + tester + gerer les audits courants | Elevee | Moyen | Phases courtes, livraisons incrementales, beta-test sur 2 campus d'abord |
| R4 | **Qualite des donnees sources** — Donnees EdSquare/IONIS incorrectes ou desynchronisees | Moyenne | Eleve | Logs de sync detailles, alertes sur incoherences, mode audit des donnees |
| R5 | **Indisponibilite API externe** — EdSquare ou IONIS en maintenance | Faible | Moyen | Circuit breaker, fallback MANUAL, retry avec backoff |
| R6 | **Evolution du referentiel** — Le RNQ passe en V10 | Faible | Moyen | Modele de donnees flexible (seed editable), indicateurs configurables |

### 8.2 RGPD

L'outil manipule des **donnees personnelles d'apprenants** provenant de multiples systemes :

| Point | Traitement |
|---|---|
| **Base legale** | Interet legitime de l'organisme de formation (obligation legale Qualiopi) |
| **Donnees traitees** | Nom, prenom, evaluations, contrats, suivi pedagogique |
| **Duree de conservation** | Alignee sur les obligations Qualiopi : duree du cycle de certification + 1 an |
| **Acces** | Limite aux roles RBAC (pilote national + campus concerne) |
| **Sous-traitance** | Les donnees transitent par les API EdSquare/IONIS deja soumises au RGPD |
| **Registre de traitement** | A ajouter au registre RGPD ESO existant (deja valide) |
| **DPO** | Le DPO IONIS doit etre informe de ce nouveau traitement |

### 8.3 Contraintes techniques

| Contrainte | Detail |
|---|---|
| Coherence avec ESO | L'app DOIT utiliser @eso/core, @eso/auth, @eso/shared, @eso/ui, @eso/db |
| SSO | Authentification via le SSO Microsoft Entra ID existant |
| Performance | Le heatmap doit charger en < 2s pour 15 campus x 32 indicateurs |
| Disponibilite | L'app doit fonctionner meme si les API externes sont indisponibles (mode degrade) |
| Mobile | Dashboard consultable sur mobile (responsive, pas d'app native) |

---

## 9. Planning previsionnel

### Phase 1 — Fondations (8 semaines)

| Semaine | Livrable |
|---|---|
| S1-S2 | Setup app `qualiopi`, schema Prisma, seed des 32 indicateurs + sous-items |
| S3-S4 | RBAC (national/local), dashboard campus (LOCAL) |
| S5-S6 | Dashboard national (heatmap), scoring, validation/rejet preuves |
| S7 | Upload documents, filtres, missions CFA |
| S8 | Tests, corrections, deploiement beta (2 campus pilotes) |

**Livrable Phase 1 :** Application fonctionnelle avec gestion manuelle des preuves, visible par les campus pilotes et Luc.

### Phase 2 — Intelligence API (6 semaines)

| Semaine | Livrable |
|---|---|
| S9-S10 | Package `qualiopi-connectors`, adapter EdSquare, sync evaluations |
| S11-S12 | Adapter IONIS Alternance, sync contrats/suivis |
| S13 | Verification de coherence numerique, Score Readiness automatique |
| S14 | Circuit breaker, fallback MANUAL, logs de sync, deploiement 15 campus |

**Livrable Phase 2 :** Synchronisation automatique avec EdSquare et IONIS Alternance, scoring automatise, deploiement tous campus.

### Phase 3 — Acceleration (4 semaines)

| Semaine | Livrable |
|---|---|
| S15-S16 | Mode Audit Blanc (simulation + rapport) |
| S17 | Generation PDF dossier de preuves, notifications Teams |
| S18 | Timeline J-30 a Jour-J, historique progression, finalisation |

**Livrable Phase 3 :** Outil complet avec simulation d'audit, export PDF et notifications.

### Total : 18 semaines (~4,5 mois)

---

## Annexes

### A. Glossaire

| Terme | Definition |
|---|---|
| **RNQ** | Referentiel National Qualite |
| **V9** | Version 9 du RNQ (8 janvier 2024) |
| **CFA** | Centre de Formation d'Apprentis |
| **VAE** | Validation des Acquis de l'Experience |
| **OF** | Organisme de Formation |
| **NC** | Non-Conformite |
| **PSH** | Personne en Situation de Handicap |
| **RNCP** | Repertoire National des Certifications Professionnelles |
| **Score Readiness** | Pourcentage de preparation d'un campus (0-100%) |
| **Audit Blanc** | Simulation d'audit sans consequence reelle |

### B. References

- [Referentiel National Qualite V9](https://travail-emploi.gouv.fr/IMG/pdf/guide_de_lecture_qualiopi_v9_du_8_janvier_2024.pdf)
- [Article L6231-2 du Code du travail](https://www.legifrance.gouv.fr/codes/article_lc/LEGIARTI000037386088)
- Dashboard Epiliopi existant : https://sebastienroth.github.io/Epiliopi/
- Fiches indicateurs : https://sebastienroth.github.io/Epiliopi/fiches_indicateurs/

---

*Document genere le 31 mars 2026 — Session collaborative BMAD Party Mode (Innovation Strategist, Creative Problem Solver, Design Thinking Coach, Brainstorming Coach, Architect, Product Manager)*
