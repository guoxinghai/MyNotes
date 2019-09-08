# 使用`JSONObject`解析`JSON`数据

**解析以下`json`数据:**

```json
[
{"id":"1",
"name":"Google Maps",
"version":"1.0"
}
,
{"id":"2",
"name":"Chorme",
"version":"2.0"
}
,
{"id":"3",
"name":"Google Play",
"version":"3.2"
}
]
```

**解析代码:**

```java
private void parseJSONWithJSONObject(String data) {
        try {
         	//获取json数组
            JSONArray jsonArray = new JSONArray(data);
            //遍历
            for (int i=0;i<jsonArray.length();i++){
                //通过json数组获取json对象
                JSONObject jsonObject = jsonArray.getJSONObject(i);
                String id = jsonObject.getString("id");
                String name = jsonObject.getString("name");
                String version = jsonObject.getString("version");
                Log.d("MainActivity","id :"+id);
                Log.d("MainActivity","name :"+name);
                Log.d("MainActivity","version :"+version);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

