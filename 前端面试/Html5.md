# Html5

## a标签

a 元素**可以没有** href 属性，此时为超链接的一个占位符

 target 属性可选有：_blank、_self、*framename*等值 

  rel 属性有多个值时，使用空格 " " 分隔 ,<a> 标签的 rel 属性用于指定当前文档与被链接文档的关系。

| 值         | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| alternate  | 文档的可选版本（例如打印页、翻译页或镜像）。                 |
| stylesheet | 文档的外部样式表。                                           |
| start      | 集合中的第一个文档。                                         |
| next       | 集合中的下一个文档。                                         |
| prev       | 集合中的前一个文档。                                         |
| contents   | 文档目录。                                                   |
| index      | 文档索引。                                                   |
| glossary   | 文档中所用字词的术语表或解释。                               |
| copyright  | 包含版权信息的文档。                                         |
| chapter    | 文档的章。                                                   |
| section    | 文档的节。                                                   |
| subsection | 文档的子段。                                                 |
| appendix   | 文档附录。                                                   |
| help       | 帮助文档。                                                   |
| bookmark   | 相关文档。                                                   |
|            |                                                              |
| nofollow   | Google 使用 "nofollow"，用于指定 Google 搜索引擎不要跟踪链接。 |
| licence    |                                                              |
| tag        |                                                              |
| friend     |                                                              |

  href 属性的可选有： 

​               绝对路径（href = "https://[www.baidu.com](https://hd.nowcoder.com/link.html?target=http://www.baidu.com)"） 

​               相对路径（href = "index.html"） 

​               锚（href = "#top"）

## WebStorage和WebSQLDatabase

### WebStorage

HTML5中增加了两种全新的数据存储方式：WebStorage和WebSQLDatabase。 

1. ​    WebStorage可用于临时或永久保存客户端的少量数据，WebSQLDatabase是客户端本地化的一套数据库系统，可以将大量的数据保存在客户端，无须与服务器端进行交互，极大地减轻了服务器端的压力。      
2. ​    WebStorage存储是HTML5为数据存储在客户端提供的一项重要功能，分为两种：sessionStorage(保存会话数据)和localStorage(在客户端长期保存数据)。     

- ​     sessionStorage对象：使用sessionStorage对象在客户端保存的数据时间非常短暂，该数据实质上还是被保存在session对象中。用户在打开浏览器时，可以查看操作过程中要求临时保存的数据，一旦关闭浏览器，所有使用sessionStorage对象保存的数据将全部丢失。     

1. ​    保存：保存数据只需调用setItem()方法，格式：sessionStorage.setItem(key,value)。参数key表示被保存内容的键名，参数value表示被保存的内容。一旦键名设置成功，则不允许修改，不能重复，如果有重复的键名，只能修改对应的键值。      
2. ​    读取：读取被保存的数据，应该调用sessionStorage对象中getItem()方法，格式：sessionStorage.getItem(key)。该方法将返回一个指定键名对应的键值，如果不存在，则返回一个null值。     

- ​    localStorage对象：长期在客户端保存数据，应该使用localStorage对象，使用该对象可以将数据长期保存在客户端，直至人工清除为止。     

1. ​    保存：保存数据调用对象中的setItem()方法，格式：localStorage.setItem(key,value)。      
2. ​    读取：与sessionStorage对象保存数据一样。 读取数据调用对象中的getItem()方法，格式：localStorage.getItem(key)。与sessionStorage对象读取数据一样。 localStorage对象可以将内容长期保存在客户端，即使是重新打开浏览器也不会丢失。      
3. ​    清除：如果需要清除localStorage对象保存的内容，应该调用该对象的另一个方法removeItem()，格式：localStorage.removeItem(key)。一旦删除成功，与键名对应的相应数据将全部被删除。

###  WebSQLDatabase和IndexedDB

|          |                        WebSQLDatabase                        | IndexedDB                                                    |
| :------: | :----------------------------------------------------------: | ------------------------------------------------------------ |
|   优点   | 真正意义上的关系型数据库，类似SQLite（SQLite是遵守ACID的轻型的关系型数据库管理系统） | 允许对象的快速索引和搜索，因此在Web应用程序场景中，您可以非常快速地管理数据以及读取/写入数据。由于是NoSQL数据库，因此我们可以根据实际需求设定我们的JavaScript对象和索引。在异步模式下工作，每个事务具有适度的粒状锁。这允许您在JavaScript的事件驱动模块内工作。 |
|   不足   | 规范不支持啦由于使用SQL语言，因此我们需要掌握和转换我们的JavaScript对象为对应的查询语句。非对象驱动。 | 如果你的世界观里面只有关系型数据库，恐怕不太容易理解。       |
|   位置   |                       包含行和列的表。                       | 包含JavaScript对象和键的存储对象。                           |
| 查询机制 |                             SQL                              | [Cursor APIs](https://developer.mozilla.org/en-US/docs/Web/API/IDBCursor)，[Key Range APIs](https://developer.mozilla.org/en-US/docs/Web/API/IDBKeyRange)，应用程序代码 |
|   事务   |           锁可以发生在数据库，表，行的“读写”时候。           | 锁可以发生在数据库版本变更事务，或是存储对象“只读”和“读写”事务时候。 |
| 事务提交 |       事务创建是显式的。默认是回滚，除非我们调用提交。       | 事务创建是显式的。默认是提交，除非我们调用中止或有一个错误没有被捕获。 |

**WebSQLDatabase和IndexedDB**: https://www.zhangxinxu.com/wordpress/2017/07/html5-indexeddb-js-example/

indexDb使用案列: https://www.zhangxinxu.com/study/201707/indexeddb-example.html

打开控制台，在Application→indexedDB中，我们可以看到当前存储的数据字段和值等信息：

### indexedDB存储和localStorage存储对比

- indexedDB存储IE10+支持，localStorage存储IE8+支持，后者兼容性更好；
- indexedDB存储比较适合键值对较多的数据，我之前不少项目需要存储多个字段，使用的是localStorage存储，结果每次写入和写出都要字符串化和对象化，很麻烦，如果使用indexedDB会轻松很多，因为无需数据转换。
- indexedDB存储可以在workers中使用，localStorage貌似不可以。这就使得在进行PWA开发的时候，数据存储的技术选型落在了indexedDB存储上面。

总结下就是，如果是浏览器主窗体线程开发，同时存储数据结构简单，例如，就存个`true/false`，显然`localStorage`上上选；如果数据结构比较复杂，同时对浏览器兼容性没什么要求，可以考虑使用indexedDB；如果是在Service Workers中开发应用，只能使用indexedDB数据存储。

## SVG和Canvas

SVG          SVG 是一种使用 XML 描述 2D 图形的语言。        SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。        在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。  Canvas        Canvas 通过 JavaScript 来绘制 2D 图形。        Canvas 是逐像素进行渲染的。        在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。  Canvas 与 SVG 的比较        下表列出了 canvas 与 SVG 之间的一些不同之处。        Canvas       依赖分辨率       不支持事件处理器       弱的文本渲染能力       能够以 .png 或 .jpg 格式保存结果图像       最适合图像密集型的游戏，其中的许多对象会被频繁重绘        SVG       不依赖分辨率       支持事件处理器       最适合带有大型渲染区域的应用程序（比如谷歌地图）       复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）       不适合游戏应用

SVG与Canvas的区别
    SVG
      不依赖分辨率
      支持事件绑定
      大型渲染区域的程序(例如百度地图)
      不能用来实现网页游戏
   Canvas
      依赖分辨率
      不支持事件绑定
      最合适网页游戏
      保存为".jpg"格式的图片