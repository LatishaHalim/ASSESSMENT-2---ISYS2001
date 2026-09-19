Week 1 — [11/09/2026]

Spent learning weeks 1-7 on labs and repo setup but hadn't locked in a project direction or started building. Starting the build now with roughly 4-5 weeks to the deadline.

Plan from here: Deciding on a problem , then API → tool → data → Gradio → tests, one real commit per session.

Problem for project:[Budgeting Assistant for Monthly Budgeting] -> A budgeting assistant for a super managing monthly budget across desired spending categories (e.g.rent, groceries, transport, entertainment, and subscriptions) -> The user will enter their income and their planned budget limit for each spending categories, then logs actual spending within the process. -> Expectations: The app will show the user category-by-category and their current financial situations whether they're (ON BUDGET, OVERBUDGET, UNDERBUDGET), following with a conversational assistant that are aware of the numbers and financial circumstances and will be available to ask questions and give financial advices.

Specific plan and objective:
University students, especially international students living out of home often face tight, and irregular financial income from part-time jobs, support from families, and even government financial services. Being an international student with freedom of spending tends to find it easy to lose track of discrete spending, especially in food and entertainment. This could be a tumbling issue when high-cost expenses such as rent or bills are due. 

Thus, this financial assistant allows students to enter their income and budget limits for each category while continuously updating their spending history in the process to display a clear and unbiased view of their financial position. Moreover, genuine conversational financial advice services by the budgeting assistant are given based on the actual data and numbers (Using Google API). By this, students would be given the opportunity of a more controlled financial planning to only spend within the budget limit, if not, solutions to stabilise the overbudget spending will be referred by the financial assistant.

* AI DECLARATION:
--> AI was used to provide ideas for potential problems users may face, in this case inconsistent and uncontrolled spending of international students due freedom of spending and income flow from family support, part-time, government financial help




Week 1 - [12/09/2026]

> Starting on the six-method planning:

1. Understanding the problem
2. Decided on expected input and output
3. Creating an example desired input and output and status by hand (as a known answer test case for final check)
   --> Miscalculated the remaining values, however fixed with the help of AI checking

* AI DECLARATION:
--> [Paste first finished 3 method of planning] Check whether the structure and content of the planning is on point and correct.




Week 1 - [13/09/2026]

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

* Issues occurred: When running the finished Pseudocode, an error message appear stating that the error came from "raise error" keyword
    --> An easy fix, the issue was focused mainly on indentation.

* AI DECLARATION:
--> Based on the problem stated, write a relevant pseudocode as a reference





Week 2 - [19/09/2026]

Converted my Step 4 pseudocode into two real Python functions: calculate_budget_status() and summarise_overall(). 
Tested both against my Step 3 worked example (7 categories, $2500 income) and the output matched my by-hand calculations exactly for every category and the overall totals.

Noticed a floating-point quirk — 115% displayed as 114.99999999999999 before I rounded it — not a logic bug, just how decimal division works in binary. Fixed the display with round().

* AI DECLARATION:
  **What I was trying to do:** Convert my Step 4 pseudocode into real Python functions for calculating budget status and overall summary, and verify they work correctly.

**AI declaration:** Used Claude to help translate my pseudocode into Python syntax (converting "for each category" loops, "raise an error" statements, and if/else logic into actual Python), and to help write test code that runs the functions against my Step 3 worked example.

**Prompt/question given:** Asked Claude to build the two functions to match my pseudocode exactly, then to help me test them against my worked example numbers.

**What it returned:** Two Python functions (calculate_budget_status, summarise_overall) matching my pseudocode logic, plus test code using my Step 3 numbers (7 categories, $2500 income).

**What I kept:** The full function structure — it matched my pseudocode's logic exactly (loop over budgets, guard clauses for bad input, status classification order). Also kept the f-string formatting suggestion for 
cleaner print output.

**What I changed/checked:** Ran the functions myself against my own worked example numbers rather than trusting the output blindly — confirmed every category's spent/remaining/percent/status matched my by-hand table exactly, and the summary totals matched too.

**Anything rejected or corrected:** Noticed the raw output showed 114.99999999999999 instead of 115.0 for Entertainment's percentage — a 
floating-point rounding artifact from binary division, not a logic error. Fixed the display by wrapping with round(percent_used, 1) rather than leaving the raw value.



  
   

   
  
  


