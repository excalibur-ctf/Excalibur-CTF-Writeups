## Description
Only professors are allowed beyond this door… or so it claims. The login page is loud, but the real weakness is silent. Think less about brute force, more about how access is checked.

## Solution

### Intented Approach

By analysing the script.js file we can see there is a `role=` parameter required for accessing admin profile.

<img width="715" height="227" alt="image" src="https://github.com/user-attachments/assets/f34954d5-513b-47b1-8999-632764d2be7a" />

So, appending the `role=` parameter with the value admin reveals the admin profile which also reveals the flag.

<img width="1297" height="455" alt="image" src="https://github.com/user-attachments/assets/566c1960-4ab3-429d-b1c2-5accb9b9ade0" />

<img width="1329" height="831" alt="image" src="https://github.com/user-attachments/assets/6f128ca1-db4e-4864-bc65-db2efd6c3ad0" />


Flag: `EXC{magic_url_controls_are_dangerous}`
