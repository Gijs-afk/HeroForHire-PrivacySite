# Hero For Hire website (support, privacy policy, app-ads.txt)

Plain static files for GitHub Pages. No JavaScript, no web fonts, no CDN, no cookies, no analytics: the site itself collects nothing.

| File | What |
|---|---|
| `index.html` | Support page: what the game is, contact, six questions |
| `privacy.html` | Privacy policy |
| `app-ads.txt` | AdMob's authorised-seller file (placeholder) |
| `style.css` | The one stylesheet, light and dark |
| `_config.yml` | Tells GitHub Pages (Jekyll) not to publish this README |

Written 20 November 2026 against the game repo on `develop` after pull request #104 (monetisation round 4). Nothing here is legal advice.

## 1. Fill the placeholders

Search all files for `{{` and replace every one. Do not publish with a placeholder left.

| Placeholder | Where | What |
|---|---|---|
| `{{CONTACT_EMAIL}}` | `index.html`, `privacy.html` (several times) | The support and privacy contact address. Use one you will read; Apple and Google both require working contact details |
| `{{DEVELOPER_NAME}}` | `index.html`, `privacy.html` | The name the stores show as the developer (your name or your business name). GDPR asks the controller's identity |
| `{{DEVELOPER_ADDRESS}}` | `privacy.html`, section "Who we are" | A postal address, or delete the line (see below) |
| `{{EFFECTIVE_DATE}}` | `privacy.html`, top | The date the policy goes live, for example `1 December 2026`. Change it with every new version |
| `{{ADMOB_PUBLISHER_LINE}}` | `app-ads.txt` | The line AdMob shows you, of the form `google.com, pub-XXXXXXXXXXXXXXXX, DIRECT, f08c47fec0942fa0` (your own publisher id). Find it in AdMob under Apps, View all apps, app-ads.txt |

### The address question (decide with your adviser)

- **GDPR (article 13)** asks for the controller's identity and contact details. An email address is a contact detail; many sole developers also give a postal address. Not strictly one or the other in the text.
- **The e-Commerce Directive (article 5; in Dutch law Burgerlijk Wetboek 3:15d)** asks an information-society service provider to make easily available: name, geographic address, email, and, where they apply, the trade register number (KvK) and the VAT number. A game with ads and in-app purchases is likely such a service.
- **The Digital Services Act (articles 30 and 31)** makes app stores show a trader's name, address, phone number and email on EU listings. Apple and Google collect this as "trader status"; this is the store listing, not this website, but it shows the same address publicly.
- If you register a business (KvK), the KvK may let you use a business or postal address instead of your home address. The road-to-live doc (`docs/superpowers/specs/2026-11-20-road-to-live.md`, rows L3 and L5) already lists this for the adviser.

If you decide to publish an address, also add it to the footer of `index.html`, and add the KvK and VAT number when you have them.

### Two things in the policy that are your promises, not facts from the repo

Check these are true for you, or change them:

1. "We keep [support email] no longer than two years after your last message." Pick your own period if you prefer; GDPR only says "no longer than necessary".
2. "We keep the little we hold ... in accounts protected with strong passwords and two-step sign-in." Turn on two-step sign-in for the mail account, AdMob, RevenueCat and the store consoles, or soften the sentence.

## 2. Publish on GitHub Pages

1. Make a new public repository and put these files at its **root** (not in a subfolder). Which name you give it matters for `app-ads.txt`; read section 4 first.
2. Repository, Settings, Pages: Source **Deploy from a branch**, Branch **main**, folder **/ (root)**. Save.
3. Wait a minute, then open the address Pages shows. Tick **Enforce HTTPS**.
4. Check: `/`, `/privacy.html` and `/app-ads.txt` open; `/README.md` and `/README.html` give 404.

## 3. Which URL goes where

With `BASE` the site's address (for example `https://<user>.github.io`):

