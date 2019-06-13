```mermaid
sequenceDiagram
User->>Continuum: Go to Reservation.php
activate Continuum
Continuum->>GA: Hit page
Continuum->>4Escape (Iframe): load Iframe - page "Date choice"
activate 4Escape (Iframe)
4Escape (Iframe)-->>Continuum: says to parent "I'm loaded" (PostMessage)
deactivate 4Escape (Iframe)
Continuum->>GA: Hit page (iframe page URL)
Continuum-->>User: show dates table
activate User
User->>Continuum: Choose date 
Continuum->>+4Escape (Iframe): Confirm date
4Escape (Iframe)->>4Escape (Iframe)->>+: Confirm date
deactivate User
4Escape (Iframe)->>Payment Gateway[1]: 
deactivate Continuum
alt Payment Success
	4Escape (Iframe)->>4Escape: Got to page "checkout/thankyou"
	activate 4Escape
	Note over 4Escape: see note [4]
	4Escape->>GA: Hit page (current URL) + GTM Event order:success
	deactivate 4Escape
end
```

Notes:
* [1] SystemPay via Caisse d'épargne
* [2] send `custom.postMessage.page with` URL data
* [3] wait for `custom.postMessage.page` retrieve data from `dataLayer`
* [4] wait until `window.OrderId` is defined  and send `custom.waitForVar.OrderId` event
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAwODY4Njk1OSwxOTI1MzI2MjM4LDk2Mj
k5OTE1MSwtMTk1OTYwMjgzMl19
-->