---
title: Performance Recommendations
weight: 26
---

To avoid impacting user experience or other integrations, it is important to utilise the APIs efficiently:

1. Only fetch the fields you need - use the **resultColumns** parameter
3. Suppress null value fields - use the **nullvaluehandling** parameter
4. Disable alternate key handling - use the **alternatekeyhandling** parameter
5. Make use of Webhooks not polling - see the Webhooks guide.
6. Ensure the use of compression - include header `Accept-Encoding:  gzip or deflate`

