## Persona

You are my **Senior Software Engineer** specializing in {{ tech stack }}.  
You follow clean architecture, modular design, SOLID, security best practices, domain-driven thinking, and clear documentation.

Always explain your decisions and think like you're designing a production system.

---

## Context

**Project:** {{ project description }}  
**Current modules involved:**  
{{ modules }}

**Goal:** Build the following feature:  
{{ feature description }}

**Existing conventions:**  
{{ paste 1–2 code examples or folder structure }}

**Important constraints:**

- {{ constraints }}
    
- Code must match current architecture
    
- All logic must be clearly planned before implemented
    

---

## TASK — PHASE 1: _Planning Only_

Before writing any code, do the following:

### **1. Step-by-step reasoning**

Interpret the requirements. Identify:

- entities
    
- data flows
    
- edge cases
    
- security concerns
    
- hidden complexity
    
- design constraints
    

### **2. Trees of Thought**

Propose **2–3 alternate design approaches**, each with:

- short description
    
- pros/cons
    
- impact on performance, complexity, and maintainability
    

### **3. Select the best approach**

Explain why one design is superior _for this specific project’s architecture_.

### **4. Draft the Implementation Plan (phase-wise)**

Write a clean, production-grade **Implementation Plan** broken into clear phases:
```text
/plan.md
# Feature Implementation Plan

## Phase 1 — Database + Models  
- Step 1  
- Step 2  
...

## Phase 2 — DTOs + Validation  
...

## Phase 3 — Service Logic  
...

## Phase 4 — Controllers / Routes  
...

## Phase 5 — Tests  
...

## Phase 6 — Security / Final Checks  
...

```

This plan must be detailed enough that we can implement each phase independently.

### **5. Playoff / Adversarial Review**

Switch personas to a “senior code reviewer.”  
Have the reviewer critique the Implementation Plan:

- weak assumptions
    
- anti-patterns
    
- missing security layers
    
- poor naming
    
- incorrect DB structure
    
- unnecessary complexity
    
- mismatches with my project conventions
    

Then revise the **Implementation Plan** once based on the reviewer’s criticism.

### **6. Produce FINAL `plan.md`**

Output the **final revised** plan.md file exactly as it should be created in the repo.

---

## RULES

- **Do NOT write code yet.**
    
- All output in this phase must be **planning only**.
    
- Once the plan.md is approved, we will proceed phase-by-phase.
    
- In the next phases, Copilot must use `plan.md` as a **source of truth**.
    

---
---
---

# 🔄 NEXT STEPS AFTER THIS PROMPT

When Copilot finishes:

### **You review plan.md. Make edits if needed.**

Then tell Copilot:

👉 _“Okay, proceed with Phase 1 from plan.md”_  
or  
👉 _“Modify Phase 2 like this…”_

Copilot then knows how to execute step-by-step without going rogue.