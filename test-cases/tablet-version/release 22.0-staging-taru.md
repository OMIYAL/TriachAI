# Tablet Test Cases

This folder contains all test cases for the tablet version.

# Test Scenario Template

# Test Scenarios for [Feature/Module Name]

| Test Case ID | Test Description | Prerequisites | Test Steps | Expected Result | Status     | Remark          | Release Cycle | Test Execution Date | Test Executed By | Version Number|
|--------------|------------------|--------------|------------|----------------|------------|-----------------|---------------|--------------------|----------------------|------------   |                
| TC-001       | CONVERSATION_MET Visible in AI Response | User is logged in | 1. Go to the landing page<br>2. Click Sign In<br>3. Welcome tour will appear<br>4. Search query<br> 5. At last, we see this error.  | AI response should end cleanly with relevant content and follow-up suggestions.| Fail | none        | Release 15.0   |17/01/2026  | Tarurendra  | v1.26.1.14   |
| TC-002       | Paused Session Resumes and Completes on Page Refresh | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Skip welcome tour<br>4. Search any query <br> 5. In mid-stop, the query and refresh the page  |The session should remain paused after refresh, and the AI response should not continue or complete unless explicitly resumed by the user. | Successful | none   | Release 15.0   |17/01/2026  | Tarurendra  |v1.26.1.14   |
| TC-003       | Remember me | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Remember button<br>4. Click on it>br>5. See error |We need to make sure the circle shows | Fail | none        | Release 15.0   |17/01/2026  | Tarurendra  | v1.26.1.14   |
| TC-004       | Developer Portal Open | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Click My Account and then select Subscription<br>4. Then click on change plan<br>5. Click on Upgrade to Premium, click the back button|Redirect to change plan | Successful | none        | Release 22.0   |14/02/2026  | Tarurendra  |  v1.26.2.13  |
| TC-005       | Developer Mode Opens After Logout from External Login Page | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Click Profile<br>4. Then click on External Logins<br>5. Click on log out|Home/Landing page |Close | none  | Release 22.0  |14/02/2026  | Tarurendra  |v1.26.2.13 |
| TC-006       | Password Strength / Requirement Not Displayed(Admin) | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Click on Administration<br>4. Click on Identity management<br>5. Click on Users-Click on New User | strength|Fail | none        | Release 15.0   |20/01/2026  | Tarurendra  | v1.26.1.14   |
| TC-007       |  Payment History | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Click My Account and then click  Payment History<br>4. Then, to check the history of payment|Open Payment History of every payment | Successful | none   | Release 22.0   |14/02/2026  | Tarurendra  |  v1.26.2.13  |
| TC-008       | Enhance UI Visibility for Login, Register, and Forgot Password button | User is logged in | 1. Go to the landing page<br>2. Click on Sign/Register <br>3. Click on login<br>4. Click on forget password|Convert into clearly styled buttons | Successful | none   | Release 22.0   |14/02/2026  | Tarurendra  |  v1.26.2.13  |
| TC-009       | Search Text Display Issue on Mobile & Desktop | User is logged in | 1. Go to the landing page<br>2. Click on Sign In <br>3. Search query area<br>4. Type any question and check it. |Search text should be clearly visible, properly aligned, and fully readable on both mobile and desktop screens. | Successful | none   | Release 22.0   |14/02/2026  | Tarurendra  |  v1.26.2.13  |
| TC-010       | Payment History Filter Not Working | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Click My Account and then click  Payment History<br>4. Then, to check the filter of payment|Open filter Payment History of every payment | Fail | none   | Release 22.0   |16/02/2026  | Tarurendra  |  v1.26.2.13  |
| TC-011       | Premium Payment Not Working | User is logged in | 1. Go to the landing page<br>2. Click on Sign In<br>3. Click My Account and then click  subscription<br>4. Then, click on change plan| payment | Fail | none   | Release 22.0   |16/02/2026  | Tarurendra  |  v1.26.2.13  |



