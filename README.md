
**Student Budget Assistant Built for SYS2001 Introduction to Business Programming, Assessment 2.[UPDATE]**


A small finance assistant for international students who manage a monthly budget across a handful of categories. You enter your income, your budget for each category, and what you've spent so far. The app shows where you stand in each category and lets you ask a budgeting coach questions. The coach's answers use your actual numbers.


**The problem**


International students often have money coming in with few rules about how to spend it. It's easy to lose track partway through the month and only notice an overspend when it's too late to adjust. This app gives a clear picture of where a student stands right now, plus practical advice based on their real figures.

**What it does**
1. **Budget checker (my own code):** calculate_budget_status() takes your budget and spending for each category and returns the amount spent, amount remaining, percent used, and a status: on track, close to limit (80% or more used) or overbudget. 

summarise_overall() returns total budgeted, total spent, unspent income and unused budget.

2. **Grounded assistant:** A budgeting coach powered by Google's Gemini model (called with the requests library). A system instruction sets its persona and tells it to steer off-topic questions back to budgeting. Your budget summary is added to each question so replies refer to your real numbers.

3. **Gradio interface:** Number boxes for income, budgets and spending, a Check My Budget button, and a question box with an Ask button.

4. **Input handling:** Zero budgets, negative spending, negative income and blank boxes are caught and shown as a plain message instead of crashing. A Gemini outage or quota error also shows a message rather than crashing the app.

**Categories:** 


Groceries, Rent, Transport, Entertainment, Subscription, Gym, Mobile data.


--> Sample input and output


Input: income $2,500, with these budgets and spending:


--Category	Budget--


-> Groceries: 400	


-> Rent: 1200

  
-> Transport: 150


->Entertainment: 100


-> Subscription: 35	


-> Gym: 48	


-> Mobile data: 39	



--Category Spending--


-> Groceries: 470


-> Rent: 300


-> Transport: 60


-> Entertainment: 115


-> Subscription: 70


-> Gym: 24


-> Mobile data: 35

**Output from Check My Budget:**


Groceries: spent $470 of $400 — 117.5% used — remaining $-70 — overbudget


Rent: spent $300 of $1200 — 25.0% used — remaining $900 — on track


Transport: spent $60 of $150 — 40.0% used — remaining $90 — on track


Entertainment: spent $115 of $100 — 115.0% used — remaining $-15 — overbudget


Subscription: spent $70 of $35 — 200.0% used — remaining $-35 — overbudget


Gym: spent $24 of $48 — 50.0% used — remaining $24 — on track


Mobile data: spent $35 of $39 — 89.7% used — remaining $4 — close to limit

.
.

Total budgeted: $1972


Total spent: $1074


Unspent from income: $1426


Unused budget: $898



**Question to the assistant: "Am I doing okay this month?"**
Reply (excerpt): The short answer is: 
**You are in a decent position overall, but you need to be cautious right now.** 

You have plenty of cash left in your account, but a couple of key categories have slipped over budget, and your biggest expense hasn't fully cleared yet. 

Here is a clear look at where you stand:

---

 1. The Big Picture: You have breathing room, but watch Rent!
* **Income:** $2,500
* **Spent so far:** $1,074
* **Unspent cash:** $1,426

At first glance, having $1,426 left looks fantastic! However, you’ve only paid **$300 of your $1,200 Rent budget**, meaning **$900 is still due**. 

Once you set that $900 aside for rent:
> **$1,426 − $900 = $526 real cash remaining** for the rest of the month.

Because you originally budgeted $1,972 against an income of $2,500, you intentionally built in a solid savings buffer of over $500. That smart planning is what is keeping you safe right now!

---

2. Where things went off track
A few categories need attention before they eat into that remaining buffer:

* **Groceries ($470 spent / $400 budget):** You are **$70 over**. As an international student, grocery shopping in a new country can be tricky—especially when buying imported comfort foods or adjusting to local prices. 
* **Subscriptions ($70 spent / $35 budget):** You're at **200% of your limit** (over by $35). Did an annual charge renew unexpectedly, or did a free trial end? Check your bank statement to see what happened here.
* **Entertainment ($115 spent / $100 budget):** You are slightly over by **$15**. Not a disaster, but try to switch to low-cost or free activities for the rest of the month.
* **Mobile Data ($35 spent / $39 budget):** You only have **$4 left**. Keep an eye on this so you don't get hit with expensive data overage fees!

---

3. Your Action Plan for the Rest of the Month

1. **Lock down the $900 for rent:** Move this money into a separate account or mentally mark it as "already spent" so you don't accidentally touch it.
2. **Pantry challenge for food:** Since you’re $70 over on groceries, try to stretch what’s already in your fridge and cupboards. Plan meals around basic staples like rice, beans, pasta, and eggs, or look for campus events offering free food.
3. **Hop on campus Wi-Fi:** Turn off cellular data whenever possible to protect that last $4 buffer on your phone bill.
4. **Audit your subscriptions:** Cancel any services you aren’t using daily, or switch to student discount rates (Spotify, Prime, etc., usually have 50% discounts for students).

**The Verdict:** You are doing fine because of the generous buffer you planned between your expenses and your income. Just put the brakes on discretionary spending now, secure that rent payment, and you'll finish the month in the green!

-
-
-
**How to run it**
1. Open the notebook Six_step_planning.ipynb in Google Colab.
2. Get a free API key from Google AI Studio.
3. In Colab, click the key icon in the left sidebar, add a secret named GEMINI_API_KEY, paste your key, and turn on notebook access. The key is never written in the notebook or the repository.
   
3. Run the notebook top to bottom with Runtime → Run all. The first Gradio cell installs Gradio (!pip install gradio --quiet).
When the last cell prints a gradio.live link, open it in its own browser tab (works better).

4. In testing, the embedded panel in Colab didn't respond, but the full tab worked.

   
**Notes and limits**
- The Gemini free tier has a small daily request limit (20 requests per day for the model used here) and can be temporarily overloaded. If you see an error message from the assistant, wait and try again. The budget checker itself works without Gemini.
The gradio.live link is public while the app is running. Don't share it, since anyone with the link could use it and consume the API key's quota.
- Spending is entered as a running total per category, not as individual dated transactions.









