# Google Ranking Signals Reference

Complete reference of ranking signals from the Google API Content Warehouse v0.4.0 leak, organized by category.

Source: https://hexdocs.pm/google_api_content_warehouse/0.4.0/api-reference.html

---

## 1. Quality & Authority Signals

### CompressedQualitySignals (Primary Quality Record)

| Signal | Type | Description |
|--------|------|-------------|
| `siteAuthority` | int | Site-level authority score |
| `authorityPromotion` | int | Authority boost signal |
| `unauthoritativeScore` | int | Inverse authority — high = low authority |
| `lowQuality` | int | Low quality flag/score |
| `pqData` | int | Page quality data (encoded) |
| `pqDataProto` | QualityNsrPQData | Full page quality data structure |
| `scamness` | int | Scam likelihood score (0-1023) |
| `experimentalQstarSignal` | number | Experimental next-gen quality signal |
| `experimentalQstarDeltaSignal` | number | Q* delta (page vs expected quality) |
| `experimentalQstarSiteSignal` | number | Q* at site level |

### Neural Site Ranking (QualityNsrNsrData) — 57 fields

| Signal | Type | Description |
|--------|------|-------------|
| `nsr` | number | Main Neural Site Ranking score |
| `titlematchScore` | number | How well titles match content |
| `siteChunk` | string | Primary site chunk identifier |
| `chardEncoded` | int | Site quality predictor (encoded) |
| `tofu` | number | Site quality based on content analysis |
| `health` | number | Health/quality score |
| `vlq` | number | Very Low Quality model score |
| `articleScore` | number | Article quality classification |
| `clutterScore` | number | Clutter penalty (ads, pop-ups, etc.) |
| `ugcScore` | number | User-generated content quality |
| `spambrainLavcScore` | number | SpamBrain Link Abuse Via Content score |
| `siteAutopilotScore` | number | Aggregated autopilot quality scores |
| `site2vecEmbedding` | list | Site-level semantic embedding vector |
| `chromeInTotal` | number | Site-level Chrome usage data |
| `impressions` | number | Site-level search impressions |
| `pnav` | number | Navigation probability (fractional) |
| `pnavClicks` | number | Denominator for pnav calculation |
| `nsrEpoch` | string | Source epoch for NSR data |
| `isVideoFocusedSite` | boolean | >50% video content flag |
| `isElectionAuthority` | boolean | Election authority classification |
| `isCovidLocalAuthority` | boolean | COVID local authority classification |

### NSR Versioned Data (in CompressedQualitySignals)

| Signal | Type | Description |
|--------|------|-------------|
| `nsrVersionedData` | list | Versioned NSR scores over time |
| `nsrConfidence` | int | Confidence in the NSR score |
| `nsrOverrideBid` | number | Manual NSR override value |
| `vlqNsr` | int | VLQ-adjusted NSR score |

### Page Quality Data (QualityNsrPQData) — 18 fields

| Signal | Type | Description |
|--------|------|-------------|
| `contentEffort` | number | LLM-estimated content effort for articles |
| `chard` | int | URL-level quality predictor (encoded) |
| `chardScoreEncoded` | list | Versioned Chard scores |
| `predictedDefaultNsr` | versioned | Predicted default NSR for the page |
| `keto` | versioned | Versioned quality predictor |
| `rhubarb` | versioned | Site-URL delta model (site quality vs page quality) |
| `unversionedRhubarb` | number | Non-versioned Rhubarb delta score |
| `tofu` | number | URL-level content quality score |
| `vlq` | number | Very Low Quality model score |
| `page2vecLq` | number | Page-level low quality via embeddings |
| `linkIncoming` | number | Incoming link signal |
| `linkOutgoing` | number | Outgoing link signal |
| `numOffdomainAnchors` | number | Off-domain anchor count (seen by NSR pipeline) |
| `deltaLinkIncoming` | number | Delta incoming links |
| `deltaLinkOutgoing` | number | Delta outgoing links |
| `urlAutopilotScore` | number | URL-level autopilot quality |
| `deltaAutopilotScore` | number | Delta autopilot score |
| `deltaSubchunkAdjustment` | number | Total deltaNSR based on subchunks |

---

## 2. Demotion Signals

### CompressedQualitySignals Demotions

