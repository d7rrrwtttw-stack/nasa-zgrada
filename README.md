# Naša zgrada — postavljanje

Aplikacija za finansije zgrade: uplate, rashodi, zaduženja, dugovanja, računi i izvještaji. Upravnik unosi, stanari gledaju.

## 1. Baza (Supabase, besplatni plan)
1. Napravi novi projekat na supabase.com.
2. SQL Editor → New query → zalijepi cijeli `schema.sql` → Run.
   (Može se pokrenuti ponovo, i preko ranije verzije, bez gubitka podataka.)
3. Authentication → URL Configuration → **Site URL** = adresa gdje će aplikacija biti (npr. Netlify link).
4. Authentication → Sign In / Providers → Email: **Confirm email mora ostati UKLJUČEN.**
   Bez potvrde bi svako ko zna nečiji email mogao da se registruje tim emailom i vidi tuđu karticu.

## 2. Email limit — važno prije nego pozoveš stanare
Supabase-ov ugrađeni mejl server šalje samo nekoliko mejlova na sat.
Ako se 20 stanara registruje isti dan, većina neće dobiti mejl za potvrdu.
Rješenje: Authentication → Emails → SMTP Settings → poveži besplatni servis
(npr. Brevo ili Resend). Alternativa: pozivaj stanare postepeno, nekoliko dnevno.

## 3. Konfiguracija
U `config.js` upiši **Project URL** i **anon / publishable key** (Project Settings → API).
Taj ključ je javan po dizajnu. Podatke štite pravila pristupa iz `schema.sql`.

## 4. Hosting
Cijeli folder (index.html, config.js, sw.js, manifest.json, ikone) ide na Netlify ili bilo koji statički hosting.
Preporuka: sa računara, ne sa iPhone-a.

## 5. Prvi admin
1. Otvori aplikaciju → **Registracija** svojim emailom → potvrdi email → prijavi se.
   Vidjećeš „Nalog nije povezan". To je očekivano.
2. U Supabase SQL Editoru:
   ```sql
   update profiles set uloga = 'admin' where email = 'tvoj@email.com';
   ```
3. Klikni **Osvježi**.

## 6. Početni unos (redom)
1. **Podešavanja** → naziv zgrade, PIB, upravnik. Žiroračun: upiši broj i **početno stanje** na dan početka evidencije.
2. **Prostori → Dodaj više** → Stan 1–20, mjesečna naknada.
3. Klikni svaki stan → **Uredi** → vlasnik i **email**.
   - Više emailova (supružnici, suvlasnici): odvoji zarezom.
   - Isti email uz stan i garažu: vlasnik vidi oba.
4. Stari dugovi: **Zaduženja → + Jednokratno**, opis „Početno stanje", za svaki prostor posebno.
5. Zaduženja za tekući mjesec: na **Pregledu** se pojavi dugme dok ih ne generišeš. Radi to **svakog mjeseca**.
6. Uplate i rashode unosiš sa bankovnog izvoda. Iznosi mogu sa zarezom (25,50).

## Stanari
Registruju se emailom koji si upisao uz njihov prostor. Vide: stanje računa zgrade, svoju karticu (sve svoje prostore), sve rashode i mjesečne izvještaje.
Listu dugovanja (samo oznake prostora, bez imena) vide samo ako to uključiš u Podešavanjima.
Ako se neko registrovao prije nego što si upisao njegov email, upiši email pa neka klikne „Osvježi".

## Računi
**Izvještaji → Štampaj sve račune** → u dijalogu za štampu „Sačuvaj kao PDF". Jedan račun po stranici.

## Rezervna kopija i pauziranje
- **Izvještaji → Preuzmi rezervnu kopiju (JSON)** — jednom mjesečno.
- Besplatni Supabase projekat se **pauzira posle 7 dana bez aktivnosti**. Podaci ostaju; u Supabase dashboardu klikneš „Restore". Ako to smeta stanarima, rješenje je plaćeni plan ili automatski „ping" bazi.
