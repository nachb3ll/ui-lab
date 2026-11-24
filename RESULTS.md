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
- Step 2.15, QP01Test run 1, result and time: Pass, Tests run: 1, Failures: 0, 12.76 s (Maven total 20.6 s)
- Step 2.15, QP01Test run 2, result and time: Pass, Tests run: 1, Failures: 0, 14.15 s (Maven total 19.2 s)
- Step 2.20, full run 1, time and number of tests: BUILD SUCCESS, Tests run: 5, Failures: 0, total time 35.988 s (screenshots/20261124-234119)
- Step 2.20, full run 2, time and number of tests: BUILD SUCCESS, Tests run: 5, Failures: 0, total time 32.778 s (screenshots/20261124-234156, report in reports/uilab.QP01PageObjectTest.txt)

## Part 4: Cypress
- node --version, npm --version and npx cypress version: v22.21.0, 10.9.4, Cypress package version 16.2.1 and binary version 16.2.1
- Step 4.3, failure message for the wrong title: `Timed out retrying after 10000ms` with `-'Web form'` (actual) and `+'Web Form'` (expected)
- Step 4.6, tool and calories in the answer of POST /api/pizza: Scissors, 350 calories per slice (run of Lab Pizza 20261124-235133)
- Step 4.7, the line of the admin list that shows your pizza: Lab Pizza 20261124-235133 (tool=Scissors, ...), see cypress/screenshots/pizza-scenario.cy.js/qp01-step7-admin-list.png
- Step 4.8, run 1, result, duration and pizza name: Lab Pizza 20261124-234928, tool Scissors, 350 calories. pizza-scenario.cy.js reached step 4 and beyond; the full run then stopped with the Cypress error `Error: * is not a valid URL` before the summary table, so no duration was recorded
- Step 4.8, run 2, result, duration and pizza name: Lab Pizza 20261124-235133, pizza-scenario.cy.js 1 passing (23 s); the same Cypress error `* is not a valid URL` stopped the run during web-form.cy.js, so the full summary table was not printed. Not yet investigated
- Step 4.9 (optional), browser and result:

## Part B
1. In Step 0.4, `$$("#floating_filled").length` returned 3, because Max Calories per Slice, Min Number of Toppings and Max Number of Toppings share the id `floating_filled`. An id that matches three boxes cannot tell them apart, so the tests start from the unique label text and step to the input (`//label[normalize-space()='Max Calories per Slice']/preceding-sibling::input`). The page should give each box its own id, for example `max-calories`, and point each label's `for` at it. Then a tester can use a short `By.id("max-calories")` or `#max-calories` that does not break when the label text or the order of the boxes changes.
2. The check in step 7 is a consistency oracle: it compares two parts of the system with each other, the tool shown in the recommendation and the tool stored in the admin list, instead of comparing with a fixed expected value. In my runs the tool was random (Scissors, Pizza cutter), so only a consistency check can judge it. It would catch a fault where the server saves a different pizza than it sends to the customer, for example saving the tool before the restrictions are applied, so the admin list shows `tool=Knife` while the customer saw Scissors.
3. A check could take the ingredients of the recommended pizza and assert that none of them is meat or fish. My run 1 by hand showed Olive oil, Roma tomatoes, Mozzarella, Ricotta cheese and Buffalo mozzarella, which a person can judge, but the test needs a list of which ingredients are vegetarian. That list must come from another source, for example a `vegetarian` flag per ingredient in the answer of `POST /api/pizza`, which the Cypress test already intercepts in Step 4.6, or from the QuickPizza ingredients data.
4. Selenium waited only where I wrote an explicit wait: `visibilityOfElementLocated` for the heading, `textToBePresentInElementLocated` for the answer, and Step 2.10 showed that without it `findElement` fails with NoSuchElementException. Cypress waited by itself, because every `cy.get` and `should` retries for up to 10 seconds, and `cy.wait('@pizza')` waited for the real request. QP-01 was quicker to write in Cypress with less code, while the Java version needed a base class, page objects and a screenshot helper. The Maven report gave clear lines such as `FirstTest.webFormOpens:20 expected: <Web Form> but was: <Web form>`, and the Cypress video and step screenshots made it easier to see what the page looked like, although the full Cypress run stopped with the internal error `* is not a valid URL`.
5. I would choose Selenium WebDriver with Java for Project 2. First, its two full runs both passed with 5 tests and 0 failures (36.0 s and 32.8 s), while my Cypress full runs stopped with an internal error that I could not explain in time. Second, the page objects kept every locator of QuickPizza in HomePage and AdminPage, so QP01PageObjectTest reads like the test case table, which suits a group project where several people write tests.
6. (Part 2) In QP01Test, the XPath of the calories box is written inline in the test method, so it has to be changed there and in every other test class that copies it. In QP01PageObjectTest nothing changes, because the test only calls `home.setMaxCalories(600)` and `home.maxCalories()`. The only change is the constant `MAX_CALORIES` in HomePage.java, one line in one file, and every test that uses the page object is repaired at once.
7. (Part 4) Cypress clears cookies, local storage and the page before each test, so each `it` starts as a new user on a blank page. QP-01 needs values that live through the whole scenario: the pizza name made in step 3 and the tool aliased as `@tool` in step 4 are needed again in step 7, and aliases also do not survive between tests. If steps 6 and 7 were a second test, it would not know the pizza name or the tool of the first test and could not find the right line in the admin list, and the two tests would depend on running in a fixed order.
