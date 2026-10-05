**Week 1 — [11/09/2026]**

--> Spent learning weeks 1-7 on labs and repo setup but hadn't locked in a project direction or started building. Starting the build now with roughly 4-5 weeks to the deadline.

--> Plan from here: Deciding on a problem , then API → tool → data → Gradio → tests, one real commit per session.



**Problem for project:[Budgeting Assistant for Monthly Budgeting]** 


A budgeting assistant for a super managing monthly budget across desired spending categories (e.g.rent, groceries, transport, entertainment, and subscriptions) -> The user will enter their income and their planned budget limit for each spending categories, then logs actual spending within the process. -> Expectations: The app will show the user category-by-category and their current financial situations whether they're (ON BUDGET, OVERBUDGET, UNDERBUDGET), following with a conversational assistant that are aware of the numbers and financial circumstances and will be available to ask questions and give financial advices.

**Specific plan and objective:**


University students, especially international students living out of home often face tight, and irregular financial income from part-time jobs, support from families, and even government financial services. Being an international student with freedom of spending tends to find it easy to lose track of discrete spending, especially in food and entertainment. This could be a tumbling issue when high-cost expenses such as rent or bills are due. 

Thus, this financial assistant allows students to enter their income and budget limits for each category while continuously updating their spending history in the process to display a clear and unbiased view of their financial position. Moreover, genuine conversational financial advice services by the budgeting assistant are given based on the actual data and numbers (Using Google API). By this, students would be given the opportunity of a more controlled financial planning to only spend within the budget limit, if not, solutions to stabilise the overbudget spending will be referred by the financial assistant.

**AI DECLARATION:**


--> AI was used to provide ideas for potential problems users may face, in this case inconsistent and uncontrolled spending of international students due freedom of spending and income flow from family support, part-time, government financial help

.
.

**Week 1 - [12/09/2026]**

**Starting on the six-method planning: (WRITTEN IN Six_step_planning Notebook)**


STEP 1: Understanding the problem 
STEP 2: Decided on expected input and output
STEP 3: Creating an example desired input and output and status by hand (as a known answer test case for final check)
   
**Anything I found and fixed**


 --> Miscalculated the remaining values, however fixed with the help of AI checking

**AI DECLARATION:**


--> [Paste first finished 3 method of planning] Check whether the structure and content of the planning is on point and correct.

**What it returned:** Pointed out corrections to be made based on values that are miscalculated 

.
.

**Week 1 - [13/09/2026]**

STEP 4: Creating a Pseudocode (4th method): Plain-logic for the problem [seen in collab]

def calculate_budget_status(budgets, spending):
    results = {}   

    for category in budgets:
        budget_amount = budgets[category] 


        if budget_amount <= 0:
            raise ValueError(f"Budget for {category} must be greater than zero")


        spent_amount = spending.get(category, 0)     
        if spent_amount < 0:
            raise ValueError(f"Spending for {category} cannot be negative")

   
        remaining = budget_amount - spent_amount
        percent_used = (spent_amount / budget_amount) * 100

        if spent_amount > budget_amount:
            status = "overbudget"
        elif percent_used >= 80:
            status = "close to limit"
        else:
            status = "on track"

  
        results[category] = {
            "spent": spent_amount,
            "remaining": remaining,
            "percent_used": percent_used,
            "status": status
        }

    return results

def summarise_overall(budgets, spending, income):
    if income < 0:
        raise ValueError("Income cannot be negative")

    total_budgeted = sum(budgets.values())
    total_spent = sum(spending.values())
    unspent_from_income = income - total_spent
    unused_budget = total_budgeted - total_spent

    return {
        "total_budgeted": total_budgeted,
        "total_spent": total_spent,
        "unspent_from_income": unspent_from_income,
        "unused_budget": unused_budget

    }

**Anything rejected or fixed:** 


--> When running the finished Pseudocode, an error message appear stating that the error came from "raise error" keyword. An easy fix, the issue was focused mainly on indentation.

**AI DECLARATION:**


--> Based on the problem or scenario written, write a relevant pseudocode as a reference


.
.

**Week 2 - [19/09/2026]** 

**What I was trying to do:** 


Finish converting my Step 4 pseudocode into working Python (Step 5), and write proper tests for it (Step 6) — both required parts of the six-step method (R6), and the testing also covers R5 and reinforces R3 (custom tool handling bad input).

**AI declaration:**


--> Used Claude throughout this session to help translate pseudocode into Python syntax, explain line-by-line what the code does, and help design and write assert-based tests, including error-case tests using try/except.

**Prompt/question given:** Asked Claude to convert my pseudocode for calculate_budget_status() and summarise_overall() into real Python, then asked it to help me write tests. First checking normal cases against my Step 3 worked example, then edge cases (no spending logged) and error cases (zero budget, negative spending, negative income).

