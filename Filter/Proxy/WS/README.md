<!--
*** AUTO-GENERATED FILE ***
This file is auto-generated by BambdaChecker.
Please do not manually edit this file, or include any changes to this file in pull requests.
-->
# Proxy WebSockets Filter
Documentation: [Filtering the WebSockets history with Bambdas](https://portswigger.net/burp/documentation/desktop/tools/proxy/websockets-history/bambdas)
## [ExtractPayloadToNotes.bambda](https://github.com/PortSwigger/bambdas/blob/main/Filter/Proxy/WS/ExtractPayloadToNotes.bambda)
### Extracts JSON elements from the WebSocket message and displays it in the "Notes" column of the WebSocket History tab
#### Author: Nick Coblentz (https://github.com/ncoblentz)
```java
//The bambda will search for json elements with the following keys. The keys below are just examples. Add the keys you want to include here:
List<String> terms = List.of("target","error");

if (!message.annotations().hasNotes()) {
  StringBuilder builder = new StringBuilder();
  String payload = utilities().byteUtils().convertToString(message.payload().getBytes());
  terms.forEach(term -> {
    Matcher m = Pattern.compile("\"" + term + "\":\"([^\"]+)\"", Pattern.CASE_INSENSITIVE).matcher(payload);
    while (m.find() && m.groupCount() > 0) {
      for (int i = 1; i <= m.groupCount(); i++) {
        if (m.group(i) != null)
          builder.append(term + ": " + m.group(i) + " ");
      }
    }
  });
  message.annotations().setNotes(builder.toString());
}
return true;

```
