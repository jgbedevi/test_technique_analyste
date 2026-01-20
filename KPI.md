---


---

<h2 id="kpi-proposés-pour-l’aide-à-la-décision">KPI Proposés pour l’Aide à la Décision</h2>
<p>Ce tableau présente les KPI retenus comme les plus utiles pour la prise de décision dans le cadre de l’amélioration de la performance du service public numérique.</p>

<table>
<thead>
<tr>
<th><strong>Nom du KPI</strong></th>
<th><strong>Objectif métier</strong></th>
<th><strong>Description / Interprétation</strong></th>
<th><strong>Règle de calcul</strong></th>
<th><strong>Requête SQL (Exemple)</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>Taux de Saturation des Centres</strong> <em>(Performance)</em></td>
<td>Optimiser la capacité des centres et répartir les ressources</td>
<td>Mesure si le volume de demandes dépasse la capacité estimée. Un taux &gt; 100% indique une surcharge et un besoin de renfort</td>
<td><code>taux_saturation = (demandes_traitees / capacite_theorique) * 100</code></td>
<td><code>SELECT c.centre_id, (COUNT(d.demande_id)::float / c.personnel_capacite_jour) * 100 AS taux_saturation FROM demandes d JOIN centres c ON d.centre_id = c.centre_id GROUP BY c.centre_id, c.personnel_capacite_jour;</code></td>
</tr>
<tr>
<td><strong>Délai Moyen de Traitement</strong> <em>(Performance)</em></td>
<td>Réduire les délais d’attente et améliorer la satisfaction usager</td>
<td>Temps moyen nécessaire pour traiter une demande. Un délai élevé révèle des lenteurs administratives</td>
<td><code>delai_moyen = moyenne(date_fin - date_debut)</code></td>
<td><code>SELECT AVG(EXTRACT(DAY FROM (date_operation_fin - date_operation_debut))) AS delai_moyen FROM logs_activites;</code></td>
</tr>
<tr>
<td><strong>Taux de Couverture Territoriale</strong> <em>(Accessibilité)</em></td>
<td>Assurer une accessibilité territoriale équitable</td>
<td>Mesure la proportion de communes disposant d’au moins un centre</td>
<td><code>taux_couverture = (communes_couvertes / communes_totales) * 100</code></td>
<td><code>SELECT (COUNT(DISTINCT commune)::float / (SELECT COUNT(*) FROM communes)) * 100 AS taux_couverture FROM centres;</code></td>
</tr>
<tr>
<td><strong>Taux de Pénétration Administrative</strong> <em>(Accessibilité)</em></td>
<td>Identifier le niveau d’usage des services par population</td>
<td>Nombre de demandes rapporté à 1000 habitants. Un faible taux suggère un manque d’information ou d’accès</td>
<td><code>taux_penetration = (demandes_totales / population) * 1000</code></td>
<td><code>SELECT d.commune, (SUM(d.nombre_demandes)::float / c.population) * 1000 AS taux_penetration FROM demandes d JOIN communes c ON d.commune = c.commune GROUP BY d.commune, c.population;</code></td>
</tr>
<tr>
<td><strong>Taux de Premier Dépôt Conforme</strong> <em>(Qualité)</em></td>
<td>Réduire le retraitement et améliorer l’efficacité du front-office</td>
<td>Proportion de demandes acceptées du premier coup. Un taux faible = usagers mal informés</td>
<td><code>taux_conformite = (demandes_valides / demandes_totales) * 100</code></td>
<td><code>SELECT (COUNT(CASE WHEN statut != 'Rejeté' THEN 1 END)::float / COUNT(*)) * 100 AS taux_conformite FROM demandes;</code></td>
</tr>
<tr>
<td><strong>Charge par Agent</strong> <em>(Efficience)</em></td>
<td>Équilibrer la charge de travail et prévenir l’épuisement</td>
<td>Nombre moyen de dossiers traités par agent</td>
<td><code>charge_par_agent = demandes_traitees / nombre_guichets</code></td>
<td><code>SELECT c.centre_id, COUNT(d.demande_id)::float / c.nombre_guichets AS charge_par_agent FROM demandes d JOIN centres c ON d.centre_id = c.centre_id GROUP BY c.centre_id, c.nombre_guichets;</code></td>
</tr>
</tbody>
</table><hr>
<h2 id="pertinence-des-kpi">Pertinence des KPI</h2>
<p>Ces KPI répondent chacun à un besoin stratégique :</p>

<table>
<thead>
<tr>
<th><strong>Besoin stratégique</strong></th>
<th><strong>KPI associés</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td>Mesurer la performance des centres</td>
<td>Saturation, Délai moyen</td>
</tr>
<tr>
<td>Évaluer l’accessibilité territoriale</td>
<td>Couverture, Pénétration</td>
</tr>
<tr>
<td>Mesurer la qualité du service rendu</td>
<td>Premier Dépôt Conforme</td>
</tr>
<tr>
<td>Piloter l’efficience opérationnelle</td>
<td>Charge par Agent</td>
</tr>
</tbody>
</table><p>Ces indicateurs permettent au décideur de prioriser :</p>
<ul>
<li>l’extension de centres</li>
<li>la redistribution des ressources humaines</li>
<li>la mise en place de guichets mobiles</li>
<li>l’amélioration de l’information aux usagers</li>
</ul>
<hr>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

