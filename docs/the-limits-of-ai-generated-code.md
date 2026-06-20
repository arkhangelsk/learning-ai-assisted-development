# The Limits of AI-Generated Code

AI often produces code that looks professional and plausible, even when it contains mistakes. These errors can be subtle and may not cause immediate failures, making them particularly dangerous. Let's explore some common failure modes of AI-generated code and how to defend against them.

**a. Hallucinated APIs**

* AI may invent functions, methods, parameters, or options that do not exist.
* Code may look correct because it follows naming conventions and patterns.
* Verify unfamiliar APIs against official documentation.
* Ask the AI to cite or justify its usage.

---

**b. Outdated Patterns**

* AI may generate code based on older versions of libraries or APIs.
* It may suggest deprecated approaches or patterns with limited support.
* Always check current documentation and compatibility requirements.

---

**c. Subtle Logic Errors**

* The code compiles and runs but fails in real-world scenarios.
* AI often handles the happy path but misses edge cases.

---

**d. Security Gaps**

* Missing input validation.
* Potential SQL injection vulnerabilities.
* Weak authentication or authorization logic.
* Unsafe handling of user data.

---

**e. False Confidence**

* AI may confidently explain incorrect information.
* Asking AI to verify itself is not always sufficient because it can reinforce its own mistakes.

---

## How to Defend Against These Issues

### Read Every Line

* Review generated code carefully.
* Don't assume correctness because the code looks clean.

### Test Edge Cases

Consider:

* Empty input
* Invalid input
* Malicious input
* Very large input
* Boundary conditions

### Verify APIs

* Check official documentation (e.g., MDN, vendor docs).
* Especially important for unfamiliar methods and parameters.

### Challenge the AI

Ask:

* "Are you sure this API exists?"
* "Show me the documentation signature."
* "What assumptions are you making?"
* "What edge cases does this implementation fail on?"

AI can often self-correct when challenged.

### Run and Break the Code

Before committing:

* Compile it
* Execute it
* Write tests
* Intentionally try to break it

---

## Important Lesson

Every AI mistake is a learning opportunity.

When AI produces incorrect code:

1. Identify the failure mode.
2. Understand why it failed.
3. Learn the limitation.
4. Improve future prompts and reviews.

---

## Key Takeaway

AI is excellent at accelerating development, but **validation remains a human responsibility**. The highest-risk errors are not syntax errors that fail immediately, but **plausible-looking code that appears correct while hiding API mistakes, logic flaws, security vulnerabilities, or edge-case failures.**
