# ZATCA e-invoicing validation rules, explained (Arabic + English)

Every business rule ZATCA applies to Saudi e-invoices in Phase 2 (BR-KSA and EN 16931), as one JSON file: the official
message, the severity (error = rejected, warning = accepted with a warning), the condition the ZATCA SDK **actually executes**
(which sometimes differs from the published text), and a plain explanation and fix in English and Arabic.

- **245 rules**, from the Schematron files in ZATCA E-invoicing SDK **3.4.8**.
- Also on Hugging Face: https://huggingface.co/datasets/nagieeb/zatca-rules
- `rules.json`: one object per rule (`id`, `severity`, `official_text`, `fails_when`, `en_title`, `en_meaning`, `en_fix`, `ar_title`, `ar_meaning`, `ar_fix`, `applies_to`, `fields`, `url_en`, `url_ar`).
- Every rule has a page: English at `https://matheel.app/en/zatca-errors/<id>`, Arabic at `https://matheel.app/akhta/<id>`.
- Validate an invoice against these rules in your browser, free: **https://matheel.app/en/zatca-validator** (Arabic: https://matheel.app/fahs).

## Why the executed condition matters

The SDK's compiled XSLT runs an `xsl:if` that is not always the `test` text it reports. Examples found while building this:
BR-KSA-27 (QR) is only enforced on simplified invoices; BR-KSA-10 (buyer address) applies to every standard invoice, not only
non-Saudi buyers; for out-of-scope lines (category `O`) the accepted reason code is `VATEX-SA-OOS`, and the older `VAT-OOS-SA`
raises BR-KSA-CL-04. Each case is noted in the rule's `en_meaning`.

## قواعد زاتكا مشروحة بالعربي

كل قواعد التحقق اللي تطبّقها هيئة الزكاة والضريبة والجمارك على الفاتورة الإلكترونية في المرحلة الثانية، مع نص الهيئة الرسمي،
والشرط اللي ينفّذه SDK فعلاً، وشرح وحل بالعربي والإنجليزي. صفحة لكل قاعدة على https://matheel.app/akhta، وأداة فحص مجانية على https://matheel.app/fahs.

## Rules

