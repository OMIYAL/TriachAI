# Release 8.0 Bug and Test Summary

**Release Milestone:** Release 8.0  
**Date:** 2025-11-11  
**Test Lead:** @yourusername  

## Bug Summary

| Bug ID | Test Case ID | Priority | Module | Status | Assigned To | Comments |
|--------|--------------|----------|--------|--------|-------------|----------|
| #101   | TC-015       | High     | Payment| Open   | @dev1       | Payment gateway timeout |
| #40   | TC-025       | Medium   | Feedback icon  | Closed | @dev2       | In dark mode it not visible clearly |
| #48   | TC-027       | Medium   | Feedback Type Buttons  | Closed | @dev2       | Feedback Type Buttons Not Clearly Visible in Dark Mode|
| #37   | TC-028       | Medium   | Smart scrolling  | Closed | @dev2       | When the AI is answering a query, the page scrolls by itself and disturbs the user's reading |
| #50   | TC-029       | Medium   | Confirmation email not received  | Open | @dev2       | Users completing either the Demo Scheduling or Request Invitation forms are shown a success confirmation screen. However, no email is received for either action.|
| #55   | TC-030       | Medium   | Welcome aboard!  | Closed | @dev2       | Clicking Next on the “Welcome aboard!” screen and then Back skips page 3 and jumps to page 4, indicating a navigation tour is completed. |
| #57   | TC-031       | Medium   | Navigation shows beyond the mobile screen  | Open | @dev2       | Clicking Next on the “Welcome aboard!” after that, beyond the screen |
| #65   | TC-032       | Medium   | Incorrect or Delayed Bookmark Query Loading  | Open | @dev2       | When a user opens the Bookmarked Queries section and clicks on any saved bookmark, the system does not immediately open the correct bookmarked query.  |
| #64   | TC-033       | Medium   | AI Response Interrupted When Navigating to Session History  | Open | @dev2       | When a user initiates a query and then navigates to Session History before the AI completes its response, the AI stops generating output.  |
| #63   | TC-034       | Medium   | No Option to Stop Query Execution  | Open | @dev2       | When a user enters a query and the system begins generating a response, there is no visible option to stop or cancel the query once it has started. |

## Test Execution Overview

| Test Status | Count | Percentage |
|-------------|-------|------------|
| Passed      | 90    | 90%        |
| Failed      | 10    | 10%        |
| Blocked     | 0     | 0%         |

## Key Insights and Recommendations

- Prioritize fixing high-priority bugs before release.
- Follow up on blocked test cases.
