**Week 1 — [11/09/2026]**

Spent learning weeks 1-7 on labs and repo setup but hadn't locked in a project direction or started building. Starting the build now with roughly 4-5 weeks to the deadline.

Plan from here: Deciding on a problem , then API → tool → data → Gradio → tests, one real commit per session.

Problem for project:[Budgeting Assistant for Monthly Budgeting] -> A budgeting assistant for a super managing monthly budget across desired spending categories (e.g.rent, groceries, transport, entertainment, and subscriptions) -> The user will enter their income and their planned budget limit for each spending categories, then logs actual spending within the process. -> Expectations: The app will show the user category-by-category and their current financial situations whether they're (ON BUDGET, OVERBUDGET, UNDERBUDGET), following with a conversational assistant that are aware of the numbers and financial circumstances and will be available to ask questions and give financial advices.

**Specific plan and objective:**
University students, especially international students living out of home often face tight, and irregular financial income from part-time jobs, support from families, and even government financial services. Being an international student with freedom of spending tends to find it easy to lose track of discrete spending, especially in food and entertainment. This could be a tumbling issue when high-cost expenses such as rent or bills are due. 

Thus, this financial assistant allows students to enter their income and budget limits for each category while continuously updating their spending history in the process to display a clear and unbiased view of their financial position. Moreover, genuine conversational financial advice services by the budgeting assistant are given based on the actual data and numbers (Using Google API). By this, students would be given the opportunity of a more controlled financial planning to only spend within the budget limit, if not, solutions to stabilise the overbudget spending will be referred by the financial assistant.

**AI DECLARATION:**
--> AI was used to provide ideas for potential problems users may face, in this case inconsistent and uncontrolled spending of international students due freedom of spending and income flow from family support, part-time, government financial help




**Week 1 - [12/09/2026]**

**Starting on the six-method planning:**

1. Understanding the problem
2. Decided on expected input and output
3. Creating an example desired input and output and status by hand (as a known answer test case for final check)
   --> Miscalculated the remaining values, however fixed with the help of AI checking

**AI DECLARATION:**
--> [Paste first finished 3 method of planning] Check whether the structure and content of the planning is on point and correct.




**Week 1 - [13/09/2026]**

4.  Creating a Pseudocode (4th method): Plain-logic for the problem [seen in collab]

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

**Issues occurred:** When running the finished Pseudocode, an error message appear stating that the error came from "raise error" keyword
    --> An easy fix, the issue was focused mainly on indentation.

**AI DECLARATION:**
--> Based on the problem stated, write a relevant pseudocode as a reference





**Week 2 - [19/09/2026]** UPDATED

**What I was trying to do:** Finish converting my Step 4 pseudocode into working Python (Step 5), and write proper tests for it (Step 6) — both required parts of the six-step method (R6), and the testing also covers R5 and reinforces R3 (custom tool handling bad input).

**AI declaration:** Used Claude throughout this session to help translate pseudocode into Python syntax, explain line-by-line what the code does, and help design and write assert-based tests, including error-case tests using try/except.

**Prompt/question given:** Asked Claude to convert my pseudocode for calculate_budget_status() and summarise_overall() into real Python, then asked it to help me write tests — first checking normal cases against my Step 3 worked example, then edge cases (no spending logged) and error cases (zero budget, negative spending, negative income).

**What it returned:** Two working Python functions matching my pseudocode's logic, plus a set of print-based checks, which I then converted into proper assert statements — including try/except blocks to test that my ValueError guard clauses raise the correct error with the correct message.

**Anything rejected or corrected:** 
1. Found a floating-point display issue — Entertainment's percent_used printed as 114.99999999999999 instead of 115.0, a binary rounding artifact. Fixed by wrapping display values with round(x, 1), and used the same rounding inside the relevant assert to avoid a false failure 
from exact-equality comparison on a near-115 float.

2. Accidentally mislabelled my Python code cell as "STEP 4: PSEUDOCODE"  instead of "STEP 5: PYTHON IMPLEMENTATION" — caught this while 
reviewing my own notebook structure and corrected the heading.
  
3. Tried running my actual pseudocode (the plain-English version) directly as a code cell out of curiosity/confusion, which correctly produced a SyntaxError ("function" isn't a real Python keyword). This helped confirm my understanding of why pseudocode belongs in a markdown cell, not a code cell.

**Result:** All normal-case and edge/error-case tests pass. Six-step method now fully evidenced (Steps 1–6 complete for the custom budget tool), with R3, R5, and R6 substantially covered.
  
  


