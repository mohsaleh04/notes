برای تبدیل JSONArray به مثلاً `ArrayList<HoldContact>` میتونیم از این سیستم استفاده کینم:
از TypeToken استفاده میشه.

```java
JSONArray roomMembersJson = jsonObject.getJSONArray(HoldJson.roomMembers);  
Type listType = new TypeToken<ArrayList<HoldContact>>() {  
}.getType();  
roomMembers = new Gson().fromJson(roomMembersJson.toString(), listType);
```


