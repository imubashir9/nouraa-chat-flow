# Autonomous Healthcare Scheduling Agent

## 📌 Project Overview
This repository contains the architectural blueprint and business logic orchestration for an autonomous AI voice/chat agent designed for **City General Hospital**. Built using the Nouraa AI platform, this agent manages complex, multi-turn patient interactions, handles dynamic intent routing, queries a native Knowledge Base (RAG), and executes functional API calls for calendar booking.

The visual flow represents the transition from basic NLU (Natural Language Understanding) to a fully functional, agentic business layer.

## 📂 Repository Contents
* `Nouraa-chat-flow.jpg`: The visual blueprint of the agent's logic nodes and conversational branching.
* `agent-Mubashir-s-2026-04-10.json`: The raw JSON export of the agent's configuration, detailing the OpenAI GPT-4o parameters, variable extraction schemas, and tool-calling definitions (sensitive IDs and phone numbers have been redacted).

## 🏗️ System Architecture & Flow Design

The provided architecture diagram (`flow-architecture.png`) demonstrates a robust conversational pipeline divided into four primary skill sets:

### 1. Dynamic Triage & Intent Routing
Instead of relying on rigid decision trees, the system uses LLM-powered intent classification to seamlessly route users from the main greeting node into distinct logical paths:
* **Information Retrieval:** General FAQs (hours, policies) routed to the RAG Knowledge Base.
* **Appointment Scheduling:** Directs to the booking pipeline.
* **Human Handoff:** Identifies frustration, critical emergencies, or explicit requests for an operator, routing to a Cold Transfer node.

### 2. Patient Onboarding (New vs. Returning)
The flow features a highly natural NLU branching system to minimize user friction:
* **Zero-Variable Branching:** The agent listens to the user's natural language to determine patient status without forcing keyword inputs. 
* **Conditional Logic:** New patients are routed to an onboarding script to collect comprehensive profiles, while returning patients are fast-tracked to verification.

### 3. Entity Validation & Slot Extraction
The central "Extract Variables" engine actively listens and parses unstructured user dialogue to fill necessary data slots before moving forward:
* `name`, `email`, `phone`
* `date` (Standardizes natural language timeframes into actionable datetime formats)
* `insurance` (Cross-referenced against the hospital's accepted networks)
* `reason` (Chief complaint capture)

### 4. Advanced Logic & API Triggers
* **Function Calling:** Once all slots are validated, the flow triggers the `mubashir_appointment_booking` function node, executing a webhook/API call to securely push the payload to the external calendar/CRM system.
* **Error Handling & Fallback:** Unrecognized responses or dropped inputs trigger a localized "Recovery Loop," politely prompting the user for clarification before re-attempting extraction, ensuring zero conversational dead-ends.

## 🧠 Knowledge Integration (RAG)
The agent is natively integrated with a localized Knowledge Base containing specific hospital data (Hours, Services, Insurance Accepted, Emergency Guidelines). 
* **Global Context:** Semantic search evaluates user inquiries in real-time. If a user asks a policy question mid-booking, the agent retrieves the fact, delivers the answer, and seamlessly returns to the scheduling flow state without losing variables.

## 🚀 Key Achievements
* Achieved human-like conversational pacing by manipulating node response settings (`Skip Response` protocols) to prevent AI-driven question stacking.
* Implemented strict emergency guidelines, allowing the NLU to override standard booking flows and deliver immediate 911/ER redirection upon detecting critical symptom keywords.
* Successfully orchestrated the "business logic layer," bridging the gap between a conversational LLM and actionable enterprise data systems.