| Signal | Type | Range | Description |
|--------|------|-------|-------------|
| `pandaDemotion` | int | — | Panda algorithm penalty (thin/low-quality content) |
| `babyPandaDemotion` | int | — | Lighter version of Panda penalty |
| `babyPandaV2Demotion` | int | — | Updated BabyPanda penalty |
| `exactMatchDomainDemotion` | int | 0-1023 | Penalty for exact-match domains (10-bit) |
| `navDemotion` | int | — | Navigation query demotion |
| `anchorMismatchDemotion` | int | — | Penalty when anchor text doesn't match content |
| `serpDemotion` | int | — | SERP-level demotion signal |

---

## 3. NavBoost / CRAPS Click Signals

### QualityNavboostCrapsCrapsClickSignals — 10 fields

| Signal | Type | Description |
|--------|------|-------------|
| `clicks` | float | Total click count |
| `goodClicks` | float | Clicks where user was satisfied |
| `badClicks` | float | Clicks followed by quick SERP return (pogo-sticking) |
| `lastLongestClicks` | float | Longest click in session (strong satisfaction/dwell time proxy) |
| `unicornClicks` | float | Clicks from Chrome/logged-in users (higher trust weight) |
| `impressions` | float | Total impressions in search results |
| `absoluteImpressions` | float | Host-level unsquashed impressions |
| `unsquashedClicks` | float | Raw click count (not currently populated) |
| `unsquashedImpressions` | float | Raw impressions (not currently populated) |
| `unsquashedLastLongestClicks` | float | Raw longest clicks |

### CRAPS Signals in CompressedQualitySignals

| Signal | Type | Description |
|--------|------|-------------|
| `crapsNewUrlSignals` | string | URL-level CRAPS signals (encoded) |
| `crapsNewHostSignals` | string | Host-level CRAPS signals (encoded) |
| `crapsNewPatternSignals` | string | URL-pattern-level CRAPS signals |
| `crapsAbsoluteHostSignals` | int | Absolute host-level CRAPS data |
| `crapsUnscaledIpPriorBadFraction` | int | IP-level bad click fraction (pre-scaling) |

---

## 4. Link & Anchor Signals

### AnchorsAnchor (Per-Anchor Record) — 43 fields

| Signal | Type | Description |
|--------|------|-------------|
| `text` | string | Space-delimited anchor words |
| `origText` | string | Original anchor text with capitalization |
| `weight` | int (0-127) | Numeric weight of the anchor |
| `pagerankWeight` | number | PageRank weight flowing through this link |
| `bucket` | int | Quality bucket for ranking |
| `sourceType` | int | Source quality: HIGH / MEDIUM / LOW |
| `firstseenDate` | int | Days since Dec 31, 1994 when first seen |
| `deletionDate` | int | Timestamp when anchor was deleted |
| `fontsize` | int | Font size of the anchor text |
| `isLocal` | boolean | Same-domain link flag |
| `context2` | int | Hash of terms near the anchor |
| `source` | AnchorsAnchorSource | Source page attributes |
| `linkTags` | list | Link type and source information |
| `compressedImageUrls` | list | Image URLs in/near the anchor |

### Anchor Trust (IndexingDocjoinerAnchorTrustedInfo)

| Signal | Type | Description |
|--------|------|-------------|
| `site` | string | Source site name |
| `text` | list[string] | Tokenized anchor text |
| `trustedScore` | number | Fraction of pages with newsy/editorial anchors (>0 = trusted) |
| `phrasesScore` | number | Count of anchors classified as spam by text |
| `matchedScore` | number | KL-divergence from spam/non-spam (>0 = similar to spam) |
| `matchedScoreInfo` | list[string] | Debug info (verbose mode only) |

### Anchor Spam (IndexingDocjoinerAnchorSpamInfo) — 21 fields

