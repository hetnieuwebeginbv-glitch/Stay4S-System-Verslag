# ONTWERP 2026-09-07 — Klantenportaal F1 (Stay4S Command Center)

Auteur: Stay4Compa [compa]
Status: ter review door Mitchell
Doel: zelfservice-dashboard voor particuliere én zakelijke klanten op één gedeeld klantmodel, multi-tenant klaar voor Stay4LM-abonnees.

## 1. Pagina-indeling (uitbreiding NexusAgent, 3 nieuwe pagina's)

### P1. Klant Overzicht (per klant)
Profielkaart: bedrijfsnaam/contactpersoon, klant_type (particulier/zakelijk), status, sector, land.
Abonnementskaart: pakket, start_datum, gratis_tot, status, referral_code.
Agents-lijst: gehuurde agents met naam + rol.
Referral-kaart: eigen code, verdiende gratis maanden, aantal doorverwezen klanten.

### P2. Klant Beheer (admin = Mitchell)
Tabel alle Klant-records, filters op status/pakket/sector.
Quick actions: status wijzigen, abonnement starten/stopzetten, agent toewijzen.
Koppeling: nieuwe Safe4AIAanvraag aanmaken vanuit klantkaart.

### P3. Aanvragen & Certificaten
Safe4AIAanvraag-lijst met status-flow: nieuw → in beoordeling → goedgekeurd → certificaat uitgegeven.
Certificaatdetail: bedrijfsnaam, pakket, agent_naam, certificaat_datum.

## 2. Data-model (bestaande entities volstaan voor MVP)
Klant, Abonnement, EarlyAdopter, Referral, Safe4AIAanvraag — allemaal al aanwezig.
Schema-update advies (1 wijziging): Klant + veld `klant_type` (particulier/zakelijk). Later eventueel `gebruikersnaam` voor API-toegang.

## 3. Toekomstbestendigheid (afspraken)
1. Eén klant-ID koppelt alles: abonnement, referral, certificaat, later Stay4Safe-account en kluis-toegang
2. API-first: pagina's lezen via entity-API zodat dezelfde data later de Stay4LM-webshop en Stay4Safe-app voedt
3. RLS aanzetten zodra klanten zelf kunnen inloggen (MVP is admin-only)
4. NL-labels veld-gestuurd houden → Engels later mogelijk zonder herstructurering

## 4. Bouwvolgorde
1. klant_type-veld doorvoeren (manage_entity_schemas update)
2. P2 Klant Beheer (admin-pagina, snelste waarde)
3. P1 Klant Overzicht
4. P3 Aanvragen & Certificaten
5. Referral-kaart activeren (sluit aan bij F1.2 referral-machine)

## 5. Opschoning Base44 (voorbereiding)
Te archiveren (geverifieerd 7 sep 2026, geen unieke data): AI-Base (Klant: 0 records), Ai/base, King, Stay4 Network (NetworkNode is kopie van Stay4Compa: zelfde 4 nodes incl. relay-mesh-03), untitled, AppHub.
Blijven: Stay4Compa (brein) + NexusAgent (hernoemen naar Stay4S Command Center).
