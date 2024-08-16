# [script、script async 和 script defer 的区别](https://github.com/Daotin/fe-interview-2024/issues/2)

1. **`script`**: 
   - 默认情况下同步执行，会阻塞页面解析。
   - **应用场景**：一般用于必须在页面加载前执行的脚本，如动态内容生成。

2. **`script async`**: 
   - 异步执行，脚本下载完成后立即执行，不会阻塞页面解析。
   - **应用场景**：适用于独立于其他脚本或DOM的脚本，如广告或统计脚本。

3. **`script defer`**: 
   - 异步执行，但会在HTML解析完成后、`DOMContentLoaded`事件前按顺序执行。
   - **应用场景**：用于需要在DOM解析完后执行的脚本，如初始化脚本。

**注意事项**：
- `async`和`defer`只对外部脚本有效，内联脚本无效。
- 使用`defer`时，确保脚本间的执行顺序。