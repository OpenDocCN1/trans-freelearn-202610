
现在，在onCreate中，就像我们在Sound Demo应用中所做的那样，在调用setContentView之后，添加代码以特定于版本的方式初始化SoundPool，如下一个代码所示：

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    // Instantiate our sound pool
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
        AudioAttributes audioAttributes = new AudioAttributes.Builder()
            .setUsage(AudioAttributes.USAGE_MEDIA)
            .setContentType(AudioAttributes.CONTENT_TYPE_SONIFICATION)
            .build();

        mSp = new SoundPool.Builder()
            .setMaxStreams(5)
            .setAudioAttributes(audioAttributes)
            .build();
    } else {
        mSp = new SoundPool(5, AudioManager.STREAM_MUSIC, 0);
    }

    try{
        // Create objects of the 2 required classes
        AssetManager assetManager = this.getAssets();
        AssetFileDescriptor descriptor;

        // Load our fx in memory ready for use
        descriptor = assetManager.openFd("beep.ogg");
        mIdBeep = mSp.load(descriptor, 0);
    }catch(IOException e){
        // Print an error message to the console
        Log.e("error", "failed to load sound files");
    }
    mNoteAdapter = new NoteAdapter();

    ListView listNote = (ListView) findViewById(R.id.listView);

    listNote.setAdapter(mNoteAdapter);
    ...
    ...
```

现在，添加下一段代码，它在onItemClick方法中播放声音。我添加了一些上下文并高亮显示了新代码，以清楚地说明它放在哪里：

```
...
public void onItemClick(AdapterView<?> adapter, View view, int
whichItem, long id) {

if(mSound) {
  mSp.play(mIdBeep, 1, 1, 0, 0, 1);
}

Note tempNote = mNoteAdapter.getItem(whichItem);
...
```

好了。现在你可以运行Note To Self应用，每次点击列表中的一条笔记时都会听到令人满意的咔哒声。

### 删除一条笔记 – 引入OnLongClick

作为对Note To Self的最后一项改进，也是为了聊聊一个酷炫的Android UI特性，我们将添加删除笔记的功能。在Android应用中，上下文操作通常通过长按来选择。长按是指用户将手指按在设备屏幕上，而不是简单地点击后抬起手指。

首先，让我们向NoteAdapter类添加一个删除笔记的方法。以下是从数组列表中删除Note对象，然后要求适配器自身及ListView更新的代码：

```
public void deleteNote(int n){
    noteList.remove(n);
    notifyDataSetChanged();
}
```

现在，在onCreate中，我们可以让ListView准备好接受长按，然后使用匿名类来监听并响应长按。这个内部类简单地调用我们的新deleteNote方法。在处理ListView常规点击的匿名类之前以及在调用setAdapter之后添加这段代码。在此，我包含了上下文并高亮显示了要添加的新代码：

```
...
listNote.setAdapter(mNoteAdapter);
// So we can long click it
listNote.setLongClickable(true);
// Now to detect long clicks and delete the note
listNote.setOnItemLongClickListener(new
AdapterView.OnItemLongClickListener() {
public boolean onItemLongClick(AdapterView<?> adapter, View view,
int whichItem, long id) {
// Ask NoteAdapter to delete this entry
mNoteAdapter.deleteNote(whichItem);
return true;
} });
```

```
listNote.setOnItemClickListener(new
AdapterView.OnItemClickListener() {
...
```

现在你可以长按删除一条笔记。在我们即将发布给真实用户的应用中，我们可能希望在删除笔记之前使用弹出对话框窗口来确认用户的决定。

### 常见问题解答

- 1. 如果我想播放音乐，该怎么办？

你可以使用MediaPlayer类在应用后台播放一些音乐。以下是示例代码，展示这有多简单：

```
// Create a MediaPlayer object
MediaPlayer mp = new MediaPlayer();
// Choose where to load the music from
mp.setDataSource(musicFileToLoad);
// Call the prepare method
mp.prepare();
// Play some music
mp.start();
```

有关完整详情，请参阅官方文档：http://developer.android.com/reference/android/media/MediaPlayer.html。

- 2. 如果我想制作一个真正的音乐播放应用，用户可以从自己的库中选择音乐播放，该怎么办？

Android也为我们简化了这一点。在Android Studio调色板上，你可以拖放一个MediaController小部件，它不仅允许用户控制音乐播放，还可以控制视频。在Android开发者网站上了解如何使用此小部件：http://developer.android.com/reference/android/widget/MediaController.html。

### 总结

现在我们知道如何让我们的应用发出声音了。本书中我们构建的大多数应用，你可能只需添加几个提示音或嗡嗡声，但当你设计自己的应用时，你可能想要做得更多，而现在你可以做到了。我们还学习了如何使用调色板中另一个有用的小部件：**Spinner**。

本章结束了Note To Self应用，是时候进入更高级的Android编程了。正如我们将看到的，更高级并不一定意味着更复杂；然而，在本书剩余部分中将介绍相当多的新主题。

在下一章中，我们将看到如何开始使用**Fragment**类编写更多实际应用，以及如何使用**设计模式**来保持不断增长的代码井井有条。

## 第18章 设计模式、Fragment以及实际应用

自本书开始时我们只是在设置Java和Android Studio以来，我们已经走得很远了。那时，我们逐步介绍了所有内容。但随着我们的进展，我们不仅试图向你展示如何给y添加x或给应用b添加功能a，还试图使你能够以自己的方式使用所学知识，从而将自己的想法变为现实。

本章更多地关乎你未来的应用，与迄今在本书中涵盖的任何内容都不同。我们将探讨Java和Android的许多方面，你可以将它们作为框架或模板，在构建更激动人心、更复杂的应用的同时保持代码的可维护性。此外，在本章中，我将建议你可以查阅的进一步学习领域，这些领域超出了本书的范围。

在本章中，你将学习以下主题：

- 模式和模型-视图-控制器模式
- Android设计准则
- 实际设计入门及处理多种不同设备
- Fragments简介

让我们开始吧。

### 介绍模型-视图-控制器模式

MVC是指将应用的不同方面分离到称为**层**的不同部分中。Android应用通常使用模型-视图-控制器**模式**。模式只是一种结构化我们的代码和其他应用资源（如布局文件、图像、数据库等）的公认方式。模式对我们很有用，因为通过遵循模式，我们可以更有信心地做正确的事情，并不太可能因为将自己编程陷入尴尬境地而不得不推翻大量辛苦的工作。

计算机科学中有许多模式，但理解MVC足以创建一些非常专业的Android应用。

我们实际上已经部分使用MVC了，所以让我们依次看看这三个层。

### 模型

模型是指驱动我们的应用的数据，以及专门管理它并使其可供其他层使用的任何逻辑/代码。例如，在我们的Note To Self应用中，Note类及其getter、setter和JSON代码就是数据和逻辑。

### 视图

Note To Self应用的视图是指所有不同布局中的所有小部件。用户在屏幕上可以看到或与之交互的任何内容通常都属于视图。并且，你可能还记得，这些小部件实际上来自Android API的视图类层次结构。
