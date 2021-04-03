# Stream



#### 用法

将list中ChannelName对象中的属性转成map

```java
Map<Long, String> channelNameMap = channelNames.stream().filter(channel -> Objects.nonNull(channel) && Objects.nonNull(channel.getChannelNumId()) && Objects.nonNull(channel.getChannelName())).collect(Collectors.toMap(ChannelName::getChannelNumId, ChannelName::getChannelName, (key1, key2) -> key2));
```

```
Map<String, List<GeneralExcelRow>> generalExcelRowGroup = rows.stream().collect(Collectors.groupingBy(GeneralExcelRow::getExcelB));
```