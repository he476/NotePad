# NotePad
This is an AndroidStudio rebuild of google SDK sample NotePad<br>
## 扩展的功能：
NoteList中显示条目增加时间戳显示<br>
添加笔记查询功能（根据标题查询）<br>
UI美化<br>
更改记事本的背景<br>
笔记排序<br>
笔记分享<br>
<br>
## 扩展的功能解析：
#### NoteList中显示条目增加时间戳显示：
在`noteslist_item.xml`中添加一个用于显示时间戳的TextView
```
<TextView
            android:id="@+id/text1_time"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:paddingLeft="5dip"
            android:textColor="#000000"
            />
```
已知数据库定义中已存在修改时间和创建时间的项：
```
db.execSQL("CREATE TABLE " + NotePad.Notes.TABLE_NAME + " ("
                   + NotePad.Notes._ID + " INTEGER PRIMARY KEY,"
                   + NotePad.Notes.COLUMN_NAME_TITLE + " TEXT,"
                   + NotePad.Notes.COLUMN_NAME_NOTE + " TEXT,"
                   + NotePad.Notes.COLUMN_NAME_CREATE_DATE + " INTEGER,"
                   + NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE + " INTEGER,"
                   + ");");
```
使用修改时间为时间戳显示，在NoteList中找到PROJECTION添加一个元素
```
private static final String[] PROJECTION = new String[] {
            NotePad.Notes._ID, // 0
            NotePad.Notes.COLUMN_NAME_TITLE, // 1
            NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,//添加该行
    };
```
修改dataColumns,viewIDs添加时间，然后用Cursor从数据库中取出数据
```
private String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE ,
                    NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE
} ;
// The view IDs that will display the cursor columns, initialized to the TextView in
private int[] viewIDs = { android.R.id.text1,R.id.text1_time };
```
修改时间的显示方式:在NotePadProvider中显示的`insert方法`和NoteEditor中的`updateNote`方法中修改`创建时间`和`修改时间`的格式
```
Long nowTime=Long.valueOf(System.currentTimeMillis());
Date date=new Date(nowTime);
SimpleDateFormat format=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String time=format.format(date);
```
```
values.put(NotePad.Notes.COLUMN_NAME_CREATE_DATE, time);
```
结果如下：
![image](https://github.com/guodongxiaren/ImageCache/raw/master/Logo/foryou.gif)




