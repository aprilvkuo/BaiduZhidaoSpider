QA
==

问答系统，目前还只针对百度知道

QA系统使用说明

1.数据库说明：
	数据库中的字段跟师兄给我的一样，只有两个评论字段没有下载。
另外为了实现师兄所说的多线程下载，我在query表，qapair_resultslist表中添加了finished字段，用来表示该条记录是否已经下载过，为1时表示已经下载过，该记录的所对应的网页都已下载到本地，只要调用相应的分析程序即可;为0表示还没有下载。

2. 系统使用说明：
	可以有两个方法来执行该程序。
	1）运行Main函数（该函数在com.hitsz.main中）
	只要数据库中的query表中有查询问句，并且finished字段为0,该函数就会将query中的问句列表逐一的去下载，并将网页结果保存到本地，将分析结果保存到数据库中。中间不需要任何干预，除非出现了还没有考虑到的错误。
	整个系统目前使用的还是单线程，因此在下载网页时可能会比较慢，为了防止由于频繁访问百度出现一些错误界面。

	2）运行test.com.hitsz.util 下的TestBaidu.java
	该类当中有四个函数，从上到下分别是四个功能模块，在类中已有说明。
	当数据库中的query表中已经导入了问题列表，就可以逐步运行下面的函数即可，和第一种方法运行的结果一样。
	/**
	 * 第一步运行这个函数，将每个问句的搜索页面保存到本地
	 * 
	 * 测试下载每个问句的baidu搜索网页
	 */
	public void testDownloadItems()
	
	/**
	 * 第二步 分析第一步下载下来的文件，将搜索的每个item存入到数据库qapair_resultlist中 
	 * 
	 * 
	 * 分析每个问题的搜索网页，并将每个结果存入到数据库中
	 * 对每个问题解析完之后，需要对TermList清空
	 * 
	 */
	public void testParseItemPages()
	/**
	 * 第三步 在第二步后，在从数据库中的qapair_resultlist中选出出那些还没有下载过的	items，保存到一个list中
	 * 		对每个url的list遍历，逐一下载对应的问答网页，然后以问题的id命名文件，		保存到本地
	 * 
	 *  从数据库qapair_resultlist中获取所有 还没有下载过
	 *  的item的URL，然后进行下载
	 */
	public void testDownLoadQAPages()

	/**
	 * 
	 * 第四步，将第三步中保存下来的问答对网页逐一分析，本将每个问答对网页结果存入到数据库	qapair,baiduuser中
	 * 
	 * 分析每个问答对的网页
	 * 并保存到数据库中
	 */
	public void testParseQAPage()

	3其他使用说明：
	1)为了能记录运行过程，该系统使用了Log日志文件，在log文件夹下，
	log4j.properties 是log记录文件的配置文件，但程序在运行出错时，可以查询该log下的qa.log文件，
	查询运行记录即可，以便做一些恢复措施。

	2）在config文件下，有一个系统配置文件url.properties
	里面含有各种系统配置信息，主要就是数据库的URL地址和密码等
	还有在下载网页时的延迟配置。
	
	3）在sql文件夹下的qa.sql文件则是建立空的数据库的sql语句，直接当该文件导入到mysql中即可，不需要建立qa数据库。
	
	4)问句的搜索结果页面保存在resource/baidu文件夹下。
	每个问答对网页保存在resource/qapair文件夹下。