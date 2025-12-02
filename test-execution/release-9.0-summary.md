# Release 9.0 Bug and Test Summary

**Release Milestone:** Release 9.0  
**Date:** 2025-12-02  
**Test Lead:** @yourusername  

## Bug Summary

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| #101   | TC-015       | High     | Payment| Open   | @dev1       | Payment gateway timeout |
| #50   | TC-029       | Medium   | Confirmation email not received  | Open | @dev2       | Users completing either the Demo Scheduling or Request Invitation forms are shown a success confirmation screen. However, no email is received for either action.|
| #65   | TC-032       | Medium   | Incorrect or Delayed Bookmark Query Loading  | Closed | @dev2       | When a user opens the Bookmarked Queries section and clicks on any saved bookmark, the system does not immediately open the correct bookmarked query.  |
| #64   | TC-033       | Medium   | AI Response Interrupted When Navigating to Session History  | Open | @dev2       | When a user initiates a query and then navigates to Session History before the AI completes its response, the AI stops generating output.  |
| #63   | TC-034       | Medium   | No Option to Stop Query Execution  | Closed | @dev2       | When a user enters a query, and the system begins generating a response, there is no visible option to stop or cancel the query once it has started. |
| #68   | TC-035       |  Minor    | The Welcome Tour is not visible in Firefox| Open   | @dev1       | The Welcome Tour does not appear when accessing the application through Firefox |
| #69   | TC-036       | Medium     | Page Refreshes After Stopping Query| Open   | @dev1       | When a user stops an active query and selects another query, the entire page refreshes automatically, interrupting the workflow. |
| #70   | TC-037       | Medium     | Jurisdiction & Adopted Codes Not Visible After Navigation| Open   | @dev1       | After opening Bookmarked Queries or Session History and then navigating back to the main page, the Jurisdiction and Adopted Codes sections do not appear. |
| #71   | TC-038       | Medium     | Menu Options Not Visible on Mobile | Open   | @dev1       | On mobile devices, after clicking the menu icon, Code & Preferences, Session History, and Collaboration options are not visible. |
| #72   | TC-039       | Minor     | Stream reading error| Open   | @dev1       | When the user searches for any query, the system shows a “stream reading error” and fails to load the AI response properly. |

## Test Execution Overview

| Test Status | Count | Percentage |
|-------------|-------|------------|
| Passed      | 90    | 90%        |
| Failed      | 10    | 10%        |
| Blocked     | 0     | 0%         |

## Key Insights and Recommendations

- Prioritize fixing high-priority bugs before release.
- Follow up on blocked test cases.
