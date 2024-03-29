```mermaid
sequenceDiagram

User->>+Continuum: Go to Reservation.php
Continuum->>GA: Hit page
Continuum->>+4Escape (Iframe): load Iframe - page "Date choice"
4Escape (Iframe)-->>-Continuum: says to parent "I'm loaded" (PostMessage)
activate Continuum
Continuum->>GA: Hit page (iframe page URL)
deactivate Continuum
Continuum-->>-User: show iframe

User->>+4Escape (Iframe): Choose date
4Escape (Iframe)->>4Escape (Iframe): load page "confirm command"
4Escape (Iframe)-->>Continuum: says to parent "I'm loaded" (PostMessage)
activate Continuum
Continuum->>GA: Hit page (iframe page URL)
deactivate Continuum
4Escape (Iframe)-->>-User: show user data form and order details

User->>4Escape (Iframe): Filled data and confirm order
activate 4Escape (Iframe)
4Escape (Iframe)->>Payment Gateway[1]: Redirect to payment gateway 
deactivate 4Escape (Iframe)
activate Payment Gateway[1]
Note over Payment Gateway[1]: see note [2]
Payment Gateway[1]-->>User: Show payment method
deactivate Payment Gateway[1]

User->>+Payment Gateway[1]: Choose and pay
alt Payment Success
	Payment Gateway[1]->>-4Escape: Redirect to page "checkout/thankyou"
	activate 4Escape
	4Escape-->>GA: Hit page (current URL) + GTM Event order:success
	4Escape-->>User: Show page
	deactivate 4Escape
end
```

Notes:
* [1] SystemPay via Caisse d'épargne
* [2] This is where we've lost user session and referral for GA ?
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE5MjM3NDUzNywtMTE2MDA4MzMyNiwxOT
I1MzI2MjM4LDk2Mjk5OTE1MSwtMTk1OTYwMjgzMl19
-->