# Test Procedure 1: Context Gathering

## The Prompt

```
Run Context Gathering for [BRAND_NAME].

===== HOW TO RUN =====

1. Search Notion Brands Masterview for [BRAND_NAME] — fetch sales notes
2. Run all steps below
3. Create a new task in the Tasks database:
   https://www.notion.so/fdada86ca84a4d04881afefd828eb17c
   Properties:
   - Task: "Onboarding [BRAND_NAME]"
   - Type: "Onboarding"
   - Status: "In Progress"
   - Brand: link to [BRAND_NAME] in Brands database (if found)
4. Write output inside that task's page

===== STEP 1: READ SALES NOTES =====

Read sales notes thoroughly. Extract and summarize:

1. Client goals (short-term and long-term)
2. Constraints and conditions (budget, timelines, restrictions)
3. Any specific products client wants to focus on
4. Any other important notes (special instructions, preferences)

===== STEP 2: WEB RESEARCH =====

Web search the brand name. Write a short summary:
- What the brand is
- What products they sell
- What their USPs are

IMPORTANT: Do NOT rely on the first few search results for the
product list. Search specifically for the brand's full catalog:
- Check the Amazon store page (search "[BRAND_NAME] Amazon store")
- Check the brand's website if it exists
- Search "[BRAND_NAME] full product line"
If the first search only shows 2-3 products, search again deeper.
The brand summary must include ALL products, not just the top sellers.

===== STEP 3: STANDARD QUESTION CHECKLIST =====

Check sales notes against each question below.
If answered in sales notes → write the answer from sales notes.
If not answered → add to "Client Questions" list.

1. Who is the target audience?
2. Any other sales channels — D2C, offline, other marketplaces?
3. What are client expectations — short-term and long-term?
4. Why is the product good — detailed USPs and differentiators?
5. Promotion plans, upcoming price changes?
6. Who are the competitors?
7. Why do people buy this product — deeper TG understanding?
8. Past advertising history — what's been tried, what worked, what didn't?
9. Are there specific products client wants us to prioritize?

Check each one by one. If partially answered, write what's
there AND add the gap to Client Questions.

===== STEP 4: DATA POINT CHECK =====

Check sales notes for these data points.
If found → capture under Important Client Notes.
If not found → add to "Pending Items".

- COGS
- Lead time
- Ad budget
- Product images / videos (check if Google Drive link is present)

===== OUTPUT =====

Write inside the task page "Onboarding [BRAND_NAME]" in the
Tasks database: https://www.notion.so/fdada86ca84a4d04881afefd828eb17c

Page must start with heading:
# Procedure 1: Context Gathering

Then the table:

| Section | Content |
|---------|---------|
| Brand Summary | • point 1 • point 2 • point 3 (each fact as a separate bullet) |
| Client Goals | • goal 1 • goal 2 • goal 3 |
| Important Client Notes | • note 1 • note 2 • COGS: value • lead time: value |
| Client Questions | • question 1? • question 2? • question 3? |
| Pending Items | 🚩 item 1 🚩 item 2 🚩 item 3 |

FORMAT RULES:
- Every section's content MUST be in bullet points (•)
- One fact/question/item per bullet — never paragraphs
- Pending Items: each item starts with 🚩 emoji so they
  stand out visually as action items needing client input
- Client Questions: each question is one bullet with ?
- Keep each bullet to ONE line — short and scannable

STOP. Present output. Wait for review.
```
