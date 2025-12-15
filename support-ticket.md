
### The "Support Ticket" System

**The Scenario**  "We are building a Customer Support mobile app. Users can fill out a text form to describe their issue and attach screenshots (images/logs) to help explain the bug.

To ensure the app feels very responsive, we designed the upload process to be  **asynchronous**  (background processing). Here is the proposed logic:

1.  **Start:**  When the user opens the 'New Ticket' screen, the mobile app generates a  **Temporary ID**  (e.g.,  `TICKET-99`) immediately.
    
2.  **Flow A (Attachments):**  As soon as the user selects a file, the app starts uploading it in the background to our  **File Service**, tagged with  `TICKET-99`.
    
3.  **Flow B (Submission):**  When the user finishes typing and clicks 'Submit', the app sends the text details (Subject, Description, User Info) to the  **Ticket Service**, also tagged with  `TICKET-99`.
    

**The Current Logic**  When the  **Ticket Service**  receives the 'Submit' request in Flow B, it executes this logic immediately:

1.  Save the ticket text to the database.
    
2.  Query the  **File Service**  for any files tagged  `TICKET-99`.
    
3.  Link those files to the ticket.
    
4.  Send a 'Success' email to the support team."
    

**The Diagram**

Code snippet

```
sequenceDiagram
    participant User
    participant App as Mobile App
    participant File as File Service
    participant Ticket as Ticket Service

    Note over App: Generates Temp ID: TICKET-99

    par Flow A: Attachments
        User->>App: Selects Screenshot (5MB)
        App->>File: Starts Uploading Screenshot (Tag: TICKET-99)
    and Flow B: Submission
        User->>App: Types text & clicks "Submit"
        App->>Ticket: Sends Ticket Data (Tag: TICKET-99)
    end

    Note right of Ticket: Ticket Service receives request<br/>and immediately runs logic. Flow B doesn't wait for Flow A.
    Ticket->>Ticket: 1. Save Text
    Ticket->>File: 2. Query for files with TICKET-99
    Ticket->>Ticket: 3. Link found files & Finish
```


**Question**
If we launch this exactly as described, are there any scenarios where the data might end up being incorrect or incomplete? If so, walk me through exactly how that happens. You can use whatever tools to explain your understanding, drawing, pseudocode code... your choice
