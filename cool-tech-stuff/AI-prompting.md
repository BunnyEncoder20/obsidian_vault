This is a good skill to have in today day and age
[Watch this video for context](https://youtu.be/pwWBcsxEoLk?si=7tzN82FGV2K-1J71)
This video explains how to improve your AI prompting skills by understanding that a **prompt is a program, not just a question** (2:10). The creator highlights several key techniques:

# Prompting Key Techniques

- **Personas** (4:12): Assigning a specific role or expertise to the AI helps narrow its focus and produce more targeted responses.
- **Context** (6:48): Providing detailed and specific information is crucial to prevent the AI from "hallucinating" or making up information. The video emphasizes "Always Be Contexting" (9:57).
- **Format** (10:56): Clearly defining the desired output format, including length, tone, and structure, ensures the AI delivers results exactly as needed. The concept of **few-shot prompting** (12:35), where examples are provided, is introduced as a way to show the AI exactly what "good" looks like.
- **Advanced Techniques** (13:36):
    - **Chain of Thought (CoT)** (13:45): Instructing the AI to "think step by step" before answering significantly improves accuracy and builds trust by showing its reasoning process.
    - **Trees of Thought (ToT)** (15:22): This technique allows the AI to explore multiple paths and self-correct, leading to more diverse and optimal solutions.
    - **Playoff Method/Adversarial Validation** (16:26): This involves setting up a "competition" between different AI personas to critique and refine responses, leveraging the AI's strength in editing over original writing.

The **meta-skill** (17:41) underlying all these techniques is **clarity of thought**. The video argues that if you can't clearly explain what you want, the AI won't be able to provide it. Improving your prompting skills ultimately means improving your ability to think and communicate clearly.

---
## Example Prompt:

**Persona:**
You are a **senior backend engineer specializing in NestJS, Prisma, and clean architecture**. You are strict about modularity, DTO validation, security, and scalable DB design.

**Context:**  
I am adding a new feature:

- Users can request a password change by submitting a new password.
    
- Admins see these requests in their dashboard.
    
- Admin can approve or reject.
    
- If approved → update password in `User` table.
    
- If rejected → mark request as rejected.
    
- Currently, no table exists for password-change requests.
    

Tech stack: NestJS + Prisma + PostgreSQL.

I need help designing **everything**: database schema, DTOs, service functions, and controller endpoints — but I want the solution to match my current architecture conventions.

Here is an example of my **current style for module structure** (few-shot):
```bash
/src
  /user
    user.module.ts
    user.controller.ts
    user.service.ts
    dto/
      create-user.dto.ts
      update-user.dto.ts
    entities/
      user.entity.ts

```

**Task:**  
Generate the complete implementation for a new `password-approval` feature including:

1. **DB schema (Prisma)** – including enums, relations, and constraints
    
2. **DTOs** – include class-validator decorators
    
3. **Service layer structure** – with method signatures + step-by-step logic
    
4. **Controller endpoints** – for both user and admin
    
5. **Routing suggestions & module organization**
    
6. **Security considerations** – what to watch out for (e.g., hashing, race conditions)
    

**Chain of Thought:**  
Think step-by-step through the requirements, reason about edge cases, auth roles, and constraints before producing your final code.

**Trees of Thought:**  
Explore at least **two alternate DB schema designs**, compare them briefly, and choose the better one.

**Playoff Method:**  
Generate two versions of the service method design:

- Version A: A single service that handles all request/approval logic
    
- Version B: Split into a RequestService and AdminApprovalService
    

Then pick the better design and justify it.

**Output:**  
Provide:

- Prisma schema
    
- DTOs
    
- Service code
    
- Controller code
    
- File structure text tree
    
- Notes on security & validation best practices
    

Keep code clean and production-ready. Avoid fluff.


### Why this works 

| **Technique**            | **How it’s used**                                                 |
| ------------------------ | ----------------------------------------------------------------- |
| **Persona**              | Enforces expert-like responses tailored to NestJS + Prisma.       |
| **Always Be Contexting** | You gave project structure, requirement details, current style.   |
| **Format**               | You defined: schema, DTOs, services, controllers, file structure. |
| **Few-shot**             | You shared your folder layout → AI copies your style.             |
| **CoT**                  | “Think step-by-step” ensures correctness & safety.                |
| **ToT**                  | “Explore 2 DB schemas + compare” → avoids bad designs.            |
| **Playoff Method**       | AI produces 2 competing designs → picks best → higher quality.    |
| **Meta-skill: Clarity**  | The entire prompt forces explicit clarity → best possible output. |


---

## [[Dev Prompt Template]]

Project prompt template optimized for VS code copilot. Fill in the blanks `{{}}` and copy and paste for good code gen:

```text
Persona:
- You are my **senior software engineer** specializing in:
- **{{ tech stack }}**  (e.g., NestJS, Prisma, PostgreSQL, React, Docker, AWS, etc.)
- Clean architecture, modular design, SOLID principles
- Secure coding, validation, error handling
- Production readiness & scalability: Act like you are reviewing and building code with me.

---

Context:
Here’s what we are working on:
- Project: {{ project description }}  
- Modules involved: {{ relevant modules/folders }}  
- Goal: {{ what feature or fix you want }}
- Current code style conventions: {{ paste 1–2 examples of actual files OR folder structure }}
- Important constraints:
	- {{ constraints like security, performance, backward compatibility, conventions }}

---
   
Tasks
Help me design and implement this feature end-to-end.  
You must output:
1. Phase wise Architecture plan
2. Updated file structure
3. Database schema / models (if relevant)
4. DTOs / types / interfaces (if relevant)
5. Service / business logic
6. Controllers / routes / APIs
7. Unit test outline (optional but preferred)
8. Security considerations
9. Edge cases & failure scenarios
10. Migration or deployment notes (if needed)
Everything must follow my existing project style and folder structure.

---

Think Before you code
Do NOT jump into code immediately. First produce in order:
1. Step-by-step reasoning about requirements and phases (short, concise) 
2. Two alternative designs (Trees of Thought)
3. Pros/cons comparison
4. Choose the best approach and justify
5. Playoff Method / Adversarial Check: After generating your solution, simulate a second senior engineer ("Reviewer/CTO Persona") who critiques it. Then refine the solution once (if needed)

Produce the Approch and Implementation plan in a plan.md file which I can review and make suggestions/corrections to. This file you can also view to see what is next to be implemented.

Once that is approved, Then generate the final code.

```

