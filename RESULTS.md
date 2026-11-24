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

## Part B: Reflection

1. Explain why the tests locate Max Calories per Slice through its label instead of its id. Propose a change to the page that would let a test use an id, and state why that change helps a tester.

Ans ใน Step 0.4 คำสั่ง `$$("#floating_filled").length` ได้ผลเป็น 3 เพราะช่อง Max Calories per Slice, Min Number of Toppings และ Max Number of Toppings ใช้ id `floating_filled` ซ้ำกัน id นี้จึงแยกช่องไม่ได้ เทสต์จึงหาช่องผ่านข้อความของ label ที่ไม่ซ้ำ แล้วไปยัง input ที่อยู่ก่อนหน้า หน้าเว็บควรให้แต่ละช่องมี id ของตัวเอง เช่น `max-calories` และให้ `for` ของ label ชี้ไปที่ id นั้น tester จะใช้ locator สั้น ๆ อย่าง `#max-calories` ได้ และ locator จะไม่พังเมื่อข้อความ label หรือลำดับของช่องเปลี่ยน

2. Step 7 of QP-01 compares the tool in the admin list with the tool in the recommendation. Identify the kind of test oracle that this check is, and describe one fault in QuickPizza that it would catch.

Ans การตรวจนี้เป็น consistency oracle คือเปรียบเทียบข้อมูลสองส่วนของระบบกันเอง ได้แก่ tool ที่แสดงให้ลูกค้าเห็นกับ tool ที่บันทึกในหน้า admin แทนการเทียบกับค่าที่กำหนดไว้ล่วงหน้า เพราะ tool ถูกสุ่มทุกครั้ง (ในการรันของฉันได้ Scissors และ Pizza cutter) oracle นี้จะจับ fault ที่ server บันทึกพิซซ่าไม่ตรงกับที่ส่งให้ลูกค้าได้ เช่น บันทึกก่อนใช้เงื่อนไข ทำให้ admin list แสดง `tool=Knife` ขณะที่ลูกค้าเห็น Scissors

3. QP-01 never checks that the pizza is vegetarian. Propose a check that would, and state what the test would need from the page or from another source.

Ans เทสต์ควรอ่านรายการ ingredients ของพิซซ่าที่แนะนำ แล้วตรวจว่าไม่มีเนื้อสัตว์หรืออาหารทะเล เช่น ในการทดสอบด้วยมือได้ Olive oil, Roma tomatoes, Mozzarella, Ricotta cheese และ Buffalo mozzarella ซึ่งคนดูออกว่าเป็นมังสวิรัติ แต่เทสต์ต้องมีข้อมูลว่า ingredient ใดเป็นมังสวิรัติ ข้อมูลนี้ต้องมาจากแหล่งอื่น เช่น ค่า `vegetarian` ของแต่ละ ingredient ใน response ของ `POST /api/pizza` ซึ่งเทสต์ Cypress ดักไว้แล้วใน Step 4.6 หรือจากข้อมูล ingredients ของ QuickPizza

4. Compare the two tools that you used. Describe how each one waited for QuickPizza, how quickly you could write QP-01 with it, and how readable its report, log or video was.

Ans Selenium รอเฉพาะจุดที่เขียน explicit wait เอง เช่น `visibilityOfElementLocated` และ `textToBePresentInElementLocated` ถ้าไม่มี wait จะได้ NoSuchElementException ตามที่เห็นใน Step 2.10 ส่วน Cypress รอให้เองเพราะ `cy.get` และ `should` ลองซ้ำได้นานถึง 10 วินาที และ `cy.wait('@pizza')` รอ request จริง Cypress เขียน QP-01 ได้เร็วกว่าและใช้โค้ดน้อยกว่า ขณะที่ Java ต้องมี base class, page object และ helper สำหรับ screenshot report ของ Maven บอกบรรทัดที่ผิดชัดเจน เช่น `FirstTest.webFormOpens:20 expected: <Web Form> but was: <Web form>` ส่วนวิดีโอและ screenshot ของ Cypress ช่วยให้เห็นหน้าเว็บในแต่ละ step ได้ง่ายกว่า แต่การรัน Cypress ทั้งชุดหยุดด้วย error ภายใน `* is not a valid URL`

5. Choose the tool that you would use for the automated UI tests of Project 2, and justify the choice with two reasons from this lab.

Ans ฉันเลือก Selenium WebDriver กับ Java สำหรับ Project 2 เหตุผลแรก การรันทั้งชุดผ่านทั้งสองรอบ 5 tests และ 0 failures (36.0 s และ 32.8 s) ขณะที่การรัน Cypress ทั้งชุดหยุดด้วย error ภายในที่ยังหาสาเหตุไม่ได้ เหตุผลที่สอง page object เก็บ locator ของ QuickPizza ไว้ใน HomePage และ AdminPage ทำให้ QP01PageObjectTest อ่านได้เหมือนตาราง test case ซึ่งเหมาะกับงานกลุ่มที่หลายคนเขียนเทสต์ร่วมกัน

6. For Part 2, explain what has to change in QP01Test and in QP01PageObjectTest if QuickPizza changes the locator of the calories box, and why the page objects make that repair smaller.

Ans ใน QP01Test ต้องแก้ XPath ของช่อง calories ที่เขียนไว้ใน method ของเทสต์โดยตรง และต้องแก้ในทุกคลาสที่คัดลอก XPath นี้ไปใช้ ส่วน QP01PageObjectTest ไม่ต้องแก้เลย เพราะเรียกแค่ `home.setMaxCalories(600)` และ `home.maxCalories()` สิ่งที่ต้องแก้คือค่าคงที่ `MAX_CALORIES` ใน HomePage.java บรรทัดเดียวในไฟล์เดียว และทุกเทสต์ที่ใช้ page object นี้จะใช้งานได้ทันที

7. For Part 4, explain why QP-01 has to be a single test in Cypress, which clears the cookies before each test, and what would go wrong if steps 6 and 7 were a second test.

Ans Cypress ล้าง cookies, local storage และหน้าเว็บก่อนทุกเทสต์ ทุก `it` จึงเริ่มเหมือนผู้ใช้ใหม่ QP-01 ต้องใช้ค่าที่ต่อเนื่องทั้ง scenario คือชื่อพิซซ่าจาก step 3 และ tool ที่เก็บเป็น `@tool` ใน step 4 ซึ่งต้องใช้อีกครั้งใน step 7 และ alias ก็ไม่ข้ามไปยังเทสต์อื่น ถ้าแยก step 6 และ 7 เป็นเทสต์ที่สอง เทสต์นั้นจะไม่รู้ชื่อพิซซ่าและ tool ของเทสต์แรก จึงหาบรรทัดที่ถูกต้องใน admin list ไม่ได้ และเทสต์ทั้งสองจะต้องพึ่งลำดับการรันกัน