| Place | Field | URL |
|---|---|---|
| App Store Connect, App Information | Privacy Policy URL | `BASE/privacy.html` |
| App Store Connect, version page | Support URL | `BASE/` |
| App Store Connect, version page | Marketing URL (optional) | `BASE/` |
| Play Console, App content, Privacy policy | Privacy policy | `BASE/privacy.html` |
| Play Console, Store settings, Store listing contact details | Website | `BASE/` (must be the host that serves `app-ads.txt`) |
| Play Console, Store settings | Email | `{{CONTACT_EMAIL}}` |
| AdMob, Privacy & messaging, the GDPR message | Privacy policy URL | `BASE/privacy.html` |
| In the game, Settings | A "Privacy policy" row (Play and Apple both ask for a link inside the app) | `BASE/privacy.html` |

The game has no "Privacy policy" link in Settings yet (only `Privacy choices`). Both stores require one inside the app: that is a small round for the game repo.

AdMob verifies `app-ads.txt` against the **developer website in the store listing**. Google crawls it after the listing is live; it can take up to 24 hours. Until then AdMob shows "not found", which is normal.

## 4. app-ads.txt: where it must live

AdMob's rule: the crawler takes only the **host name** of the developer website in the store listing and asks for `https://<host>/app-ads.txt` (then `http://`). The path is ignored: a developer website `example.com/game` is checked at `example.com/app-ads.txt`. It checks at most one subdomain level, and treats `www.` and `m.` as the bare domain. A redirect from that place to the file elsewhere is allowed.

What that means for GitHub Pages:

| Kind of site | Address | Works for app-ads.txt? |
|---|---|---|
| **Project site** (repo with any name, e.g. `heroforhire-site`) | `https://<user>.github.io/heroforhire-site/` | **No, not by itself.** The crawler asks `https://<user>.github.io/app-ads.txt`, which belongs to your user site, not to this repo. Only works if you also have a user site repo that serves `app-ads.txt` at its root |
| **User site** (repo named exactly `<user>.github.io`) | `https://<user>.github.io/` | **Yes.** The files sit at the host's root. The simplest choice: name the repository `<user>.github.io` |
| **Custom domain on either** (e.g. `heroforhire.example`) | `https://heroforhire.example/` | **Yes**, when the domain's root serves the file. If you use `www.heroforhire.example`, the crawler checks the bare `heroforhire.example`; configure both in DNS (GitHub Pages then redirects one to the other, and a redirect is allowed). A subdomain like `games.example.com` works too (one level) |

Recommendation: a **user site** (`<user>.github.io`) or a **custom domain**. Use the same host as the Play developer website and, on Apple, as the Marketing or Support URL. If your GitHub user name is personal and you do not want it in the store, a custom domain avoids that.

Note: if you already have a user site for something else, put `app-ads.txt` at its root instead and keep this as a project site; one `app-ads.txt` can hold the lines of several apps of the same AdMob account (it is per publisher, not per app).

## 5. What must change when the game changes

