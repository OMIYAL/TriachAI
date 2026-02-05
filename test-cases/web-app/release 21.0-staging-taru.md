# Web Test Cases

This folder contains all test cases for the web version.

# Test Scenario Template

# Test Scenarios for [Feature/Module Name]

| Test Case ID | Test Description | Prerequisites | Test Steps | Expected Result | Status     | Remark          | Release Cycle | Test Execution Date | Test Executed By | Version Number|
|--------------|------------------|--------------|------------|----------------|------------|-----------------|---------------|--------------------|----------------------|------------   |                
| TC-001       | CONVERSATION_MET Visible in AI Response | User is logged in | 1. Go to the landing page<br>2. Click Sign In<br>3. Welcome tour will appear<br>4. Search query<br> 5. At last, we see this error.  | AI response should end cleanly with relevant content and follow-up suggestions.| Fail | none        | Release 15.0   |17/01/2026  | Tarurendra  | v1.26.1.14   |
| TC-002       | Show password does not work| User is logged in | 1. Go to the landing page<br>2. Click on Register<br>3. Registration page open <br>4. Fill the password<br> 5. When we want to see the password  | the Password text should become visible (plain text) when the eye icon is clicked.| Successful | none        | Release 15.0   |17/01/2026  | Tarurendra  | v1.26.1.14|
| TC-003       | Paused Session Resumes and Completes on Page Refresh | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Skip welcome tour<br>4. Search any query <br> 5. In mid-stop, the query and refresh the page  |The session should remain paused after refresh, and the AI response should not continue or complete unless explicitly resumed by the user. | Fail | none        | Release 15.0   |17/01/2026  | Tarurendra  |v1.26.1.14   |
| TC-004       | Sessions Not Displayed After Applying Jurisdiction Filter on Page 2 | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Skip tour<br>4. Click on Session History<br>5. Check filter |Sessions matching the selected jurisdiction should be displayed | Successful | none        | Release 21.0  |05/02/2026  | Tarurendra  | v1.26.2.5  |
| TC-005       | Bookmarked Queries Not Displayed After Applying Jurisdiction Filter on Page 2 | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Skip tour<br>4. Click on Bookmarked Queries<br>5. Check filter |Bookmarked queries matching the selected jurisdiction should be displayed | Successful | none        | Release 21.0   |05/02/2026  | Tarurendra  |   v1.26.2.5  |
| TC-006       | Remember me | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Remember button<br>4. Click on it>br>5. See error |We need to make sure the circle shows | Fail | none        | Release 15.0   |17/01/2026  | Tarurendra  | v1.26.1.14   |
| TC-007       | Developer Portal Open | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Click My Account and then select Subscription<br>4. Then click on change plan<br>5. Click on Upgrade to Premium, click the back button|Redirect to change plan | Fail | none        | Release 15.0   |17/01/2026  | Tarurendra  | v1.26.1.14   |
| TC-008       | Developer Mode Opens After Logout from External Login Page | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Click Profile<br>4. Then click on External Logins<br>5. Click on log out|Home/Landing page | login|Fail | none        | Release 15.0   |20/01/2026  | Tarurendra  | v1.26.1.14   |
| TC-009       | Password Strength / Requirement Not Displayed(Admin) | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Click on Administration<br>4. Click on Identity management<br>5. Click on Users-Click on New User | strength|Fail | none        | Release 15.0   |20/01/2026  | Tarurendra  | v1.26.1.14   |
| TC-010       | Real-Time Query Counter Not Updating  | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. After login<br>4. See |The Query Counter should update in real time immediately after each query is submitted |Successful | none | Release 20.0   |04/01/2026  | Tarurendra  |v1.26.2.3   |
| TC-011       | Tenant Information Display  | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. After login, skip tour <br>4. See right side at top | Added Tenant name display in the top section of the interface.|Successful | none | Release 20.0   |04/02/2026  | Tarurendra  | v1.26.2.3   |
| TC-012       | Buy More Queries  | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. After login<br>4. See |Rediredct to Buy Additional Queries |Successful | none | Release 20.0   |04/01/2026  | Tarurendra  |v1.26.2.3   |
| TC-013       | Premium profile |  User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. After login<br>4. See | When we change from free to premium, then the  profile icon changes |Successful | none | Release 20.0   |04/01/2026  | Tarurendra  |v1.26.2.3   |


