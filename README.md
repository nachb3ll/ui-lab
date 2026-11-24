# ui-lab

ITDS362 Software Quality Assurance and Testing, Lab: Automated UI Testing.

Tool parts that I chose: **Part 2, Selenium WebDriver in Java** and **Part 4, Cypress**.

## selenium-java (Part 2)

A Maven project with JUnit tests for the Selenium web form and test case QP-01 on QuickPizza, with page objects for the home page and the admin page. The folders `screenshots` and `reports` hold the evidence of two full runs.

```
cd selenium-java
mvn test
```

## cypress (Part 4)

A Cypress project with specs for the Selenium web form and test case QP-01 on QuickPizza, with a custom command for the admin login. The folders `cypress/videos` and `cypress/screenshots` hold the evidence of two full runs.

```
cd cypress
npx cypress run --browser chrome
```

Results and answers to Part B are in [RESULTS.md](RESULTS.md).
