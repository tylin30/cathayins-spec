# HR小精靈需求規格書 (草稿)



# API

## 身份驗證 [POST] /v1/login
### Request



# auth_token

```json
{
    "username": "ba",
    "password": "ba1234"
}
```

### Response
```json
{
    "msg": true, //true: 成功, false:失敗
    "session_id": "z4ubh11d6ef"
}
```

## 開場說明 [POST] /v1/instruction

### Request

```json
```

### Response
```json
{
   "msg":"嗨~~歡迎使用HR小精靈服務!
          詢問時建議一次問一個問題，越精準單一的提問，應答效果會越好喔!
          由於目前小精靈還在POC測試階段，請注意回覆上可能有錯漏之情形
          小精靈可以協助解答的問題範圍如下 : 
          請假規則 : 特休假、公假、公傷病假、事假、普通傷病假(病假)、婚假、喪假、
                           生理假、產檢假、產假、陪產檢及陪產假、家庭照顧假、          留職停薪、其他
          差勤系統 : 差勤管理、居家辦公申請、加值班申請、外出申請、其他
          勞健保 : 健保投保退保、員工退休金
          代理人 : 主管管理、職務代理、請假代理
          其他 : 福團理賠、結婚生育津貼、服務證明、請調申請",
    "session_id": "z4ubh11d6ef"
}
```

## 對話紀錄 [GET] /v1/join
<!-- conversation (tmp) -->

### Request
```json
{
    "session_id": "z4ubh11d6ef",
    "data": //current_question + history_questions&responses
        [
            user_question3,
        [
            [user_question1, chatbot_response1],
            [user_question2, chatbot_response2],
            // ......
        ]
        ],
    // "event_data": null, # gradio api有，但不知道要做什麼用的
    // "fn_index":0, # gradio api有，但不知道要做什麼用的
    // "trigger_id":8, # gradio api有，但不知道要做什麼用的
    
}
```
##

### SSE [GET]
### Request
```json
{
    "session_id": "z4ubh11d6ef",
}
```

### Response 
```json
{
    {
        "msg":"estimation",
        // "event_id":"dc02961ca6aa47a28ba8b2732fece954",
        // "rank":0,
        // "queue_size":1,
        // "rank_eta":10.38514494895935
    }
}
```

```json
{
    {
        "msg":"process_starts",
        "event_id":"dc02961ca6aa47a28ba8b2732fece954",
        // "eta":10.38514494895935
    }	

}
```

```json
{
    {"msg": "process_generating", // process_completed, close_stream
    // "event_id": "2c8572b069dd47c586f0f192f83b08e5", // gradio api有，但不知道要做什麼用的，原理是什麼
    "output": 
        {"data": [[["test", ""]]], 
        "is_generating": true, 
        // "duration": 1.0497472286224365, 
        // "average_duration": 1.0497472286224365
        }, 
    // "success": true
    }	
}
``` 

```json
{
    {"msg":"process_completed",
    // "event_id":"dc02961ca6aa47a28ba8b2732fece954",
    "output":
    {"data":

    [[["test","台積電今天召開股東常會，這是將退休的董事長劉德音最後一次主持股東會。他表示，在去年基期較低的狀況下，台積電今年又將是大大成長的一年；人工智慧應用帶動先進半導體需求，是台積電強項，對於未來幾年成長深具信心。"],["myquestion2","台積電今天召開股東常會，這是將退休的董事長劉德音最後一次主持股東會。他表示，在去年基期較低的狀況下，台積電今年又將是大大成長的一年；人工智慧應用帶動先進半導體需求，是台積電強項，對於未來幾年成長深具信心。"]]],
    
    "markdown": "![](http://) p1, p2",

    "is_generating":false,
    "duration":0.0003695487976074219,
    "average_duration":0.09979360774882788,"render_config":null,
    "changed_state_ids":[]},
    "success":true}
}
```

## 機器人回覆回饋 [POST] /v1/response_feedback

### Request
```json
{
    {"session_id" "z4ubh11d6ef":, 
     "index": 0, //哪一個歷史對話
     "feedback": "good" //
    }
}
```