# 解析XML数据

**解析`xml`如下：**

```xml
<apps>
    <app>
        <id>1</id>
        <name>Google Maps</name>
        <version>1.0</version>
    </app>
    <app>
        <id>2</id>
        <name>Chorme</name>
        <version>2.0</version>
    </app>
    <app>
        <id>3</id>
        <name>Google Play</name>
        <version>3.2</version>
    </app>
</apps>
```

**解析函数如下:**

```java
private void parseXMLWithPull(String data) {
        try {
            XmlPullParserFactory factory = XmlPullParserFactory.newInstance();
            XmlPullParser xmlPullParser = factory.newPullParser();
            //向XmlPullParser中以StringReader的格式放入xml数据
            xmlPullParser.setInput(new StringReader(data));
            //获取第一个节点的类型(XML结束，节点开始，节点结束等)
            int eventType = xmlPullParser.getEventType();
            String id="";
            String name="";
            String version="";
            //当XML没有解析完时继续解析
            while(eventType !=XmlPullParser.END_DOCUMENT){
                //获取节点名称
                String nodeName = xmlPullParser.getName();
                switch (eventType){
                        //如果eventType是节点的开始则获取节点内的内容
                    case XmlPullParser.START_TAG: {
                        if("id".equals(nodeName)){
                            id = xmlPullParser.nextText();
                        }else if("name".equals(nodeName)){
                            name = xmlPullParser.nextText();
                        }else if("version".equals(nodeName)){
                            version = xmlPullParser.nextText();
                        }
                    }
                    break;
                        //如果eventType是节点的结束则打印一个节点的内容
                    case XmlPullParser.END_TAG:{
                        if("app".equals(nodeName)){
                            Log.d("MainActivity","id is "+id);
                            Log.d("MainActivity","name is "+name);
                            Log.d("MainActivity","version is "+version);
                        }
                    }
                    break;
                    default:break;
                }
                //读取下一个节点类型
                eventType = xmlPullParser.next();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

