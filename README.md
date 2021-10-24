# object-
Func _WD_FrameLeave($sSession)     Local $sOption     Local $sResponse, $sJSON, $asJSON     Local $sValue      ;*** For Firefox, you HAVE to have the $sOption present, but the value can be anything.     ;    Chrome ignores it just fine.     ;From Firefox: {"value":{"error":"invalid argument","message":"Failed to decode request as JSON: \"\"","stacktrace":"Syntax error at :1:1"}}     $sOption = '{"id":"anything"}'      $sResponse = _WD_Window($sSession, "parent", $sOption)     ;Chrome--     ;   Good: '{"value":null}'     ;   Bad: '{"value":{"error":"chrome not reachable"....     ;Firefox--     ;   Good: '{"value": {}}'     ;   Bad: '{"value":{"error":"unknown error","message":"Failed to decode response from marionette","stacktrace":""}}'      $sJSON = Json_Decode($sResponse)     $sValue = Json_Get($sJSON, "[value]")      ;*** Is this something besides a Chrome PASS?     If $sValue &lt;> Null Then         ;*** Check for a nested JSON object         If Json_IsObject($sValue) = True Then             $asJSON = Json_ObjGetKeys($sValue)              ;*** Is this an empty nested object             If UBound($asJSON) = 0 Then ;Firefox PASS                 $sValue = True
