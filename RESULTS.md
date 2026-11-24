# UI lab results

Tool parts that I chose: Part 2, Selenium WebDriver in Java, and Part 4, Cypress

## Part 0
- Step 0.1, Chrome version: 158.0.8127.88 (Official Build) (64-bit)
- Step 0.3, ChromeDriver version: 158.0.8127.88
- Step 0.4, locator of Pizza, Please!: `button[name='pizza-please']`
- Step 0.4, locator of Excluded tools: `//label[normalize-space()='Excluded tools']/following-sibling::select`
- Step 0.4, locator of Must be vegetarian: `#default-checkbox`
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name`
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261124-233500, Scissors, 500 calories per slice

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | The heading reads "Looking to break out of your pizza routine?" | Pass |
| 2    | Max Calories per Slice, Excluded tools, Must be vegetarian and Custom Pizza Name appear | Pass |
| 3    | The fields show 600, Knife selected, vegetarian ticked and Lab Pizza 20261124-233500 (calories first showed 1000 and had to be replaced) | Pass |
| 4    | The answer shows Name: Lab Pizza 20261124-233500, Tool: Scissors, Calories per slice: 500 | Pass |
| 5    | The page says "Please log in first." | Pass |
| 6    | After Sign in as admin, the page shows "Latest pizza recommendations" | Pass |
| 7    | The list shows "Lab Pizza 20261124-233500 (tool=Scissors, ingredients_number=5)", the same tool as in step 4 | Pass |

## Part 2: Selenium WebDriver in Java
- Step 2.4, the failure line for the wrong title: `[ERROR]   FirstTest.webFormOpens:20 expected: <Web Form> but was: <Web form>`
- Step 2.15, QP01Test run 1, result and time:
- Step 2.15, QP01Test run 2, result and time:
- Step 2.20, full run 1, time and number of tests:
- Step 2.20, full run 2, time and number of tests:

## Part 4: Cypress
- node --version, npm --version and npx cypress version:
- Step 4.3, failure message for the wrong title:
- Step 4.6, tool and calories in the answer of POST /api/pizza:
- Step 4.7, the line of the admin list that shows your pizza:
- Step 4.8, run 1, result, duration and pizza name:
- Step 4.8, run 2, result, duration and pizza name:
- Step 4.9 (optional), browser and result:

## Part B
1.
2.
3.
4.
5.
6.
7.
