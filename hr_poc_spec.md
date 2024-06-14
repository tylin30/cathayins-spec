# HR小精靈需求規格書 (草稿)



# API

## 開場說明 [POST] /v1/welcome_message

### Request

```json
{
    "jwt": "eyJhbGciOiJIUzUxMiJ9.eyJ1c2VyaW5mbyI6eyJDT01QQU5ZIjoiSU5TIiwiRElWX1NIT1JUX05BTUUiOiLllYbmpa3liIbmnpDnp5EiLCJFTUFJTCI6InRlc3RAY2F0aGF5LWlucy5jb20udHciLCJST0xFUyI6WyJST1pYWFhYIiwiUk9aWFhYWCIsIlJPWlhYWFgiLCJST1pYWFhYIiwiUk9aWFhYWCJdLCJFTVBJRCI6IkExMjM0NTY3ODkiLCJESVZfTk8iOiI0N0EwMjAwIiwiRU1QX05BTUUiOiLmiJHmmK_lpb3lk6Hlt6UiLCJDQVRIQVlfTk8iOiIwMDg3ODc4NyJ9LCJleHAiOjE3MTI1Njc4OTB9.hyWkyxZGjzPez3x_nz6rVwTXXSwEjQhGkNoBV4aodLpMYZWAt1sEOpeuB76cm1KeVxVand3dtRiwY2jqR_pjPQ"
}

```

### Response
```json
{
   "msg":"嗨~~歡迎使用HR小精靈服務!
          詢問時建議一次問一個問題，越精準單一的提問，應答效果會越好喔!
          由於目前小精靈還在POC測試階段，請注意回覆上可能有錯漏之情形
          小精靈可以協助解答的問題範圍如下 : 
          請假規則 : 特休假、公假、公傷病假、事假、普通傷病假(病假)、婚假、喪假、
                    生理假、產檢假、產假、陪產檢及陪產假、家庭照顧假、留職停薪、其他
          差勤系統 : 差勤管理、居家辦公申請、加值班申請、外出申請、其他
          勞健保 : 健保投保退保、員工退休金
          代理人 : 主管管理、職務代理、請假代理
          其他 : 福團理賠、結婚生育津貼、服務證明、請調申請",
    "session_id": "938c2cc0dcc05f2b68c4287040cfcf71"
}
```

## 送出新的使用者問題 [POST] /v1/conversation
<!-- conversation (tmp) -->

### Request
```json
{
    "session_id": "938c2cc0dcc05f2b68c4287040cfcf71",
    "data":
    {
        "user_submit": "你好，我想問一年可以請多少天病假",
        "user_submit_epochtime": "1718064549001", // ms
        "conversation_history": 
        {
            "conversation":[
            // 使用者問題 , chatbot回答
            ["小精靈你好", "你好，有什麼我可以幫助你的嗎？"], //第一個對話
            ["可以告訴我最近的麥當勞要怎麼走嗎", "不好意思，這個問題與差勤、請假無關"], //第二個對話
            ["可以告訴我要怎麼請病假嗎", "你可以到iHR系統上去申請"], //第三個對話
            ],
            "conversation_epochtime":
            [
               //submit time  //response start//response end
                ["1718064450004", "1718064470002", "1718064473002"],
                ["1718064500003", "1718064530001", "1718064532003"], 
                ["1718064535002", "1718064540006", "1718064545009"]
            ],
            "conversation_info": //要提供的markdown
            [
                "", 
                "",
                "<https://www.google.com/> 參見p1, p2"
            ]
        }
    }
}
```
### Response

```json
{
    "session_id": "938c2cc0dcc05f2b68c4287040cfcf71",
    "data":
    {
        "user_submit": "",
        "user_submit_epochtime": "",
        "conversation_history": 
        {
            "conversation":[
            // 使用者問題 , chatbot回答
            ["小精靈你好", "你好，有什麼我可以幫助你的嗎？"], //第一個對話, index=0
            ["可以告訴我最近的麥當勞要怎麼走嗎", "不好意思，這個問題與差勤、請假無關"], //第二個對話, index=1
            ["可以告訴我要怎麼請病假嗎", "你可以到iHR系統上去申請"], //第三個對話, index=2
            ["你好，我想問一年可以請多少天病假", ""] //新增第四個對話, index=3
            ],
            "conversation_epochtime":
            [
                //submit time     //response start //response end
                ["1718064450004", "1718064470002", "1718064473002"],
                ["1718064500003", "1718064530001", "1718064532003"], 
                ["1718064535002", "1718064540006", "1718064545009"],
                ["1718064549001", "", ""] //新增第四個對話 
            ],
            "conversation_info": //固定資訊(markdown格式)
            [
                "", 
                "",
                "<https://www.google.com/> 參見p1, p2",
                ""
            ]
        }
    }
}
```
###

### 聊天機器人回覆 [GET] /v1/chatbot_response
<!-- SSE -->
### Request
```json
{
    "session_id": "938c2cc0dcc05f2b68c4287040cfcf71",
}
```

### Response 

<!-- example1 -->
```json
{
    {
        "status": "process_starts", // 有process_generating, process_completed
        "session_id": "938c2cc0dcc05f2b68c4287040cfcf71",
        "epochtime": "1718064550000" 
    }
}
```

<!-- example2 -->
```json
{
    {
        "status": "process_generating",
        "session_id": "938c2cc0dcc05f2b68c4287040cfcf71",
        "epochtime": "1718064551000",
        "data": "根據"
    }	
}
```

```json
{
    {
        "status": "process_generating",
        "session_id": "938c2cc0dcc05f2b68c4287040cfcf71",
        "epochtime": "1718064551002",
        "data": "差勤管理"
    }	
}
```

<!-- example3 -->
```json
{
    {
        "status": "process_completed", 
        "session_id": "938c2cc0dcc05f2b68c4287040cfcf71",
        "epochtime": "171806491023",
        "data": "根據差勤管理的規定，一年有5天有薪病假。",
        "info": "<https://www.google.com/> 參見p3, p4" //markdown格式
    }	
}
``` 

## 機器人回覆回饋 [POST] /v1/response_feedback

### Request
```json
{
    {
        "session_id": "938c2cc0dcc05f2b68c4287040cfcf71",
        "feedback": {
            "0": "good",
            "2": "bad",
        }
    }
}
```
```json
{
    {
        "session_id": "938c2cc0dcc05f2b68c4287040cfcf71",
        "feedback": {
            "0": "good",
            "2": "bad",
        }
    }
}
```

### 備註
1. 請前端卡未輸入任何字不讓他submit
2. 送出後清除文字
