# Streamlit Widget To Processing One-Pager

Project: insurance-agent
Primary UI file: app/main.py

## 1) Widget Inventory (UI Field -> Code Location)

Manual Entry tab (starts at app/main.py:207)

- Claim ID field
  - Widget: st.text_input("Claim ID *")
  - Location: app/main.py:213
  - Variable: claim_id

- Policy Holder Name field
  - Widget: st.text_input("Policy Holder Name *")
  - Location: app/main.py:214
  - Variable: policy_holder

- Vendor or Service Provider field
  - Widget: st.text_input("Vendor/Service Provider *")
  - Location: app/main.py:215
  - Variable: vendor_name

- Total Claim Amount field
  - Widget: st.number_input("Total Claim Amount ($) *")
  - Location: app/main.py:218
  - Variable: total_amount

- Invoice Item Description field (dynamic list)
  - Widget: st.text_input("Item Description", key=f"item_desc_{i}")
  - Location: app/main.py:230
  - Variable: item_desc

- Invoice Item Amount field (dynamic list)
  - Widget: st.number_input("Amount ($)", key=f"item_amount_{i}")
  - Location: app/main.py:233
  - Variable: item_amount

- Remove Invoice Item button
  - Widget: st.button("❌", key=f"remove_{i}")
  - Location: app/main.py:236
  - Behavior: removes one item from st.session_state.invoice_items and reruns

- Add Another Item button
  - Widget: st.button("➕ Add Another Item")
  - Location: app/main.py:242
  - Behavior: appends blank item to st.session_state.invoice_items and reruns

- Process Claim submit button
  - Widget: st.button("🚀 Process Claim", type="primary")
  - Location: app/main.py:248
  - Behavior: validates inputs, builds claim_data payload, calls process_claim

Upload JSON tab (starts at app/main.py:309)

- JSON file upload field
  - Widget: st.file_uploader("Choose a JSON file", type=["json"])
  - Location: app/main.py:312
  - Variable: uploaded_file

- Process Uploaded Claim button
  - Widget: st.button("🚀 Process Uploaded Claim", type="primary")
  - Location: app/main.py:321
  - Behavior: parses JSON and calls process_claim

Logs Viewer tab (starts at app/main.py:352)

- Log file dropdown selector
  - Widget: st.selectbox("Select log file", files)
  - Location: app/main.py:366
  - Variable: selected

- Download selected log file
  - Widget: st.download_button("Download Log File", ...)
  - Location: app/main.py:379

## 2) Field Crosswalk (UI -> Payload -> Graph State -> Outcome)

- Claim ID
  - UI source: app/main.py:213 (claim_id)
  - Payload key: claim_data["claim_id"] at app/main.py:260
  - Graph state after parse: state["claim_id"] in app/agent/graph.py:29
  - Used by: validation, recommendation, finalize decision nodes
  - Included in result display: app/main.py:290

- Policy Holder Name
  - UI source: app/main.py:214 (policy_holder)
  - Payload key: claim_data["policy_holder"] at app/main.py:261
  - Graph state after parse: state["policy_holder"] in app/agent/graph.py:30
  - Used by: validation node input claim_data in app/agent/graph.py:52

- Vendor Name
  - UI source: app/main.py:215 (vendor_name)
  - Payload key: claim_data["vendor_name"] at app/main.py:262
  - Graph state after parse: state["vendor_name"] in app/agent/graph.py:31
  - Used by: query generation and recommendation nodes

- Invoice Items
  - UI source: app/main.py:230 and app/main.py:233
  - Session storage: st.session_state.invoice_items in app/main.py:225 and app/main.py:240
  - Payload key: claim_data["invoice_items"] at app/main.py:263
  - Graph state after parse: state["invoice_items"] in app/agent/graph.py:32
  - Used by: query generation and recommendation nodes

- Total Amount
  - UI source: app/main.py:218 (total_amount)
  - Payload key: claim_data["total_amount"] at app/main.py:264
  - Graph mapped amount: state["claim_amount"] in parse node app/agent/graph.py:33
  - Used by: validation, query generation, recommendation, price check nodes

## 3) Submit And Execution Path

Manual Entry path

1. User clicks Process Claim at app/main.py:248.
2. Required field and amount validation runs at app/main.py:250 and app/main.py:254.
3. claim_data dictionary is assembled at app/main.py:259.
4. process_claim(claim_data) called at app/main.py:269.
5. process_claim creates initial_state with JSON string at app/main.py:152.
6. claims_graph.invoke(initial_state) runs workflow at app/main.py:162.
7. Result fields are rendered in success or error boxes plus detailed JSON at app/main.py:278 onward.

Upload JSON path

1. User uploads file at app/main.py:312.
2. json.load(uploaded_file) creates claim_data at app/main.py:316.
3. User clicks Process Uploaded Claim at app/main.py:321.
4. process_claim(claim_data) follows same graph path as manual entry.

## 4) Graph Routing Summary

Graph builder: app/agent/graph.py:188

Node order and routing:

1. parse_claim -> validate_claim (app/agent/graph.py:208)
2. Conditional route after validate_claim (app/agent/graph.py:211):
   - continue -> generate_queries
   - invalid -> invalid_claim
3. continue path then runs:
   - generate_queries -> retrieve_policy -> recommendation -> price_check -> finalize_decision -> END
   - edges declared at app/agent/graph.py:220 to app/agent/graph.py:224
4. invalid path:
   - invalid_claim -> END at app/agent/graph.py:225

State schema reference: app/agent/state.py:5
Input model reference: app/agent/state.py:43

## 5) Safe Scope Statement

This one-pager is documentation only. No source files in app were modified.