| Rule | Severity | What it requires |
|---|---|---|
| [BR-02](https://matheel.app/en/zatca-errors/br-02) | warning | Every invoice needs an invoice number (BT-1) |
| [BR-03](https://matheel.app/en/zatca-errors/br-03) | error | Every invoice needs an issue date (BT-2) |
| [BR-04](https://matheel.app/en/zatca-errors/br-04) | error | Every invoice needs an invoice type code (BT-3) |
| [BR-05](https://matheel.app/en/zatca-errors/br-05) | error | Every invoice needs a currency code (BT-5) |
| [BR-06](https://matheel.app/en/zatca-errors/br-06) | warning | The seller's legal name (BT-27) must be present |
| [BR-08](https://matheel.app/en/zatca-errors/br-08) | warning | The seller's postal address (BG-5) must be present |
| [BR-09](https://matheel.app/en/zatca-errors/br-09) | warning | The seller address must include a country code (BT-40) |
| [BR-13](https://matheel.app/en/zatca-errors/br-13) | warning | The invoice total without VAT (BT-109) must be present |
| [BR-14](https://matheel.app/en/zatca-errors/br-14) | error | The invoice total with VAT (BT-112) must be present |
| [BR-15](https://matheel.app/en/zatca-errors/br-15) | error | The amount due for payment (BT-115) must be present |
| [BR-16](https://matheel.app/en/zatca-errors/br-16) | error | Every invoice needs at least one invoice line (BG-25) |
| [BR-21](https://matheel.app/en/zatca-errors/br-21) | warning | Each invoice line needs a line identifier (BT-126) |
| [BR-22](https://matheel.app/en/zatca-errors/br-22) | error | Each invoice line needs an invoiced quantity (BT-129) |
| [BR-24](https://matheel.app/en/zatca-errors/br-24) | error | Each invoice line needs a line net amount (BT-131) |
| [BR-25](https://matheel.app/en/zatca-errors/br-25) | warning | Each invoice line needs an item name (BT-153) |
| [BR-26](https://matheel.app/en/zatca-errors/br-26) | error | Each invoice line needs an item net price (BT-146) |
| [BR-31](https://matheel.app/en/zatca-errors/br-31) | error | Each document-level allowance needs an amount (BT-92) |
| [BR-32](https://matheel.app/en/zatca-errors/br-32) | error | Each document-level allowance needs a VAT category code (BT-95) |
| [BR-36](https://matheel.app/en/zatca-errors/br-36) | warning | Each document-level charge needs an amount (BT-99) |
| [BR-37](https://matheel.app/en/zatca-errors/br-37) | warning | Each document-level charge needs a VAT category code (BT-102) |
| [BR-41](https://matheel.app/en/zatca-errors/br-41) | error | Each line-level allowance needs an amount (BT-136) |
| [BR-43](https://matheel.app/en/zatca-errors/br-43) | warning | Each line-level charge needs an amount (BT-141) |
| [BR-45](https://matheel.app/en/zatca-errors/br-45) | error | Each VAT breakdown needs a taxable amount (BT-116) |
| [BR-46](https://matheel.app/en/zatca-errors/br-46) | error | Each VAT breakdown needs a VAT amount (BT-117) |
| [BR-47](https://matheel.app/en/zatca-errors/br-47) | error | Each VAT breakdown needs a VAT category code (BT-118) |
| [BR-48](https://matheel.app/en/zatca-errors/br-48) | error | Each VAT breakdown needs a VAT rate (BT-119) unless category is O |
| [BR-49](https://matheel.app/en/zatca-errors/br-49) | warning | Payment means must state a payment means code (BT-81) |
| [BR-52](https://matheel.app/en/zatca-errors/br-52) | warning | Each additional document reference needs an ID (BT-122) |
| [BR-53](https://matheel.app/en/zatca-errors/br-53) | warning | With a tax currency (BT-6), give the VAT total in it (BT-111) |
| [BR-54](https://matheel.app/en/zatca-errors/br-54) | error | Each item attribute needs a name (BT-160) and a value (BT-161) |
| [BR-55](https://matheel.app/en/zatca-errors/br-55) | error | A billing reference must give the preceding invoice number (BT-25) |
| [BR-CL-01](https://matheel.app/en/zatca-errors/br-cl-01) | error | Invoice type code must be a valid UNTDID 1001 code |
| [BR-CL-03](https://matheel.app/en/zatca-errors/br-cl-03) | error | Amount currencyID must be an ISO 4217 alpha-3 code |
| [BR-CL-04](https://matheel.app/en/zatca-errors/br-cl-04) | error | Invoice currency code must be an ISO 4217 alpha-3 code |
| [BR-CL-05](https://matheel.app/en/zatca-errors/br-cl-05) | error | Tax currency code must be an ISO 4217 alpha-3 code |
| [BR-CL-14](https://matheel.app/en/zatca-errors/br-cl-14) | warning | Country codes must be ISO 3166-1 alpha-2 codes |
| [BR-CL-16](https://matheel.app/en/zatca-errors/br-cl-16) | warning | Payment means code must be from the UNCL4461 list |
| [BR-CL-18](https://matheel.app/en/zatca-errors/br-cl-18) | error | Tax category codes must come from the UNCL5305 code list |
| [BR-CL-KSA-14](https://matheel.app/en/zatca-errors/br-cl-ksa-14) | error | The QR code value must be 1 to 1000 characters long |
| [BR-CO-04](https://matheel.app/en/zatca-errors/br-co-04) | error | Every invoice line needs a VAT category code |
| [BR-CO-10](https://matheel.app/en/zatca-errors/br-co-10) | warning | Total line amount must equal the sum of all line net amounts |
| [BR-CO-11](https://matheel.app/en/zatca-errors/br-co-11) | error | Allowance total must equal the sum of document-level allowances |
| [BR-CO-12](https://matheel.app/en/zatca-errors/br-co-12) | error | Charge total must equal the sum of document-level charges |
| [BR-CO-13](https://matheel.app/en/zatca-errors/br-co-13) | warning | Total without VAT = line amounts - allowances + charges |
| [BR-CO-14](https://matheel.app/en/zatca-errors/br-co-14) | error | Total VAT must equal the sum of the VAT breakdown amounts |
| [BR-CO-15](https://matheel.app/en/zatca-errors/br-co-15) | error | Total with VAT must equal total without VAT plus total VAT |
| [BR-CO-16](https://matheel.app/en/zatca-errors/br-co-16) | warning | Amount due = total with VAT - prepaid amount + rounding amount |
| [BR-CO-17](https://matheel.app/en/zatca-errors/br-co-17) | warning | VAT amount per category must equal taxable amount x rate |
| [BR-CO-18](https://matheel.app/en/zatca-errors/br-co-18) | error | Every invoice needs at least one VAT breakdown |
| [BR-CUSTOM-VALIDATION-01](https://matheel.app/en/zatca-errors/br-custom-validation-01) | error | Seller and buyer VAT numbers must not be the same |
| [BR-DEC-01](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Document level allowance amount (BT-92) is  |
| [BR-DEC-02](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Document level allowance base amount (BT-93 |
| [BR-DEC-05](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Document level charge amount (BT-99) is 2. |
| [BR-DEC-06](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Document level charge base amount (BT-100)  |
| [BR-DEC-09](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Sum of Invoice line net amount (BT-106) is  |
| [BR-DEC-10](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Sum of allowanced on document level (BT-107 |
| [BR-DEC-11](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Sum of charges on document level (BT-108) i |
| [BR-DEC-12](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Invoice total amount without VAT (BT-109) i |
| [BR-DEC-13](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Invoice total VAT amount (BT-110) is 2. |
| [BR-DEC-14](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Invoice total amount with VAT (BT-112) is 2 |
| [BR-DEC-15](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Invoice total VAT amount in accounting curr |
| [BR-DEC-16](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Pre-Paid amount (BT-113) is 2. |
| [BR-DEC-17](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Rounding amount (BT-114) is 2. |
| [BR-DEC-18](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Amount due for payment (BT-115) is 2. |
| [BR-DEC-19](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the VAT category taxable amount (BT-116) is 2. |
| [BR-DEC-20](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the VAT category tax amount (BT-117) is 2. |
| [BR-DEC-23](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Invoice line net amount (BT-131) is 2. |
| [BR-DEC-24](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Invoice line allowance amount (BT-136) is 2 |
| [BR-DEC-25](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Invoice line allowance base amount (BT-137) |
| [BR-DEC-27](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Invoice line charge amount (BT-141) is 2. |
| [BR-DEC-28](https://matheel.app/en/zatca-errors/br-dec) | warning | The allowed maximum number of decimals for the Invoice line charge base amount (BT-142) is |
| [BR-E-01](https://matheel.app/en/zatca-errors/br-e-01) | warning | Exempt (E) items need an 'E' VAT breakdown |
| [BR-E-05](https://matheel.app/en/zatca-errors/br-e-05) | error | Exempt (E) invoice lines must have a VAT rate of 0 |
| [BR-E-06](https://matheel.app/en/zatca-errors/br-e-06) | error | Exempt (E) document allowances must have a VAT rate of 0 |
| [BR-E-07](https://matheel.app/en/zatca-errors/br-e-07) | error | Exempt (E) document charges must have a VAT rate of 0 |
| [BR-E-08](https://matheel.app/en/zatca-errors/br-e-08) | warning | Exempt (E) taxable amount must match the exempt lines and adjustments |
| [BR-E-09](https://matheel.app/en/zatca-errors/br-e-09) | error | VAT amount in an exempt (E) breakdown must be 0 |
| [BR-KSA-03](https://matheel.app/en/zatca-errors/br-ksa-03) | error | Every invoice needs a UUID |
| [BR-KSA-04](https://matheel.app/en/zatca-errors/br-ksa-04) | error | Issue date must not be in the future |
| [BR-KSA-05](https://matheel.app/en/zatca-errors/br-ksa-05) | error | Invoice type code must be 388, 383, 381 or 386 |
| [BR-KSA-06](https://matheel.app/en/zatca-errors/br-ksa-06) | error | Invoice transaction code (name attribute) must follow NNPNESB |
| [BR-KSA-07](https://matheel.app/en/zatca-errors/br-ksa-07) | error | Export invoices cannot be self-billed |
| [BR-KSA-08](https://matheel.app/en/zatca-errors/br-ksa-08) | warning | Seller ID needs a valid scheme (CRN, MOM, MLS, SAG, OTH, 700) |
| [BR-KSA-09](https://matheel.app/en/zatca-errors/br-ksa-09) | warning | Seller address: street, building, postcode, city, district, country |
| [BR-KSA-10](https://matheel.app/en/zatca-errors/br-ksa-10) | warning | Standard invoices need buyer street, city and country code |
| [BR-KSA-11](https://matheel.app/en/zatca-errors/br-ksa-11) | error | 'Not subject to VAT' (O) lines must have a VAT rate of 0 if given |
| [BR-KSA-12](https://matheel.app/en/zatca-errors/br-ksa-12) | warning | 'Not subject to VAT' (O) allowances must have a VAT rate of 0 if given |
| [BR-KSA-13](https://matheel.app/en/zatca-errors/br-ksa-13) | warning | 'Not subject to VAT' (O) charges must have a VAT rate of 0 if given |
| [BR-KSA-14](https://matheel.app/en/zatca-errors/br-ksa-14) | error | Buyer ID must use an allowed scheme ID |
| [BR-KSA-15](https://matheel.app/en/zatca-errors/br-ksa-15) | warning | Standard tax invoices (388) need a supply date |
| [BR-KSA-16](https://matheel.app/en/zatca-errors/br-ksa-16) | warning | Payment means code must be a valid UNTDID 4461 code |
| [BR-KSA-17](https://matheel.app/en/zatca-errors/br-ksa-17) | error | Credit and debit notes need a reason for issuing |
| [BR-KSA-18](https://matheel.app/en/zatca-errors/br-ksa-18) | error | VAT category code must be S, Z, E or O |
| [BR-KSA-19](https://matheel.app/en/zatca-errors/br-ksa-19) | warning | Each document-level charge needs a reason code |
| [BR-KSA-20](https://matheel.app/en/zatca-errors/br-ksa-20) | warning | Each invoice line charge needs a reason code |
| [BR-KSA-21](https://matheel.app/en/zatca-errors/br-ksa-21) | warning | Each document-level charge needs a charge reason text (BT-104) |
| [BR-KSA-22](https://matheel.app/en/zatca-errors/br-ksa-22) | warning | Each invoice line charge needs a charge reason text (BT-144) |
| [BR-KSA-23](https://matheel.app/en/zatca-errors/br-ksa-23) | warning | VAT-exempt (E) breakdown needs an exemption reason code (BT-121) |
| [BR-KSA-24](https://matheel.app/en/zatca-errors/br-ksa-24) | warning | Not-subject-to-VAT (O) breakdown needs a reason code (BT-121) |
| [BR-KSA-25](https://matheel.app/en/zatca-errors/br-ksa-25) | warning | Simplified invoice with EDU/HEA exemption needs the buyer name |
| [BR-KSA-26](https://matheel.app/en/zatca-errors/br-ksa-26) | warning | Previous invoice hash (PIH) attachment must be filled, text/plain |
| [BR-KSA-27](https://matheel.app/en/zatca-errors/br-ksa-27) | warning | Simplified invoices must carry a QR code (KSA-14) |
| [BR-KSA-28](https://matheel.app/en/zatca-errors/br-ksa-28) | error | Signature information ID must be ...ubl:signature:1 |
| [BR-KSA-29](https://matheel.app/en/zatca-errors/br-ksa-29) | warning | Signature ID and referenced signature ID must be ...signature:Invoice |
| [BR-KSA-30](https://matheel.app/en/zatca-errors/br-ksa-30) | error | Signature method must be the enveloped XAdES URN |
| [BR-KSA-31](https://matheel.app/en/zatca-errors/br-ksa-31) | error | Simplified invoices cannot be self-billed (KSA-2 position 7 = 0) |
| [BR-KSA-33](https://matheel.app/en/zatca-errors/br-ksa-33) | error | Every invoice needs an invoice counter value (ICV, KSA-16) |
| [BR-KSA-34](https://matheel.app/en/zatca-errors/br-ksa-34) | error | Invoice counter value (ICV, KSA-16) must contain only digits |
| [BR-KSA-35](https://matheel.app/en/zatca-errors/br-ksa-35) | warning | A supply end date (KSA-24) requires a supply date (KSA-5) |
| [BR-KSA-36](https://matheel.app/en/zatca-errors/br-ksa-36) | warning | Supply end date (KSA-24) must not be before supply date (KSA-5) |
| [BR-KSA-37](https://matheel.app/en/zatca-errors/br-ksa-37) | warning | Seller building number must be exactly 4 digits |
| [BR-KSA-39](https://matheel.app/en/zatca-errors/br-ksa-39) | error | Seller VAT registration number (BT-31) is required |
| [BR-KSA-40](https://matheel.app/en/zatca-errors/br-ksa-40) | error | Seller VAT number (BT-31): 15 digits, starting and ending with 3 |
| [BR-KSA-42](https://matheel.app/en/zatca-errors/br-ksa-42) | warning | Standard tax invoices and their notes need the buyer name (BT-44) |
| [BR-KSA-44](https://matheel.app/en/zatca-errors/br-ksa-44) | error | Buyer VAT number (BT-48): 15 digits, starting and ending with 3 |
| [BR-KSA-49](https://matheel.app/en/zatca-errors/br-ksa-49) | error | EDU/HEA exemption requires buyer national ID (BT-46, scheme NAT) |
| [BR-KSA-51](https://matheel.app/en/zatca-errors/br-ksa-51) | warning | Line amount with VAT (KSA-12) = line net amount + line VAT |
| [BR-KSA-52](https://matheel.app/en/zatca-errors/br-ksa-52) | error | Each line needs a line VAT amount (KSA-11) on standard invoices |
| [BR-KSA-53](https://matheel.app/en/zatca-errors/br-ksa-53) | error | Each line needs a line amount with VAT (KSA-12) on standard invoices |
| [BR-KSA-56](https://matheel.app/en/zatca-errors/br-ksa-56) | warning | Credit and debit notes must reference the original invoice (BT-25) |
| [BR-KSA-60](https://matheel.app/en/zatca-errors/br-ksa-60) | warning | Simplified invoices must include the cac:Signature block (KSA-15) |
| [BR-KSA-61](https://matheel.app/en/zatca-errors/br-ksa-61) | warning | Every invoice needs the previous invoice hash (PIH, KSA-13) |
| [BR-KSA-63](https://matheel.app/en/zatca-errors/br-ksa-63) | warning | Saudi buyer needs a full national address on standard invoices |
| [BR-KSA-66](https://matheel.app/en/zatca-errors/br-ksa-66) | warning | Seller postal code (BT-38) must be exactly 5 digits |
| [BR-KSA-67](https://matheel.app/en/zatca-errors/br-ksa-67) | warning | Saudi buyer postal code (BT-53) must be 5 digits |
| [BR-KSA-68](https://matheel.app/en/zatca-errors/br-ksa-68) | error | Every invoice needs a tax currency code (BT-6) |
| [BR-KSA-69](https://matheel.app/en/zatca-errors/br-ksa-69) | warning | Zero-rated (Z) VAT breakdown needs a reason code (BT-121) |
| [BR-KSA-70](https://matheel.app/en/zatca-errors/br-ksa-70) | error | Invoice issue time (KSA-25) is required as hh:mm:ss or hh:mm:ssZ |
| [BR-KSA-71](https://matheel.app/en/zatca-errors/br-ksa-71) | warning | Simplified summary invoices need the buyer name |
| [BR-KSA-72](https://matheel.app/en/zatca-errors/br-ksa-72) | error | Simplified summary invoices need supply start and end dates |
| [BR-KSA-73](https://matheel.app/en/zatca-errors/br-ksa-73) | warning | Prepaid amount (BT-113) requires full prepayment reference on lines |
| [BR-KSA-74](https://matheel.app/en/zatca-errors/br-ksa-74) | error | Prepayment document type code (KSA-30) on lines must be 386 |
| [BR-KSA-75](https://matheel.app/en/zatca-errors/br-ksa-75) | warning | Prepayment lines need taxable amount, tax amount, category and rate |
| [BR-KSA-76](https://matheel.app/en/zatca-errors/br-ksa-76) | warning | Zero-rated (Z) prepayment line VAT rate must be 0 |
| [BR-KSA-77](https://matheel.app/en/zatca-errors/br-ksa-77) | warning | Exempt (E) prepayment line VAT rate must be 0 |
| [BR-KSA-78](https://matheel.app/en/zatca-errors/br-ksa-78) | warning | Not-subject (O) prepayment line VAT rate must be 0 |
| [BR-KSA-79](https://matheel.app/en/zatca-errors/br-ksa-79) | warning | Line VAT breakdown: tax amount = taxable amount x rate / 100 |
| [BR-KSA-80](https://matheel.app/en/zatca-errors/br-ksa-80) | warning | Prepaid amount must equal prepayment taxable plus tax amounts |
| [BR-KSA-81](https://matheel.app/en/zatca-errors/br-ksa-81) | warning | Standard invoice without buyer VAT number needs another buyer ID |
| [BR-KSA-82](https://matheel.app/en/zatca-errors/br-ksa-82) | error | Prepayment lines must carry zero line amounts |
| [BR-KSA-83](https://matheel.app/en/zatca-errors/br-ksa-83) | warning | Exemption reason code in the VAT breakdown needs a reason text |
| [BR-KSA-84](https://matheel.app/en/zatca-errors/br-ksa-84) | error | Standard-rated (S) VAT rate must be 15 or 5 |
| [BR-KSA-85](https://matheel.app/en/zatca-errors/br-ksa-85) | warning | Buyer phone must start with 0 or + followed by 4 to 15 digits |
| [BR-KSA-86](https://matheel.app/en/zatca-errors/br-ksa-86) | warning | Buyer email should follow the XXX@YYY.ZZ format |
| [BR-KSA-87](https://matheel.app/en/zatca-errors/br-ksa-87) | warning | Tax exchange rate needs source currency, target currency and rate |
| [BR-KSA-88](https://matheel.app/en/zatca-errors/br-ksa-88) | error | Exchange-rate source currency must equal the invoice currency |
| [BR-KSA-89](https://matheel.app/en/zatca-errors/br-ksa-89) | error | Exchange-rate target currency must equal the tax currency |
| [BR-KSA-90](https://matheel.app/en/zatca-errors/br-ksa-90) | error | Tax exchange rate may be at most 14 characters long |
| [BR-KSA-92](https://matheel.app/en/zatca-errors/br-ksa-92) | error | B2G invoices must include a contract ID |
| [BR-KSA-93](https://matheel.app/en/zatca-errors/br-ksa-93) | warning | Zero-rated (Z) line VAT breakdown needs an exemption reason code |
| [BR-KSA-94](https://matheel.app/en/zatca-errors/br-ksa-94) | warning | Exempt (E) line VAT breakdown needs an exemption reason code |
| [BR-KSA-95](https://matheel.app/en/zatca-errors/br-ksa-95) | warning | Not-subject (O) line VAT breakdown needs an exemption reason code |
| [BR-KSA-96](https://matheel.app/en/zatca-errors/br-ksa-96) | warning | Line exemption reason code needs a reason text |
| [BR-KSA-97](https://matheel.app/en/zatca-errors/br-ksa-97) | warning | Non-SAR invoice: SAR VAT total must differ from the VAT total |
| [BR-KSA-98](https://matheel.app/en/zatca-errors/br-ksa-98) | warning | Simplified invoices should be submitted within 24 hours |
| [BR-KSA-99](https://matheel.app/en/zatca-errors/br-ksa-99) | warning | Buyer VAT number must not be empty when the element is present |
| [BR-KSA-CL-01](https://matheel.app/en/zatca-errors/br-ksa-cl-01) | error | Invoice currency code must be a valid ISO 4217 code |
| [BR-KSA-CL-02](https://matheel.app/en/zatca-errors/br-ksa-cl-02) | error | Every amount's currencyID must match the invoice currency |
| [BR-KSA-CL-03](https://matheel.app/en/zatca-errors/br-ksa-cl-03) | error | Attachment mimeCode must be one of the allowed types |
| [BR-KSA-CL-04](https://matheel.app/en/zatca-errors/br-ksa-cl-04) | warning | VAT exemption reason code must fit the VAT category |
| [BR-KSA-CL-06](https://matheel.app/en/zatca-errors/br-ksa-cl-06) | warning | Charge reason codes must come from UNTDID 7161 |
| [BR-KSA-CL-07](https://matheel.app/en/zatca-errors/br-ksa-cl-07) | warning | Prepayment exemption reason code must fit its VAT category |
| [BR-KSA-DEC-01](https://matheel.app/en/zatca-errors/br-ksa-dec-01) | error | Document allowance percentage must be 0-100 with up to 2 decimals |
| [BR-KSA-DEC-02](https://matheel.app/en/zatca-errors/br-ksa-dec-02) | error | VAT rates must be 0-100 with at most two decimals |
| [BR-KSA-DEC-03](https://matheel.app/en/zatca-errors/br-ksa-dec-03) | warning | Line VAT amount may have at most two decimals |
| [BR-KSA-DEC-04](https://matheel.app/en/zatca-errors/br-ksa-dec-04) | warning | Line amount with VAT may have at most two decimals |
| [BR-KSA-DEC-06](https://matheel.app/en/zatca-errors/br-ksa-dec-06) | warning | Prepayment taxable and tax amounts may have at most two decimals |
| [BR-KSA-EN16931-01](https://matheel.app/en/zatca-errors/br-ksa-en16931-01) | error | Business process (ProfileID) must be reporting:1.0 |
| [BR-KSA-EN16931-02](https://matheel.app/en/zatca-errors/br-ksa-en16931-02) | error | VAT accounting currency (TaxCurrencyCode) must be SAR |
| [BR-KSA-EN16931-03](https://matheel.app/en/zatca-errors/br-ksa-en16931-03) | warning | Allowance/charge amount must equal base amount × percentage / 100 |
| [BR-KSA-EN16931-04](https://matheel.app/en/zatca-errors/br-ksa-en16931-04) | warning | An allowance/charge percentage needs a base amount |
| [BR-KSA-EN16931-05](https://matheel.app/en/zatca-errors/br-ksa-en16931-05) | warning | An allowance/charge base amount needs a percentage |
| [BR-KSA-EN16931-06](https://matheel.app/en/zatca-errors/br-ksa-en16931-06) | error | Price-level allowance/charge must have ChargeIndicator false |
| [BR-KSA-EN16931-07](https://matheel.app/en/zatca-errors/br-ksa-en16931-07) | error | Net item price must equal gross price minus price discount |
| [BR-KSA-EN16931-08](https://matheel.app/en/zatca-errors/br-ksa-en16931-08) | warning | Exactly one TaxTotal with tax subtotals per invoice |
| [BR-KSA-EN16931-09](https://matheel.app/en/zatca-errors/br-ksa-en16931-09) | warning | With a tax currency code, add one TaxTotal without subtotals |
| [BR-KSA-EN16931-11](https://matheel.app/en/zatca-errors/br-ksa-en16931-11) | warning | Line net amount = quantity × net price ± line charges/allowances |
| [BR-KSA-EN16931-12](https://matheel.app/en/zatca-errors/br-ksa-en16931-12) | error | Item price base quantity must be greater than zero |
| [BR-KSA-F-01](https://matheel.app/en/zatca-errors/br-ksa-f-01) | error | Dates must use the YYYY-MM-DD format |
| [BR-KSA-F-02](https://matheel.app/en/zatca-errors/br-ksa-f-02) | error | Allowance/charge indicator must be 'false' or 'true' |
| [BR-KSA-F-04](https://matheel.app/en/zatca-errors/br-ksa-f-04) | error | Invoice amounts and quantities must not be negative |
| [BR-KSA-F-05](https://matheel.app/en/zatca-errors/br-ksa-f-05) | warning | Prepayment issue time must be a valid hh:mm:ss time |
| [BR-KSA-F-06-C1](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Invoice number field (BT-1) have not been met. The minimum limi |
| [BR-KSA-F-06-C2](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Invoice note field (BT-22) have not been met. The maximum limit |
| [BR-KSA-F-06-C3](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Purchase order ID field (BT-13) have not been met. The maximum  |
| [BR-KSA-F-06-C4](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Seller Address - Street field (BT-35) have not been met. Th |
| [BR-KSA-F-06-C5](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Contract ID field (BT-12) have not been met. The maximum limit  |
| [BR-KSA-F-06-C6](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Seller Address - Additional street field (BT-36) have not b |
| [BR-KSA-F-06-C7](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Seller Address - City field (BT-37) have not been met. The  |
| [BR-KSA-F-06-C8](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Seller Address - Province/State field (BT-39) have not been |
| [BR-KSA-F-06-C9](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Seller Address - District field (KSA-3) have not been met.  |
| [BR-KSA-F-06-C10](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Seller name field (BT-27) have not been met. The minimum limit  |
| [BR-KSA-F-06-C11](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Buyer Address - Additional street field (BT-51) have not be |
| [BR-KSA-F-06-C12](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Buyer name field (BT-44) have not been met. The minimum limit i |
| [BR-KSA-F-06-C13](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Reasons for issuance of credit / debit note field (KSA-10) have |
| [BR-KSA-F-06-C14](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Payment terms field (KSA-22) have not been met. The maximum lim |
| [BR-KSA-F-06-C15](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Payment account identifier field (BT-84) have not been met. The |
| [BR-KSA-F-06-C16](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for VAT exemption reason text field (BT-120) have not been met. The |
| [BR-KSA-F-06-C17](https://matheel.app/en/zatca-errors/br-ksa-f-06) | error | Field character limits for Invoice line identifier field (BT-126) have not been met. The m |
| [BR-KSA-F-06-C18](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Invoiced quantity unit of measure field (BT-130) have not been  |
| [BR-KSA-F-06-C19](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Item name field (BT-153) have not been met. The minimum limit i |
| [BR-KSA-F-06-C20](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Item standard identifier field (BT-157) have not been met. The  |
| [BR-KSA-F-06-C21](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Item price base quantity unit code field (BT-150) have not been |
| [BR-KSA-F-06-C22](https://matheel.app/en/zatca-errors/br-ksa-f-06) | error | Field character limits for Billing reference ID field (BT-25) have not been met. The minim |
| [BR-KSA-F-06-C23](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Buyer Address - Street field (BT-50) have not been met. The |
| [BR-KSA-F-06-C24](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Buyer Address - Street field (BT-50) have not been met. The |
| [BR-KSA-F-06-C25](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Buyer Address - City field (BT-52) have not been met. The m |
| [BR-KSA-F-06-C26](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Buyer Address - City field (BT-52) have not been met. The m |
| [BR-KSA-F-06-C27](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Buyer Address - Province/State field (BT-54) have not been  |
| [BR-KSA-F-06-C28](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Buyer Address - District field (KSA-4) have not been met. T |
| [BR-KSA-F-06-C29](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Buyer Address - District field (KSA-4) have not been met. T |
| [BR-KSA-F-06-C30](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Item Buyer's identifier field (BT-156) have not been met. The m |
| [BR-KSA-F-06-C31](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Item Seller's identifier field (BT-155) have not been met. The  |
| [BR-KSA-F-06-C32](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Buyer name field (BT-44) have not been met. The maximum limit i |
| [BR-KSA-F-06-C33](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Reason for the document level charge (BT-104) have not been met |
| [BR-KSA-F-06-C34](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Reason for invoice line charge (BT-144) have not been met. The  |
| [BR-KSA-F-06-C35](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Reason for the document level allowance (BT-97) have not been m |
| [BR-KSA-F-06-C36](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Reason for the invoice line allowance (BT-139) have not been me |
| [BR-KSA-F-06-C37](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the contact name - Buyer's contact point field (BT-56) have not |
| [BR-KSA-F-06-C38](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the contact note - Buyer's Contact Note field (KSA-35) have not |
| [BR-KSA-F-06-C39](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for the Prepayment VAT exemption reason text (KSA-42) have not been |
| [BR-KSA-F-06-C40](https://matheel.app/en/zatca-errors/br-ksa-f-06) | warning | Field character limits for Invoice transaction code (KSA-2) have not been met. The minimum |
| [BR-KSA-F-07](https://matheel.app/en/zatca-errors/br-ksa-f-07) | warning | Buyer TIN must be 10 digits starting with 3 |
| [BR-KSA-F-08](https://matheel.app/en/zatca-errors/br-ksa-f-08) | warning | Seller/buyer CRN must be exactly 10 digits |
| [BR-KSA-F-09](https://matheel.app/en/zatca-errors/br-ksa-f-09) | warning | Seller/buyer '700' number must be 10 digits starting with 7 |
| [BR-KSA-F-10](https://matheel.app/en/zatca-errors/br-ksa-f-10) | warning | Buyer national ID (NAT) must be 10 digits starting with 1 |
| [BR-KSA-F-11](https://matheel.app/en/zatca-errors/br-ksa-f-11) | warning | Buyer Iqama number (IQA) must be 10 digits starting with 2 |
| [BR-KSA-F-12](https://matheel.app/en/zatca-errors/br-ksa-f-12) | warning | Seller/buyer ID schemeID must not contain extra spaces |
| [BR-KSA-F-13](https://matheel.app/en/zatca-errors/br-ksa-f-13) | warning | Seller/buyer ID must not be a placeholder digit sequence |
| [BR-O-01](https://matheel.app/en/zatca-errors/br-o-01) | warning | 'Not subject to VAT' (O) items need an O VAT breakdown |
| [BR-O-08](https://matheel.app/en/zatca-errors/br-o-08) | warning | 'Not subject to VAT' (O) taxable amount must match its lines |
| [BR-O-09](https://matheel.app/en/zatca-errors/br-o-09) | error | 'Not subject to VAT' (O) breakdown tax amount must be 0 |
| [BR-S-01](https://matheel.app/en/zatca-errors/br-s-01) | warning | Standard-rated (S) items need an S VAT breakdown |
| [BR-S-05](https://matheel.app/en/zatca-errors/br-s-05) | warning | Standard-rated (S) line VAT rate must be above zero |
| [BR-S-06](https://matheel.app/en/zatca-errors/br-s-06) | error | Standard-rated (S) document allowance VAT rate must be above zero |
| [BR-S-07](https://matheel.app/en/zatca-errors/br-s-07) | warning | Standard-rated (S) document charge VAT rate must be above zero |
| [BR-S-08](https://matheel.app/en/zatca-errors/br-s-08) | warning | Standard-rated (S) taxable amount must match its lines |
| [BR-S-09](https://matheel.app/en/zatca-errors/br-s-09) | warning | Standard-rated (S) tax amount = taxable amount × rate / 100 |
| [BR-S-10](https://matheel.app/en/zatca-errors/br-s-10) | warning | Standard-rated (S) VAT breakdown must not have an exemption reason |
| [BR-Z-01](https://matheel.app/en/zatca-errors/br-z-01) | warning | Zero-rated (Z) items need a Z VAT breakdown |
| [BR-Z-05](https://matheel.app/en/zatca-errors/br-z-05) | error | Zero-rated (Z) line VAT rate must be 0 |
| [BR-Z-06](https://matheel.app/en/zatca-errors/br-z-06) | error | Zero-rated (Z) document allowance VAT rate must be 0 |
| [BR-Z-07](https://matheel.app/en/zatca-errors/br-z-07) | error | Zero-rated (Z) document charge VAT rate must be 0 |
| [BR-Z-08](https://matheel.app/en/zatca-errors/br-z-08) | warning | Zero-rated (Z) taxable amount must match its lines |
| [BR-Z-09](https://matheel.app/en/zatca-errors/br-z-09) | error | Zero-rated (Z) breakdown tax amount must be 0 |

## License

Explanations, titles and structure: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), please link to https://matheel.app/en/zatca-errors.
Official rule texts and conditions are ZATCA's, from the ZATCA E-invoicing SDK 3.4.8. Not affiliated with ZATCA.

Maintained by [Matheel](https://matheel.app/en), a ZATCA Phase 2 e-invoicing API. Corrections welcome via issues.
