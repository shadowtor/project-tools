# Paperwork and promises (10 items)

Source: https://www.murphmaxxing.com/vault/10-ways-to-get-your-app-sued - verify current law and penalties there.

Fix order if you must pick: **6 first** (silent, worsens every hour), then 5 (a broken deletion promise turns a leak into a lawsuit), then 2-4 (the policy is the document regulators compare against your code), then the rest.

| # | Risk | How to check in code | Fails when | Fix |
| --- | --- | --- | --- | --- |
| 1 | No privacy policy | Routes/pages named privacy/legal, footer and signup links | No policy page, or not linked from signup and footer | Write one describing what the app actually does |
| 2 | Policy doesn't cover collected data | List every DB table/column, form field, cookie and local-storage key holding personal data; read the policy text | Any category of personal data collected that the policy never mentions | One sentence per category of data, generated from the schema |
| 3 | Policy doesn't disclose AI use | Grep SDK imports and API hosts (OpenAI, Anthropic, Gemini, Mistral, etc.), prompt-building code | User input goes to a model provider and the policy is silent | Name the provider, what is sent, and whether it is used for training |
| 4 | Third parties not named | Script tags, SDK imports, network calls: analytics, error tracking, ad pixels, email, payments, chat widgets | A domain data goes to that the policy doesn't list | Reconcile the list of domains with the policy; add or remove |
| 5 | Promised deletion not enforced | Find every "we delete X after Y" claim; look for the job/TTL/lifecycle rule that does it | Policy promises deletion and no scheduled job or storage lifecycle exists, or it never ran | Write the deletion job and a check that it ran |
| 6 | **Silent:** storage/database public | Firebase rules, Supabase RLS and bucket visibility, S3/R2 policies and public URLs, `allow read: if true`, public ACLs, signed-URL use | Any bucket/table readable without auth that holds user data; RLS off | Default-deny everywhere, scope to owner, test from a logged-out private window |
| 7 | Fake testimonials | Testimonial/review components, hard-coded quotes, seed data, AI-generated names | Reviews not tied to a real account, or team/employee reviews unlabelled, or low ratings filtered out while claiming to show all | Real person per review; label "from our team"; don't hide negatives |
| 8 | Cancelling harder than signing up | Count taps from signup to paid vs. from account to cancelled; look for "contact us to cancel" | Cancel needs a human, phone/chat, or more steps than signup | Self-serve cancel with the same number of taps as signup |
| 9 | Trial auto-charges with no reminder | Checkout/trial UI copy, billing webhook, reminder email job | Price, cadence, renewal date or cancel path not on the same screen as the card field, or no pre-charge reminder | Show them together; email before the first charge |
| 10 | Companion chatbot without self-harm response | Chat feature with memory across sessions, system prompt, crisis keyword handling, age gate | No "I'm not human" disclosure on first message, no crisis routing, no age gate | Disclosure on first message, keyword-triggered crisis routing, age gate first |

Items 1-4 and 7 can be partly checked from the live policy and terms alone if the code isn't available: ask the user to paste them plus a description of what the app does.
