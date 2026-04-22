# Web App Test Cases

This folder contains all test cases for the web version.

# Test Scenario Template

# Test Scenarios for [Feature/Module Name]

| Test Case ID | Test Description | Prerequisites | Test Steps | Expected Result | Status     | Remark          | Release Cycle | Test Execution Date | Test Executed By | Version Number |
|--------------|------------------|--------------|------------|----------------|------------|-----------------|---------------|--------------------|------------------| --------- |
| TC-001       | Verify logo and title presence | Homepage opened | 1. Observe the top section of the page. | The 'Triarch' logo and title should be clearly visible. | Successful | None            | Release 1.0   | 22/04/2026   | Tarurendra     |   v5 |
| TC-002       | Verify Landing Page Load | Website open | Open the landing page | Landing Page open | Successful |  None    | Release 1.0   | 22/04/2026                 | Tarurendra                 |   v5  |
| TC-003       | Verify Header Navigation | Landing page loaded successfully. | Click Ecosystem, ControlRoom, BuildRoom, About Us | Navigate correctly | Successfully | None        | Release 1.0   | 22/04/2026        | Tarurendra         |   v5    |
| TC-004       | Verify Request Demo  | Landing page loaded successfully. | 1. Go to Landing Page<br>2. Click Request demo<br>| Redirect works | Fail | Not working   | Release 1.0   | 22/04/2026     | Tarurendra    |  v5      |
| TC-005       | Verify Access Portal | Home page |1. Landing page<br>2. Click access portal<br> | open login page | Successfully| None    | Release 1.0   | 22/04/2026   |Tarurendra      |v5 |
| TC-006       | Verify  | User completes registration successfully. | 1. Complete registration with valid data.<br>2. Observe the page redirection.<br>3. Enter correct password<br>4. Click | User should be redirected to the redirect_uri mentioned in the URL parameters. | Successful | None                | Release 1.0   | 13/10/2025                 | Tarurendra                 |
| TC-007       | Check for "Quick access" | After User has logged in | 1. Go to login<br>2. After login, we see the home page<br>3. It shows <br>4. Click  | Each parameter has its unique functionality | Fail |In the quick access section, apart from "Saved code sessions," nothing works.                 | Release 1.0   |13/10/2025                  | Tarurendra                 |
| TC-008       | Check "Feature Resources" tab | After User has logged in | 1. Go to login<br>2. After login, we see the home page<br>3. It shows<br>4. Click  | The code guide appears. Different Jurisdictions appears | Fail | Not working                | Release 1.0   |13/10/2025                  | Tarurendra                 |
| TC-009       | Check at the top left search toolbar | After User has logged in | 1. Go to login<br>2. After login, we see the home page<br>3. It shows the left side at the top<br>4. Click on search | The searchbar shows the tools the user wishes without disappearing | Fail |The searchbar disappears.                 | Release 1.0   |13/10/2025                  |Tarurendra                  |
| TC-010       | Verify “View My Projects” button works | Home page loaded | 1. Go to login<br>2. After login, we see the home page<br>3. It shows<br>4. Click View My Projects | Redirects to the detailed project management page. | Fail | Unable to create or start a new project                | Release 1.0   | 13/10/2025                 | Tarurendra                 |
| TC-011       | Verify project list visibility under “My Projects” | Home page loaded | 1. Go to login<br>2. After login, we see the home page<br>3. It shows<br>4. Click View My Projects | Shows projects (e.g., Downtown Office Complex, Residential Tower A). | Fail | Unable to create or start a new project                | Release 1.0   |13/10/205                  |Tarurendra                  |
| TC-012       | Verify “View All Projects” button functionality | Home page loaded | 1. Go to login<br>2. After login, we see the home page<br>3. It shows view all projects<br>4. Click on | Redirects to all projects overview page. | Fail | Unable to see                | Release 1.0   | 13/10/2025                 | Tarurendra                 |
| TC-013       | Verify navigation to 'Saved Code Sessions' | User is on Home page | 1. Go to login<br>2. After login, we see the home page<br>3. It shows the view saved code session<br>4. Click on | User is redirected to Saved Code Sessions page | Fail |Leads to a new query instead.                 | Release 1.0   | 13/10/205                 | Tarurendra                 |
| TC-014       | Verify navigation to 'Code Amendments' | User is on Home page | 1. Go to login<br>2. After login, we see the home page<br>3. It shows the view link<br>4. Click on | User is redirected to Code Amendments page | Fail | Not open                | Release 1.0   |13/10/2025                  | Tarurendra                 |

