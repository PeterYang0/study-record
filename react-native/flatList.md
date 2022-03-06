#### 属性

- ItemSeparatorComponent - 行与行之间的分隔线

```
_renderItemSeparatorComponent = () => (
   <View style={{ height:1, backgroundColor:'#c2c3c4' }}></View>
);
<FlatList
  data={[{key: 'a', goods: 'banner'}, {key: 'b', goods: 'apple'}]}
  renderItem={({item}) => <Text>{item.goods}</Text>}
  ItemSeparatorComponent={ this._renderItemSeparatorComponent }
/>
```

- ListFooterComponent - 尾部组件

```
_renderFooter = () => (
    <View>
        <Text>
            甄选好货
        </Text>
    </View>
)
<FlatList
  data={[{key: 'a', goods: 'banner'}, {key: 'b', goods: 'apple'}]}
  renderItem={({item}) => <Text>{item.goods}</Text>}
  ListFooterComponent={ this._renderFooter }
/>
```

- ListHeaderComponent - 头部组件

```
_renderHeader = () => (
    <View>
        <Text>
            甄选好货
        </Text>
    </View>
)
<FlatList
  data={[{key: 'a'}, {key: 'b'}]}
  renderItem={({item}) => <Text>{item.key}</Text>}
  ListHeaderComponent={ this._renderHeader }
/>
```

- ListEmptyComponent - 列表为空时呈现的内容

```
_renderEmptyComponent = () => (
    <View>
        <Text>
            数据为空啦！不存在的！
        </Text>
    </View>
)
<FlatList
  data={[{key: 'a', goods: 'banner'}, {key: 'b', goods: 'apple'}]}
  renderItem={({item}) => <Text>{item.goods}</Text>}
  ListFooterComponent={ this._renderEmptyComponent }
/>
```

- data - 数据源
- renderItem - 列表呈现方式
- extraData - 用于告诉列表重新渲染，因为它实现了 PureComponent，但是 props 浅拷贝是不会重新渲染的
- onEndReached - 下拉加载触发的函数
- onEndReachedThreshold - 决定当距离内容最底部还有多远时触发 onEndReached 回调
- onRefresh - 上拉刷新触发的函数
- refreshing - 在等待加载新数据时将此属性设为 true，列表就会显示出一个正在加载的符号
- scrollToEnd
- scrollToIndex
- scrollToItem
- scrollToOffset

demo

```
<FlatList
    style={styles.scrollViewStyle}
    ref={view => {
    this.myFlatList = view;
    }}
    data={this.state.list} // 数据源
    renderItem={this._renderItem} // 从数据源中挨个取出数据并渲染到列表中
    showsVerticalScrollIndicator={false} // 当此属性为true的时候，显示一个垂直方向的滚动条，默认为: true
    showsHorizontalScrollIndicator={false} // 当此属性为true的时候，显示一个水平方向的滚动条，默认为: true
    ItemSeparatorComponent={this._renderSeparator()} // 行与行之间的分隔线组件。不会出现在第一行之前和最后一行之后
    ListEmptyComponent={this._renderListEmptyComp()} // 列表为空时渲染该组件。可以是 React Component, 也可以是一个 render 函数，或者渲染好的 element
    onEndReachedThreshold={0.01} // 决定当距离内容最底部还有多远时触发onEndReached回调，范围0~1，如0.01表示触底时触发
    onEndReached={this._onEndReached} // 在列表底部往下滑时触发该函数。表示当列表被滚动到距离内容最底部不足onEndReachedThreshold的距离时调用
    refreshControl={
    <RefreshControl
        refreshing={this.state.refreshing} // 在等待加载新数据时将此属性设为 true，列表就会显示出一个正在加载的符号
        onRefresh={this._onRefresh.bind(this)} // 在列表顶部往下滑时触发该函数。如果设置了此选项，则会在列表头部添加一个标准的RefreshControl控件，以便实现“下拉刷新”的功能
        tintColor="#ffffff" // 指定刷新指示器的背景色(iOS)
        title="加载中..." // 指定刷新指示器下显示的文字(iOS)
        titleColor="#000000" // 指定刷新指示器下显示的文字的颜色(iOS)
        colors={['#ff0000', '#00ff00', '#0000ff']} // 刷新指示器在刷新期间的过渡颜色(Android)
        progressBackgroundColor="#ffffff" // 指定刷新指示器的背景色(Android)
    />
    }
/>
```