| Signal | Type | Description |
|--------|------|-------------|
| `spamProbability` | number | ML-predicted spam likelihood |
| `spamPenalty` | number | Combined demotion penalty |
| `demoted` | int | Count of demoted anchors |
| `demotedAll` | boolean | All anchors demoted vs spam-only |
| `demotedStart` | int | Demotion period start date |
| `demotedEnd` | int | Demotion period end date |
| `phraseCount` | number | Spam phrases found |
| `phraseRate` | number | Daily spam phrase discovery rate |
| `phraseFraq` | number | Fraction of spam phrases |
| `phraseDays` | number | Days to discover 80% of phrases |
| `anchorFraq` | number | Ratio of spam-period anchors to all anchors |
| `trustedMatching` | int | Trusted anchors containing spam terms |
| `trustedTarget` | boolean | URL appears on trusted sources |
| `trustedTotal` | int | Total trusted source count |
| `trustedDemoted` | int | Trusted anchors included in demotion calc |
| `anchorStart` | int | First anchor date |
| `anchorEnd` | int | Last anchor date |
| `processed` | int | Total anchors processed |
| `sampled` | boolean | Whether sampling was used |

### Anchor Statistics (IndexingDocjoinerAnchorStatistics) — 52 fields

| Signal | Type | Description |
|--------|------|-------------|
| `anchorCount` | int | Total anchor count |
| `baseAnchorCount` | int | Base anchor count |
| `scannedAnchorCount` | int | Total scanned from storage |
| `localAnchorCount` | int | Same-site anchor count |
| `nonLocalAnchorCount` | int | External anchor count |
| `ondomainAnchorCount` | int | Same-domain anchor count |
| `offdomainAnchorCount` | int | Off-domain anchor count |
| `fakeAnchorCount` | int | Detected fake anchor count |
| `forwardedAnchorCount` | int | Forwarded/redirected anchor count |
| `anchorPhraseCount` | int | Unique phrases (capped at 5000) |
| `droppedRedundantAnchorCount` | int | Redundant anchors removed |
| `totalDomainsSeen` | int | Total linking domains |
| `totalDomainsAbovePhraseCap` | int | Domains exceeding phrase cap |
| `totalDomainPhrasePairsSeenApprox` | number | Approximate domain-phrase pair count |

### Penguin Penalty (in AnchorStatistics)

| Signal | Type | Description |
|--------|------|-------------|
| `penguinPenalty` | number | 0 = clean, 1 = fully penalized |
| `penguinLastUpdate` | int | Days since Jan 1, 1995 |
| `penguinEarlyAnchorProtected` | boolean | Early anchor protection flag |
| `penguinTooManySources` | boolean | Excessive source count flag |
| `badbacklinksPenalized` | boolean | Bad backlinks penalty active |
| `spamLog10Odds` | number | Log-odds of spammy link behavior |

### PageRank Variants (in PerDocData)

| Signal | Type | Description |
|--------|------|-------------|
| `pagerank` | int | Primary PageRank score |
| `pagerank0` | int | PageRank variant 0 |
| `pagerank1` | int | PageRank variant 1 |
| `pagerank2` | int | PageRank variant 2 |
| `crawlPagerank` | int | PageRank used for crawl priority |
| `toolbarPagerank` | int | Legacy toolbar PageRank |
| `homepagePagerankNs` | int | Homepage PageRank (namespace) |
| `ScaledIndyRank` | int (16-bit) | Independence Rank — measures authority independent of link networks |
| `scaledSelectionTierRank` | int (0-32767) | Selection tier ranking |

---

## 5. Content Quality Signals

### PerDocData Content Signals

| Signal | Type | Description |
|--------|------|-------------|
| `OriginalContentScore` | int (7-bit, 0-127) | How original the content is |
| `bodyWordsToTokensRatioTotal` | number | Body text to total tokens ratio |
| `bodyWordsToTokensRatioBegin` | number | Ratio at beginning of document |
| `originalTitleHardTokenCount` | int | Hard token count in title |
| `titleHardTokenCountWithoutStopwords` | int | Title tokens minus stopwords |
| `commercialScore` | int | Commercial intent classification |

### Quality Signals

| Signal | Type | Description |
|--------|------|-------------|
| `contentEffort` | number | LLM-estimated effort in article content (QualityNsrPQData) |
| `ugcDiscussionEffortScore` | number | Effort score for UGC/discussion content (CompressedQualitySignals) |

### Product Review Signals (CompressedQualitySignals)

