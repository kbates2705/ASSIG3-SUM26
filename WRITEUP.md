# GreenThumb Security Assignment Writeup

## Task 1 – Login SQL Injection

### Vulnerability
The login page built the SQL query by directly inserting the username and password into the SQL statement. This allowed an attacker to inject SQL and bypass authentication.

### Fix
I replaced the string concatenation with parameterized queries using placeholders (`?`) and passed the username and password as parameters.

### Why it works
Parameterized queries separate user input from SQL commands, preventing injected SQL from being executed.

---

## Task 2 – Search SQL Injection

### Vulnerability
The search feature inserted the search term and sort option directly into the SQL query. This made it possible to perform UNION SQL injection and manipulate the ORDER BY clause.

### Fix
I parameterized the search values using placeholders and created an allow-list of valid sort options before building the ORDER BY clause.

### Why it works
The search text is treated as data instead of SQL, and only approved sort values can be used.

---

## Task 3 – Reflected XSS

### Vulnerability
The application displayed the user's search term directly in the HTML response without escaping special characters.

### Fix
I created an HTML escaping function and encoded the search term before displaying it.

### Why it works
Special characters like `<` and `>` are converted into HTML entities, so the browser displays the text instead of executing it.

---

## Task 4 – Stored and DOM XSS

### Stored XSS

#### Vulnerability
User comments were displayed directly in the page without encoding, allowing malicious JavaScript to be stored and executed.

#### Fix
I escaped the comment body and author before displaying them.

#### Why it works
Any HTML entered by a user is shown as plain text instead of being interpreted by the browser.

### DOM XSS

#### Vulnerability
The shared-note feature used HTML to insert user-controlled data into the page.

#### Fix
I replaced HTML with safe DOM methods.

#### Why it works
This treats the value as plain text, preventing injected HTML or JavaScript from executing.

---

## Task 5 – Security Hardening

### Vulnerability
The session cookie did not use secure attributes, and the application did not send a Content Security Policy (CSP).

### Fix
I added the `HttpOnly` and `SameSite=Strict` cookie flags and conditionally added the `Secure` flag for HTTPS. I also added a Content Security Policy header to every response.

### Why it works
The cookie is better protected from JavaScript access and cross-site request attacks, while the CSP reduces the risk of executing unauthorized scripts.

---

## Testing

After making the changes, I verified that:

- Normal login still worked.
- SQL injection login bypass no longer worked.
- UNION SQL injection no longer leaked data.
- Normal searching and sorting still worked.
- Reflected XSS payloads were displayed as plain text.
- Stored XSS payloads were displayed as plain text.
- DOM XSS payloads no longer executed.
- Session cookies included the required security flags.
- The Content Security Policy header was present.
- The exploit scripts reported the application as secure.