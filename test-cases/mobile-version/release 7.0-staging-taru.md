# Mobile Test Cases

This folder contains all test cases for the mobile version.

# Test Scenario Template

# Test Scenarios for [Feature/Module Name]

| Test Case ID | Test Description | Prerequisites | Test Steps | Expected Result | Status     | Remark          | Release Cycle | Test Execution Date | Test Executed By | Version Number|
|--------------|------------------|--------------|------------|----------------|------------|-----------------|---------------|--------------------|----------------------|------------   |                
| TC-001       | Navigation Skips | User on Query Page | 1. Go to the login<br>2. Check the Query Page<br>3. It shows Welcome Tour<br>4. Click Next/Back | It works in correct way when we click on Next/Back | Fail | Back skips page 3 and jumps to page 4, indicating a navigation tour is completed.        | Release 7.0   |08/11/202          | Tarurendra  | v1.25.11.7     |
| TC-002       | Recommended followup error | User on Query Page | 1. Go to the login<br>2. Check the Query Page<br>3. Click on Recommended query follow-up <br>4. You can see the error | It should run query | Fail | Recommended followup error and it will create new session           | Release 7.0   |07/11/202          | Tarurendra  | v1.25.11.7    |
| TC-003       | Navigation shows beyond the mobile screen | User on Query Page | 1. Go to the login<br>2. Check the Query Page<br>3. It will show Welcome Tour <br>4. Click on Next | It should show the Next button | Fail | Navigation shows beyond the mobile screen        | Release 7.0   |07/11/202          | Tarurendra  | v1.25.11.7    |