The policy is written to be true **today** (game on `develop` after #104). Update `privacy.html` (and the effective date) **before** the build that ships any of these:

| When this ships | Change |
|---|---|
| **The Guild Charter / RevenueCat (monetisation round 5)** | The "Purchases" section is already written as "when purchases are available". Check it against what round 5 actually sends (RevenueCat app user id, receipts, the `purchaseLog` in the save). Fill the App Store privacy label (Purchases) and the Play data safety form (Purchase history, Device or other ids) |
| **Analytics or crash reporting** (planned through Unity Gaming Services, `2026-11-20-crash-and-analytics-design.md`) | Today the policy says plainly "no analytics and no crash reporting". **Remove that line** and add a section: who (Unity Technologies), what (events, device data, crash logs), legal basis (consent, given in the game), retention, transfers, how to opt out. Update the "In short" list, both store forms and the consent design. This was left out on purpose so the published page is true now |
| **Cloud save or accounts** (`2026-11-20-cloud-save-and-account-design.md`) | Change "There is no account" and "Your game is saved only on your phone" everywhere (also the support FAQ). Add what is stored on the server, for how long, and **account deletion**: Apple requires deletion inside the app for any app that lets players create an account, and Google Play requires an in-app path and a **web page** to request deletion. Add that page here |
| **Paid gems or a subscription** | Update the purchases section and the Guild Charter answer in the FAQ; check "Is anything random sold" is still true |
| **ATT on iOS, or personalised ads** | The policy says the game does not track and that ads are non-personalised everywhere. Both lines must change, and the App Store label "Data Used to Track You" too |
| **Removing the `AD_ID` permission on Android** | Drop "advertising ID" from the Google list if Google no longer receives it |
| **A different ad network, mediation or a new SDK** | Name it, with a link to its policy |
| **Audience changed below 13** | Rewrite "Children"; Families policy applies on Play |

**Check before the first public release:** `ProjectSettings/ProjectSettings.asset` has `submitAnalytics: 1` (Unity's old "hardware statistics" switch), while Unity Analytics, Performance Reporting and Crash Reporting are all off in `UnityConnectSettings.asset`. Unity staff say that with Analytics off no hardware statistics are sent, but users report traffic anyway. Before you publish, watch a release build's network traffic once (a proxy, or Android's network inspector) and confirm that only Google ads traffic leaves the phone. If Unity sends device data, the policy needs a Unity section.

## 6. What was checked in the game repo

What leaves the phone today, from the code on `develop`:

- **Google Mobile Ads 11.7.0 with UMP** (`Packages/manifest.json`, `Assets/Scripts/Monetisation/*`): the SDK starts only after UMP says ads may be requested; consent form at the first ad tap where required; `Privacy choices` in Settings when `PrivacyOptionsRequirementStatus` is `Required`; `MaxAdContentRating = PG`; `PublisherPrivacyPersonalizationState.Disabled` (non-personalised for everyone); child tags unset; no ATT, empty `userTrackingUsageDescription`; Android Topics permission removed, `AD_ID` kept. The paid-event value is only logged locally.
- **RevenueCat 10.0.3**: installed, no code calls it.
- **Notifications** (`com.unity.mobile.notifications` 2.4.3): local only; `NotificationsSettings.asset` has the remote-notification capability off; switches in `PlayerPrefs`.
- **Save**: `savegame.json` in `Application.persistentDataPath`, never sent.
- **No other network code**: no `UnityWebRequest`, `HttpClient`, `WebClient`, sockets or analytics calls in `Assets/Scripts`. Unity Analytics, Crash Reporting and Performance Reporting are off.

## 7. Sources (read 20 November 2026)

Google AdMob and Google
- Google Publisher Policies, Privacy disclosures (a privacy policy must disclose data collection, sharing and use, third parties' cookies and identifiers; the optional link "How Google uses data when you use our partners' sites or apps"): https://support.google.com/publisherpolicies/answer/10437794
- How Google uses information from sites or apps that use our services: https://policies.google.com/technologies/partner-sites
- EU user consent policy (identify each party, consent for local storage and personalisation, revocation): https://www.google.com/about/company/user-consent-policy/
- AdMob data disclosure for the Play data safety form (IP address, app interactions, diagnostics, device ids; advertising, analytics, fraud prevention; encrypted in transit): https://developers.google.com/admob/android/privacy/play-data-disclosure
- AdMob app-ads.txt hosting rules (host only, path ignored, one subdomain level, `www.` and `m.` skipped, redirects allowed): https://support.google.com/admob/answer/9363762
- Google as an independent controller for AdMob, Controller-Controller Data Protection Terms (with the EU and UK SCCs): https://support.google.com/admob/answer/9035987 , https://business.safety.google/adscontrollerterms/ , https://business.safety.google/adsservices/
- Non-personalised ads still use cookies or identifiers for frequency capping, reporting and fraud: https://support.google.com/admob/answer/7676680
- Consent is needed for both personalised and non-personalised ads in the EEA: https://support.google.com/admob/answer/9760862
- UMP for Unity: https://developers.google.com/admob/unity/privacy

RevenueCat
- What the SDK collects for Apple's privacy labels (purchase history; identifiers only with custom ids; no diagnostics; no location beyond locale and currency): https://www.revenuecat.com/docs/platform-resources/apple-platform-resources/apple-app-privacy
- Data Processing Addendum (RevenueCat is the processor, the developer the controller; EU SCCs, UK addendum; sub-processors mostly in the USA): https://www.revenuecat.com/dpa
- Data and compliance (DPA included in the standard terms): https://www.revenuecat.com/docs/welcome/set-up-revenuecat/data-and-compliance

Apple
- App Review Guidelines 1.5 (Support URL with an easy way to contact you), 5.1.1(i) (privacy policy link in App Store Connect and in the app; what data, how, all uses; third parties give equal protection; retention and deletion; how to revoke consent or request deletion), 5.1.1(v) (account deletion if the app supports account creation): https://developer.apple.com/app-store/review/guidelines/
- Offering account deletion in your app: https://developer.apple.com/support/offering-account-deletion-in-your-app/
- App privacy details (labels): https://developer.apple.com/app-store/app-privacy-details/
- EU trader status under the DSA: https://developer.apple.com/help/app-store-connect/manage-compliance-information/manage-european-union-digital-services-act-compliance-information

Google Play
- User Data policy, privacy policy requirements (public, non-geofenced URL, no PDF; developer information and a privacy contact; data types; parties shared with; secure handling; retention and deletion; labelled as a privacy policy; link in Play Console and in the app): https://support.google.com/googleplay/android-developer/answer/10144311
- Target audience and content (13 and over is outside the Families programme; an app that also appeals to children must follow the Families rules): https://support.google.com/googleplay/android-developer/answer/9893335
- Data safety form: https://support.google.com/googleplay/android-developer/answer/10787469
- Account deletion requirement (in-app and a web link, for apps with accounts): https://support.google.com/googleplay/android-developer/answer/13327111

EU and the Netherlands
- GDPR, article 13 (what to tell people), 6 (legal bases), 8 (child's consent, 13 to 16 by country), 15 to 22 (rights), 44 to 49 (transfers), 77 (complaint): https://eur-lex.europa.eu/eli/reg/2016/679/oj
- Dutch GDPR Implementation Act (UAVG), article 5: digital consent age 16 in the Netherlands: https://wetten.overheid.nl/BWBR0040940
- Autoriteit Persoonsgegevens, the Dutch supervisory authority: https://autoriteitpersoonsgegevens.nl
- EU-U.S. Data Privacy Framework (Google LLC participates): https://www.dataprivacyframework.gov
- e-Commerce Directive 2000/31/EC, article 5 (information a service provider must publish): https://eur-lex.europa.eu/eli/dir/2000/31/oj
- Burgerlijk Wetboek Boek 3, artikel 15d: https://wetten.overheid.nl/BWBR0005291
- Digital Services Act, articles 30 and 31 (trader traceability on marketplaces): https://eur-lex.europa.eu/eli/reg/2022/2065/oj

Unity
- Hardware statistics and Analytics (old switch removed; turning Analytics off is the switch now; reports of traffic anyway): https://discussions.unity.com/t/app-for-kids-how-to-totally-disable-hw-stats-and-analytics/892637 , https://docs.unity3d.com/2023.2/Documentation/ScriptReference/Analytics.Analytics-deviceStatsEnabled.html

GitHub
- What is GitHub Pages (user, organisation and project sites): https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages
- Configuring a publishing source: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
- Custom domains: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site
- GitHub General Privacy Statement (hosting logs): https://docs.github.com/en/site-policy/privacy-policies/github-general-privacy-statement

Not verified online this round: the Play account-deletion page (answer 13327111) and the e-Commerce, DSA and UAVG texts were cited from knowledge, not reopened; check them with your adviser.
