---
title: 使用EasyExcel导出维度信息
date: 2024-02-19 11:19:52
tags:
index_img: /img/easyexcel.png
---

在工作过程中，遇到了一个比较棘手的文件导出需求，其实主要是由于需要导出的数据没有对应的实体类，并且表头和表头数据都是动态的，所以没办法按照Easyexcel官方文档提供的方法去实现，这里将该需求记录一下。

## EasyExcel

### 复杂写

导出的实体类只有两个字段，这两个字段的值均是不固定的

```java
@Data
public class DimensionModel {
    private String columnKey;
    private String remark;
}
```

设置上面实体类的一些测试数据

```java
private List<DimensionModel> fetchDummyModels() {
    List<DimensionModel> data = Lists.newArrayList();
    DimensionModel model = new DimensionModel();
    model.setColumnKey("function_id");
    model.setRemark("H5_show: 活动页面曝光\n" +
            "tab_show: tab曝光\n" +
            "tab_click: 模块按钮点击\n" +
            "game_show: 玩法区域曝光\n" +
            "egg_click: 宠物蛋按钮点击\n" +
            "background_click: 背景奖励预览点击\n" +
            "levitation_turntable_click: 玩法悬浮入口点击\n" +
            "puzzle_show: 拼图按钮曝光\n" +
            "puzzle_click: 拼图按钮点击\n" +
            "friend_information_show: 好友信息跑马灯曝光\n" +
            "friend_information_click: 好友信息跑马灯点击\n" +
            "send_show: 赠送按钮曝光\n" +
            "send_click: 赠送按钮点击\n" +
            "task_show: 任务按钮曝光\n" +
            "task_click: 任务按钮点击\n" +
            "task_reward_show：任务奖励领取弹窗曝光\n" +
            "background_show: 背景弹窗曝光\n" +
            "obtain_click: 获取按钮点击\n" +
            "share_channel_click: 分享渠道点击\n" +
            "share_click: 分享按钮点击\n" +
            "lottery_one_time_click: 抽1次按钮点击\n" +
            "lottery_ten_time_click: 抽10次按钮点击\n" +
            "win_show: 中奖弹窗曝光\n" +
            "invite_click: 邀请按钮点击");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("H5_source");
    model.setRemark("0：开屏\n" +
            "1：首页活动banner\n" +
            "2：房间轮播入口\n" +
            "3：独立装扮页轮播入口\n" +
            "4：APP push\n" +
            "5：IM push\n" +
            "6：公屏消息\n" +
            "7：闪屏\n" +
            "8：space tab活动banner\"");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("game_id");
    model.setRemark("1:拼图玩法\n" +
            "2:转盘抽奖");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("react_uid");
    model.setRemark("uid统计求赠、赠送、邀请好友信息");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("tabs");
    model.setRemark("1.年度赛程\n" +
            "2.年度荣耀殿堂\n" +
            "3.拼图挑战");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("puzzles");
    model.setRemark("1.拼图1\n" +
            "2.拼图2\n" +
            "3.拼图3\n" +
            "4.拼图4\n" +
            "5.拼图5\n" +
            "6.拼图6\n" +
            "7.拼图7\n" +
            "8.拼图8\n" +
            "9.拼图9");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("puzzle_status");
    model.setRemark("1.获取\n" +
            "2.赠送");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("send_status");
    model.setRemark("1.赠送\n" +
            "2.已赠送");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("tasks");
    model.setRemark("1.每日签到\n" +
            "2.频道活跃\n" +
            "3.送礼\n" +
            "4.赠送好友拼图\n" +
            "5.邀请好友参与");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("task_status");
    model.setRemark("1.去完成\n" +
            "2.领取\n" +
            "3.已完成");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("share_channel");
    model.setRemark("1.WhatsApp\n" +
            "2.Facebook\n" +
            "3.Instagram\n" +
            "4.zalo\n" +
            "5.copylink");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("privilege_id");
    model.setRemark("上报中奖特权id，多个特权id用“,”间隔");
    data.add(model);
    model = new DimensionModel();
    model.setColumnKey("invitations");
    model.setRemark("1.流失好友\n" +
            "2.活跃好友");
    data.add(model);
    return data;
}
```

将表头与数据封装进一个不可变Pair对象dimensionPair中

```java
private Pair<List<List<String>>, List<List<String>>> fetchDummyData() {
    List<DimensionModel> dimensionModels = fetchDummyModels();

    List<List<String>> heads = new ArrayList<>(dimensionModels.size());
    List<String> firstLineDatas = new ArrayList<>(dimensionModels.size());
    for (DimensionModel model : dimensionModels) {
        heads.add(Lists.newArrayList(model.getColumnKey()));
        firstLineDatas.add(model.getRemark());
    }
    List<List<String>> datas = Lists.newArrayList();
    datas.add(firstLineDatas);

    return new ImmutablePair<>(heads, datas);
}
```

设置导出文件的文件名、文件样式等

```java
public void testDynamicHeadWrite() {
    String fileName = String.format("hello-%d.xlsx", System.currentTimeMillis());
    int eventId = 60133231;

    Pair<List<List<String>>, List<List<String>>> dummyData = fetchDummyData();

    // 表格头样式
    WriteCellStyle headCellStyle = new WriteCellStyle();
    WriteFont headFont = new WriteFont();
    headFont.setFontName("微软雅黑");
    headFont.setBold(true); // 粗体
    headFont.setFontHeightInPoints((short) 14);
    // 字体
    headCellStyle.setWriteFont(headFont);
    // 填充前景色
    headCellStyle.setFillForegroundColor(IndexedColors.LIGHT_CORNFLOWER_BLUE.index);
    // 水平居左
    headCellStyle.setHorizontalAlignment(HorizontalAlignment.LEFT);
    // 垂直居中
    headCellStyle.setVerticalAlignment(VerticalAlignment.CENTER);
    // 无边框
    headCellStyle.setBorderBottom(BorderStyle.NONE);
    headCellStyle.setBorderLeft(BorderStyle.NONE);
    headCellStyle.setBorderRight(BorderStyle.NONE);
    headCellStyle.setBorderTop(BorderStyle.NONE);

    // 表格内容样式
    WriteCellStyle contentCellStyle = new WriteCellStyle();
    // 自动换行
    contentCellStyle.setWrapped(true);
    // 宽度自适应
    contentCellStyle.setShrinkToFit(true);
    // 水平居左
    contentCellStyle.setHorizontalAlignment(HorizontalAlignment.LEFT);
    // 垂直居顶
    contentCellStyle.setVerticalAlignment(VerticalAlignment.TOP);

    HorizontalCellStyleStrategy styleStrategy = new HorizontalCellStyleStrategy(headCellStyle, contentCellStyle);

    EasyExcel.write(fileName)
            .registerWriteHandler(styleStrategy)
            .registerWriteHandler(new AutoFitCellWeightStrategy())
            .sheet("event_" + eventId + "_维度测试用例")
            .head(dummyData.getKey())
            .doWrite(dummyData.getValue());
}
```

最后导出的效果如下

![image-20240219153038520](https://zzh-blog-images.oss-cn-guangzhou.aliyuncs.com/img/image-20240219153038520.png)