| Signal | Type | Description |
|--------|------|-------------|
| `productReviewPReviewPage` | number | Probability page is a product review |
| `productReviewPUhqPage` | number | Probability page is ultra-high-quality review |
| `productReviewPPromotePage` | number | Probability page should be promoted |
| `productReviewPPromoteSite` | number | Probability site should be promoted for reviews |
| `productReviewPDemoteSite` | number | Probability site should be demoted for reviews |

---

## 6. Spam Detection Signals

### SpamBrain (in PerDocData)

| Signal | Type | Description |
|--------|------|-------------|
| `spambrainTotalDocSpamScore` | number (0-1) | Document-level spam probability |
| `spambrainData` | object | Full SpamBrain data |
| `spambrainDomainSitechunkData` | object | Domain/site-chunk spam data |

### Document-Level Spam (in PerDocData)

| Signal | Type | Description |
|--------|------|-------------|
| `DocLevelSpamScore` | int (7-bit, 0-127) | Overall document spam score |
| `uacSpamScore` | int (7-bit, 0-127) | UAC spam classification |
| `KeywordStuffingScore` | int | Keyword stuffing detection score |
| `GibberishScore` | int | Gibberish/auto-generated content score |
| `SpamWordScore` | int | Spam word density score |
| `TagPageScore` | int | Tag/category page spam score |
| `spamrank` | int (0-65535) | SpamRank score |
| `ScaledSpamScoreYoram` | int | Spam score (Yoram model) |
| `ScaledSpamScoreEric` | int | Spam score (Eric model) |
| `ScaledLinkAgeSpamScore` | int | Link age-based spam score |
| `IsAnchorBayesSpam` | boolean | Bayesian anchor spam classification |
| `spamMuppetSignals` | object | Hacked site detection signals |
| `spamCookbookAction` | object | Manual spam action data |
| `WhirlpoolDiscount` | number | Whirlpool spam discount |

### Anchor Spam Detection

| Signal | Type | Description |
|--------|------|-------------|
| `spamProbability` | number | ML-predicted anchor spam likelihood |
| `spamPenalty` | number | Combined anchor spam demotion |
| `phraseCount` | number | Spam phrases detected in anchors |
| `phraseRate` | number | Rate of new spam phrase discovery |
| `phraseFraq` | number | Fraction of anchors that are spam phrases |
| `phraseDays` | number | Days to discover 80% of spam phrases (spike detection) |

---

## 7. Freshness Signals

### PerDocData Freshness

| Signal | Type | Description |
|--------|------|-------------|
| `lastSignificantUpdate` | timestamp | Last meaningful content update |
| `lastSignificantUpdateInfo` | object | Details about the update |
| `semanticDate` | date | Semantically extracted date from content |
| `semanticDateConfidence` | number | Confidence in the semantic date |
| `semanticDateInfo` | object | Date extraction details |
| `freshnessEncodedSignals` | int | Encoded freshness signals |
| `timeSensitivity` | number | How time-sensitive the content is |
| `eventsDate` | list | Event dates associated with the page |

### Anchor Freshness

| Signal | Type | Description |
|--------|------|-------------|
| `firstseenDate` | int | Days since Dec 31, 1994 when anchor first appeared |
| `deletionDate` | int | When the anchor was removed |

---

## 8. YMYL & Specialization Signals

### PerDocData YMYL

| Signal | Type | Description |
|--------|------|-------------|
| `ymylHealthScore` | number | Your Money Your Life — health content score |
| `ymylNewsScore` | number | YMYL — news content score |

### Authority Classifications (QualityNsrNsrData)

| Signal | Type | Description |
|--------|------|-------------|
| `isElectionAuthority` | boolean | Classified as election authority |
| `isCovidLocalAuthority` | boolean | Classified as COVID local authority |
| `isVideoFocusedSite` | boolean | >50% video content |

### PerDocData Authority Flags

| Signal | Type | Description |
|--------|------|-------------|
| `nsrIsCovidLocalAuthority` | boolean | COVID authority flag (PerDocData copy) |
| `nsrIsElectionAuthority` | boolean | Election authority flag (PerDocData copy) |
| `nsrIsVideoFocusedSite` | boolean | Video site flag (PerDocData copy) |

---

## 9. Domain & Site Signals

### PerDocData Domain/Host

