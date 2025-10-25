# Web App Test Cases

This folder contains all test cases for the web version.

# Test Scenario Template

# Test Scenarios for [Feature/Module Name]

| Test Case ID | Test Description | Prerequisites | Test Steps | Expected Result | Status     | Remark          | Release Cycle | Test Execution Date | Test Executed By |
|--------------|------------------|--------------|------------|----------------|------------|-----------------|---------------|--------------------|------------------|
| TC-001       | User login with valid credentials | User account exists | 1. Go to login<br>2. Enter username<br>3. Enter password<br>4. Click Login | Login successfully | Successfully | None            | Release 4.0   |25/10/2025                  | Tarurendra                 |
| TC-002       | Query Page | User logged in | 1. Go to login<br>2. After that, click on the bookmark icon<br>3. After that, click on the bookmarked queries | Shows Bookmarked queries | Successfully | None               | Release 4.0   |25/10/2025                  | Tarurendra                 |
| TC-003       | Query Page | User Logged in | 1. Go to login<br>2. After that, click on the feedback icon<br>3. Give rating<br>4. After that, icon change the colour | Feedback icon works | Successfully | None            | Release 4.0   | 25/10/2025                 | Tarurendra                 |
| TC-004       | Session History | User logged | 1. Go to login<br>2. After that, click on the Session History<br>3. Showing query history<br>4. You can check the old query | It gives an accurate time of the query session | Fail | It does not give the correct time of the query session.             | Release 4.0   |25/10/2025                  | Tarurendra                 |
| TC-005       | Feedback Icon | User logged in | 1. Go to login<br>2. After that, click on the feedback icon<br>3. Give rating<br>4. After that, submitted | Feedback submitted   | Successfully | None          | Release 4.0   |25/10/2025                  | Tarurendra                 |
| TC-006       | Bookmark icon | User logged in | 1. Go to login<br>2. After that, click on the bookmark icon<br>3. Check bookmarked queries | Bookmarked shows   | Successfully | None        | Release 4.0   |25/10/2025                  | Tarurendra                 |
| TC-007       | Login Input Fields | User logged in | 1. Go to login<br>2. After that, check the input field<br>3. The input field is black, showing |The input text and placeholders should have sufficient contrast to remain easily readable in dark mode.   | Fail | Poor Contrast in Login Input Fields        | Release 4.0   |25/10/2025                  | Tarurendra                 |
| TC-008       | Register Input Fields | User logged in | 1. Go to Register<br>2. After that, check the input field<br>3. The input field is black, showing |The input text and placeholders should have sufficient contrast to remain easily readable in dark mode.   | Fail | Poor Contrast in Register Input Fields        | Release 4.0   |25/10/2025                  | Tarurendra                 |