**What it returned:** Two working Python functions matching my pseudocode's logic, plus a set of print-based checks, which I then converted into proper assert statements, including try/except blocks to test that my ValueError guard clauses raise the correct error with the correct message.

**Anything rejected or fixed:** 
1. Found a floating-point display issue, Entertainment's percent_used printed as 114.99999999999999 instead of 115.0, a binary rounding artifact. Fixed by wrapping display values with round(x, 1), and used the same rounding inside the relevant assert to avoid a false failure 
from exact-equality comparison on a near-115 float.

2. Accidentally mislabelled my Python code cell as "STEP 4: PSEUDOCODE"  instead of "STEP 5: PYTHON IMPLEMENTATION", caught this while 
reviewing my own notebook structure and corrected the heading.
  
3. Tried running my actual pseudocode (the plain-English version) directly as a code cell out of curiosity/confusion, which correctly produced a SyntaxError ("function" isn't a real Python keyword). This helped confirm my understanding of why pseudocode belongs in a markdown cell, not a code cell.

**Result:** All normal-case and edge/error-case tests pass. Six-step method now fully evidenced (Steps 1–6 complete for the custom budget tool), with R3, R5, and R6 substantially covered.

.
.
  
**Week 3 - [21/09/2026]**

**What I was trying to do:** Set up the Gemini API connection (R1) using requests, with a persona and system instruction, and confirm it handles both on-topic and off-topic questions sensibly.

**AI declaration:** 


--> Used Claude to help write the ask_gemini() wrapper function using the requests library, confirm the correct Gemini REST 
endpoint format, and design the persona system instruction.

**What I tested:** Asked the assistant a grocery-saving question (on-topic) and "what's the capital of France?" (off-topic), using the same persona instruction for both.

**What it returned:** A detailed, well-structured grocery-saving response in a warm coaching tone (ethnic markets, cheap staples, batch cooking, etc.), and for the off-topic question, a brief, polite answer followed by a redirect back to budgeting: "my real specialty is helping you navigate student life on a budget... Is there a money question..."

**What I kept:** The full persona instruction and the wrapper function as designed where both worked as intended on the first real test.

**Anything I found and fixed:**


--> Hit a 503 "model currently experiencing high demand" error on my first attempt, not a bug in my code, since the error was Google's server being temporarily overloaded. My error handling (checking response.status_code) caught it cleanly and gave a clear message rather than crashing confusingly. Re-ran the same cell a minute later and it succeeded.

.
.


**WEEK 4 — [28/09/2026]**

**What I was trying to do:** Make the assistant answer using my real budget numbers (R2), make the Gemini calls more robust, and build the first version of the Gradio interface (R4).

**AI declaration:** 


--> Used Claude to help write the retry logic in ask_gemini(), the build_budget_summary() and ask_gemini_grounded()
functions, and the first version of the Gradio interface code. I asked for line-by-line explanations of each piece and ran everything myself.

**Prompt/question given:** Asked how to feed my budget results into the Gemini prompt so replies use my actual numbers, then how to handle the API errors I hit, then how to build a Gradio interface around my existing functions.

**What it returned:**
- build_budget_summary(), which formats the output of my two existing functions into a text block, and ask_gemini_grounded(), which adds that block and the user's question to the prompt.
- A retry loop for 503 errors in ask_gemini().
- A Gradio app with number inputs, a "Check My Budget" button and an "Ask" button, with try/except around my ValueError cases.

**What happened when I tested it:**


- Grounded reply: asked "Am I doing okay this month?" with my Step 3 numbers. The reply used my real figures ($2,500 income, $1,074 spent)
  and noticed that $900 of rent was still unpaid.<img width="1278" height="309" alt="Screenshot 2026-09-28 at 13 32 40" src="https://github.com/user-attachments/assets/42e24080-ca0e-4638-b7ef-d37a4cc03b08" />

  
- 503 errors: hit "model currently experiencing high demand" several times. My status_code check surfaced a clear message each time. I
  added retries for 503s only, and once it correctly gave up after 3 attempts with my own error message.
  
- 429 error: hit the free-tier daily quota (20 requests per day per model). This was not a bug. The 13-second retry hint in the message
  applies to short-term limits, not the daily cap. My earlier retries had also used up quota, so I'm now more deliberate about test calls.
  

**What I kept:** The wrapper structure and the grounding approach. I kept retries for 503 only, because retrying a 400 or 403 did not help.

**What I changed or decided:**
- Decided not to use pandas. My data is typed in manually and stored in dictionaries, and my spec allows dict/loop logic for the analysis.
  Wrapping seven values in a DataFrame just to use pandas wouldn't be using a library for what it is meant for.
- Gradio launched successfully in Colab. <img width="1295" height="121" alt="Screenshot 2026-09-28 at 13 34 12" src="https://github.com/user-attachments/assets/0a3c9268-c8b6-4918-b42d-55b213b240a5" />


**Mistakes I made and fixed:**
1. NameError for ask_gemini_grounded, then for budgets. My Colab runtime had restarted, so earlier functions and variables were gone. Fixed
   with Runtime → Run all. I now run everything from the top after a break.
2. SyntaxError from pasting the summary sketch of the Gradio code (with "..." placeholders) instead of the full code. "..." is shorthand,
   not valid Python.

**Still to do:** Variation test (different numbers, confirm the reply changes) once my quota resets. Test the Gradio interface, including a
deliberate bad input such as a $0 budget.

**[CONTINUATION — found and fixed a bug via Gradio testing]**
**What I was trying to do:** Test the Gradio interface against the cases I'd already designed for (worked example, zero budget, negative spending), then deliberately try something I hadn't planned for, to see if the app would survive it.

**AI declaration:** 


--> Used Claude to help design the test cases, predict what would likely happen with a blank input box, write the fix, and write the new assert tests. I ran every test myself and read the tracebacks before accepting any explanation of what went wrong.

**What I tested and found:**
- Confirmed the interface matches my Step 3 worked example and my earlier assert tests exactly — same statuses, percentages and remaining amounts, and the same totals (1972 / 1074 / 1426 / 898). <img width="540" height="260" alt="Screenshot 2026-09-28 at 16 27 51" src="https://github.com/user-attachments/assets/79b60c8b-10c6-43c9-8ec0-f9412b947074" />

- Confirmed the existing guard clauses work through the interface, not just in the raw functions: a $0 Groceries budget gave "⚠️ Input error: Budget for Groceries must be greater than zero", and negative spending gave the matching message. <img width="456" height="43" alt="Screenshot 2026-09-28 at 16 29 34" src="https://github.com/user-attachments/assets/a545fef6-3a2e-4f20-8f20-7743694f60b3" />
  
- Then tried clearing a number box completely and clicking Check My Budget. This produced a generic "Error" in Gradio, not a friendly message. <img width="1047" height="344" alt="Screenshot 2026-09-28 at 14 00 30" src="https://github.com/user-attachments/assets/2d49c44b-78bd-409d-9d1f-d824488e34bf" />

  
**Why it broke:** 


an empty Gradio number box is passed to Python as None, not 0. My guard clauses checked budget_amount <= 0 and spent_amount < 0, but comparing None with a number raises a TypeError: '<' not supported between instances of 'NoneType' and 'int', which my except ValueError block does not catch. I reproduced this directly by calling calculate_budget_status({"Gym": 48}, {"Gym": None}) in a code cell before changing anything, and got the same TypeError. <img width="1023" height="331" alt="Screenshot 2026-09-28 at 14 12 08" src="https://github.com/user-attachments/assets/03608a58-9110-44c8-9712-0abb8dc519d2" />

**What I changed:** 
added an is None check in both calculate_budget_status and summarise_overall, before the existing numeric comparisons, so a missing value is caught and reported the same way a zero or negative one is: (Add-ons)

python
if budget_amount is None:
    raise ValueError(f"Budget for {category} is empty - please enter a number")
    
**What I kept:** 
the rest of the function logic and the existing ValueError pattern — I extended the same approach rather than introducing a different error-handling style for this case.

**How I verified the fix:**
- Added three new assert tests (empty budget, empty spending, empty income) that reproduce the exact TypeError scenario and check for the new, specific error message.
  
-  All new tests passed, and my earlier normal-case and edge-case tests still passed after the change.

- Re-ran the Gradio app, cleared the Entertainment spent box again, and got "⚠️ Input error: Spending for Entertainment is empty - please enter a number" instead of "Error". <img width="427" height="33" alt="Screenshot 2026-09-28 at 16 33 24" src="https://github.com/user-attachments/assets/07d1dd5f-b5c0-41bd-992f-1de70500fb31" />


**Real (not staged) failure:**
tried asking the assistant a question through the interface and hit Google's free-tier daily quota (20 requests/day), a 429 RESOURCE_EXHAUSTED error. This wasn't something I deliberately tested for — it happened because of my own earlier testing volume. The interface still didn't crash: my except ValueError in chat_with_assistant caught it and displayed "⚠️ The assistant couldn't respond right now: Gemini API error 429: ...".<img width="1245" height="158" alt="Screenshot 2026-09-28 at 14 38 54" src="https://github.com/user-attachments/assets/37cd5c26-0818-4861-94c4-a832da15b293" />

This is a genuine, unplanned example of the app handling an external failure gracefully, on top of the input cases I designed deliberately.


.
.



**WEEK 6 — [5/10/2026]**
**What I was trying to do:** Cleaning up Assessment repository by finalising and tidying codes, diary entries, README pass
