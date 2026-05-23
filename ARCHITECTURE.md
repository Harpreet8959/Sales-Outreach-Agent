# System Architecture

## End-to-End Flow

```mermaid
flowchart TD
    subgraph EXT["🌐 Chrome Extension"]
        A1["content.js\nDOM scraping"] --> A2["background.js\nMessage relay"]
        A2 --> A3["popup.js\nApprove / Edit / Reject"]
    end

    subgraph BACK["⚡ FastAPI Backend"]
        B1["POST /chats/ai-response\nrouters/chats.py"]
    end

    subgraph PIPELINE["🤖 AI Pipeline — 5-Stage State Machine"]
        P1["1 · State detection\nTurn count + last sender"]
        P2["2 · Parallel execution\nIntent + portfolio + embed"]
        P3["3 · Intent lock\nConfidence ≥ 0.65"]
        P4["4 · RAG retrieval\nSemantic or metadata-only"]
        P5["5 · Playbook lookup\nIntent × state rules"]
        P6["6 · LLM generation\ngpt-4o-mini · temp=0"]
        P7["Parse + validate\nJSON schema"]
        P8["Save to DB\nChat + audit log"]
        P9["Embed + cache\nChromaDB update"]

        P1 --> P2 --> P3 --> P4 --> P5 --> P6 --> P7
        P7 --> P8
        P7 --> P9
    end

    subgraph STORES["🗄️ Data Stores"]
        D1["PostgreSQL\nChat · Client · RAG docs"]
        D2["ChromaDB\nMain + portfolio collections\nRAM cache · 5 min TTL"]
        D3["External APIs\nOpenAI · Zoho CRM · Calendly"]
    end

    EXT -->|"Scraped conversation"| BACK
    BACK --> PIPELINE
    PIPELINE --> STORES
    PIPELINE -->|"AI response"| EXT
```

---

## Intent Classification

```mermaid
flowchart TD
    MSG["Client message"] --> LLM["gpt-4o-mini intent classifier"]
    LLM --> POS["✅ Positive\nCV request / active engagement"]
    LLM --> NEG["❌ Negative\nExplicit rejection"]
    LLM --> OBJ["⚠️ Objection\nLocation / budget / timing"]
    LLM --> NEU["😐 Neutral\nPassive acknowledgment"]
    LLM --> HIR["🎉 Hired someone\nRole already filled"]
    POS & NEG & OBJ & NEU & HIR --> PRI["Priority resolution\nOBJECTION > HIRED > NEGATIVE > POSITIVE > NEUTRAL"]
```

---

## Conversation State Machine

```mermaid
stateDiagram-v2
    [*] --> campaign_message : CEO sends first message
    campaign_message --> client_response : Client replies
    client_response --> 1st_message : CEO replies
    1st_message --> 1st_followup : No new client reply
    1st_followup --> 2nd_followup : CEO follows up
    2nd_followup --> 3rd_followup : CEO follows up
    3rd_followup --> 4th_followup : CEO follows up
    4th_followup --> final_message : CEO sends last message
    1st_followup --> client_response : Client re-engages
    2nd_followup --> client_response : Client re-engages
    3rd_followup --> client_response : Client re-engages
    client_response --> hired_congrats : Intent = hired_someone
    client_response --> double_objection : 2nd objection detected
    hired_congrats --> [*]
    double_objection --> [*]
    final_message --> [*]
```

---

## RAG Retrieval Logic

```mermaid
flowchart TD
    START["smart_retrieve()"] --> CACHE{"Cache hit?\n5 min TTL"}
    CACHE -->|"Yes"| RETURN["Return cached examples"]
    CACHE -->|"No"| FU{"Follow-up state?"}
    FU -->|"Yes — client msg stale"| META["Metadata-only retrieval\nChromaDB .get() with filters"]
    FU -->|"No — fresh client msg"| SEM["Semantic search\nChromaDB .query() + embedding"]
    SEM --> FB{"Results found?"}
    FB -->|"No"| META
    FB -->|"Yes"| NORM["normalize_state()\nMap code state → ChromaDB names"]
    META --> NORM
    NORM --> OUT["Return formatted examples\nwith retrieval_method tag"]
```

---

## Performance

| Metric | Value |
|--------|-------|
| End-to-end latency | ~5 seconds |
| Daily conversations | 100+ |
| Intent detection accuracy | 90%+ |
| CRM duplicate contacts | 0 |
| Downtime since launch | 0 |

> ⚠️ Source code is proprietary. This repo contains architecture documentation only.