| Signal | Type | Description |
|--------|------|-------------|
| `domainAge` | int (16-bit) | Age of the domain |
| `hostAge` | int | Age of the host |
| `homepagePagerankNs` | int | Homepage PageRank |
| `onsiteProminence` | number | How prominent the page is within its site |
| `nsrSitechunk` | string | NSR site chunk identifier |
| `hostNsr` | encoded | Encoded: nsr + site_pr + new_nsr |
| `tundraClusterId` | int | Tundra cluster assignment |
| `topPetacatTaxonomy` | object | Top category taxonomy |

---

## 10. Sitemap & Navigation Signals

### QualitySitemapScoringSignals — 18 fields

| Signal | Type | Description |
|--------|------|-------------|
| `pagerank` | int | Sitemap-context PageRank |
| `navboostScore` | number | NavBoost quality in sitemap context |
| `navmenuScore` | number | Navigation menu quality score |
| `titleScore` | number | Title quality score |
| `longCtr` | number | Long-click CTR |
| `recentLongCtr` | number | Recent long-click CTR |
| `chromeWeight` | number | Chrome usage weight |
| `chromeTransProb` | number | Chrome transition probability |
| `longClicks` | string | Long click data (encoded) |
| `impressions` | string | Impression data (encoded) |
| `chromeTransCount` | string | Chrome transition count (encoded) |

---

## 11. Embedding & Topic Signals

### CompressedQualitySignals Embeddings

| Signal | Type | Description |
|--------|------|-------------|
| `topicEmbeddingsVersionedData` | list | Versioned topic embeddings |
| `pairwiseqVersionedData` | list | Pairwise quality comparison data |
| `pairwiseqScoringData` | object | Pairwise quality scoring |
| `experimentalNsrTeamData` | list | Experimental NSR team signals |
| `experimentalNsrTeamWsjData` | list | Experimental NSR WSJ team signals |

### Site Embeddings (QualityNsrNsrData)

| Signal | Type | Description |
|--------|------|-------------|
| `site2vecEmbedding` | list | Site-level semantic vector representation |

---

## 12. Mobile & Device Signals

### PerDocData Mobile

| Signal | Type | Description |
|--------|------|-------------|
| `MobileData` | object | Mobile rendering/usability data |
| `smartphoneData` | object | Smartphone-specific data |
| `desktopInterstitials` | object | Desktop interstitial ad data |
| `voltData` | object | VOLT (Video Optimization for Loading Time) |

---

## 13. Entity & Semantic Signals

### PerDocData Entity Data

| Signal | Type | Description |
|--------|------|-------------|
| `webrefEntities` | object | WebRef entity annotations |
| `mediaOrPeopleEntities` | object | Media/people entity annotations |
| `knexAnnotation` | object | Knowledge entity annotations |
| `v2KnexAnnotation` | object | Knowledge entity annotations v2 |

---

## Module Cross-Reference

| SEO Concept | Internal System | Key Signal |
|-------------|----------------|------------|
| Site authority | CompressedQualitySignals | `siteAuthority` |
| Page quality | QualityNsrPQData | `contentEffort`, `chard`, `tofu` |
| Link quality | AnchorsAnchor | `pagerankWeight`, `sourceType`, `bucket` |
| Link spam | AnchorSpamInfo | `spamProbability`, `penguinPenalty` |
| User satisfaction | NavBoost/CRAPS | `goodClicks`, `lastLongestClicks` |
| Content freshness | PerDocData | `lastSignificantUpdate`, `freshnessEncodedSignals` |
| Thin content penalty | CompressedQualitySignals | `pandaDemotion`, `babyPandaDemotion` |
| Keyword stuffing | PerDocData | `KeywordStuffingScore` |
| Overall spam | SpamBrain | `spambrainTotalDocSpamScore` |
| Domain quality | QualityNsrNsrData | `nsr`, `clutterScore` |
| Click quality | CRAPS | `crapsNewUrlSignals`, `crapsNewHostSignals` |
| Content originality | PerDocData | `OriginalContentScore` |
| Review quality | CompressedQualitySignals | `productReviewPUhqPage` |
| EMD penalty | CompressedQualitySignals | `exactMatchDomainDemotion` |
| YMYL | PerDocData | `ymylHealthScore`, `ymylNewsScore` |
| Independence | PerDocData | `ScaledIndyRank` |
