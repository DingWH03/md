# qml

## 组件

### 分类常用组件表格

### **基础组件**

| **组件名称** | **功能描述**                                           | **常用属性**             |
|--------------|--------------------------------------------------------|--------------------------|
| Rectangle    | 用于绘制矩形或方形，常用作背景或容器。                   | `width`, `height`, `color`, `radius` |
| Text         | 用于显示文本内容，支持多种字体、颜色和对齐方式。         | `text`, `font.family`, `font.pointSize`, `color`, `horizontalAlignment` |
| Image        | 用于显示图像，支持多种格式（如 PNG、JPG、SVG 等）。      | `source`, `fillMode`, `width`, `height` |
| Item         | 所有可视组件的基类，用于作为容器或布局基础。             | `width`, `height`, `x`, `y`, `visible` |

---

### **布局组件**

`import QtQuick.Layouts`

| **组件名称** | **功能描述**                                           | **常用属性**             |
|--------------|--------------------------------------------------------|--------------------------|
| Row          | 水平布局，将子项水平排列。                              | `spacing`, `anchors`     |
| Column       | 垂直布局，将子项垂直排列。                              | `spacing`, `anchors`     |
| Grid         | 网格布局，将子项排列为行和列的形式。                    | `columns`, `rows`, `spacing` |
| Flow         | 流式布局，将子项按水平或垂直优先排列。                  | `flow`, `spacing`, `anchors` |

---

### **交互组件**

`import QtQuick.Controls`

| **组件名称** | **功能描述**                                           | **常用属性**             |
|--------------|--------------------------------------------------------|--------------------------|
| Button       | 按钮组件，常用于用户操作。                              | `text`, `onClicked`, `enabled`, `checkable` |
| CheckBox     | 复选框，用于表示多选状态。                              | `checked`, `onClicked`   |
| RadioButton  | 单选按钮，用于在多个选项中选择一个。                    | `checked`, `onClicked`, `group` |
| Slider       | 滑块组件，用于调整数值范围。                            | `value`, `minimumValue`, `maximumValue`, `stepSize` |
| TextField    | 单行文本输入框。                                        | `text`, `placeholderText`, `onEditingFinished` |
| TextArea     | 多行文本输入框。                                        | `text`, `wrapMode`, `onTextChanged` |
| MouseArea    | 用于检测鼠标事件，例如点击、悬停等。                    | `onClicked`, `onPressed`, `onReleased` |

---

### **高级组件**

| **组件名称** | **功能描述**                                           | **常用属性**             |
|--------------|--------------------------------------------------------|--------------------------|
| ListView     | 用于显示垂直排列的列表内容。                            | `model`, `delegate`, `spacing` |
| GridView     | 用于显示网格排列的内容。                                | `model`, `delegate`, `cellWidth`, `cellHeight` |
| PathView     | 沿路径展示的视图组件。                                  | `model`, `delegate`, `path` |
| Repeater     | 用于重复显示多个相同或类似的组件。                      | `model`, `delegate`      |
| Loader       | 用于动态加载 QML 文件或组件。                          | `source`, `onLoaded`, `visible` |

---

### **动画与效果组件**

| **组件名称**         | **功能描述**                                 | **常用属性**             |
|----------------------|----------------------------------------------|--------------------------|
| PropertyAnimation    | 属性动画，用于平滑改变属性值。                | `target`, `property`, `to`, `duration` |
| SequentialAnimation  | 顺序动画，将多个动画按顺序执行。              | `animations`             |
| ParallelAnimation    | 并行动画，同时执行多个动画。                  | `animations`             |
| OpacityAnimator      | 控制透明度变化的动画。                        | `target`, `from`, `to`, `duration` |
| ShaderEffect         | 使用 GLSL 着色器创建自定义视觉效果。          | `fragmentShader`, `source` |

---

### **图形与绘制组件**

| **组件名称** | **功能描述**                                           | **常用属性**             |
|--------------|--------------------------------------------------------|--------------------------|
| Canvas       | 提供类似 HTML5 的画布功能，用于绘制自定义图形。          | `contextType`, `width`, `height` |
| Path         | 描述矢量路径，用于高级绘制。                            | `startX`, `startY`, `pathElements` |
| PathLine     | 绘制路径中的直线段。                                    | `x`, `y`                |
| PathArc      | 绘制路径中的弧形段。                                    | `x`, `y`, `radius`      |

---

### 布局组件使用方法

在 **Qt Quick** 中，布局组件（如 `Row`、`Column`、`Grid` 和 `Flow`）是用来对子组件进行排布的容器。以下是各个布局组件的具体使用方法和示例：

---

### **1. Row (水平布局)**

- 将子项水平排列，所有子项按从左到右的顺序显示。
- 可以通过 `spacing` 属性调整子项之间的间距。

**示例代码**：

```qml
Row {
    spacing: 10
    anchors.centerIn: parent
    Rectangle { width: 50; height: 50; color: "red" }
    Rectangle { width: 50; height: 50; color: "green" }
    Rectangle { width: 50; height: 50; color: "blue" }
}
```

**关键点**：

- `spacing`：控制子项之间的水平间距。
- 子项按照声明的顺序从左到右排列。

---

### **2. Column (垂直布局)**

- 将子项垂直排列，所有子项按从上到下的顺序显示。
- 也可以通过 `spacing` 属性调整子项之间的垂直间距。

**示例代码**：

```qml
Column {
    spacing: 10
    anchors.centerIn: parent
    Rectangle { width: 50; height: 50; color: "red" }
    Rectangle { width: 50; height: 50; color: "green" }
    Rectangle { width: 50; height: 50; color: "blue" }
}
```

**关键点**：

- `spacing`：控制子项之间的垂直间距。
- 子项按照声明的顺序从上到下排列。

---

### **3. Grid (网格布局)**

- 将子项排列成行和列的形式。
- 可以指定 `columns` 或 `rows` 属性来控制每行/每列的子项数量。

**示例代码**：

```qml
Grid {
    columns: 3
    rowSpacing: 10
    columnSpacing: 10
    anchors.centerIn: parent

    Rectangle { width: 50; height: 50; color: "red" }
    Rectangle { width: 50; height: 50; color: "green" }
    Rectangle { width: 50; height: 50; color: "blue" }
    Rectangle { width: 50; height: 50; color: "yellow" }
    Rectangle { width: 50; height: 50; color: "purple" }
}
```

**关键点**：

- `columns`：指定网格的列数。
- `rowSpacing` 和 `columnSpacing`：设置行间距和列间距。

---

### **4. Flow (流式布局)**

- 子项会根据容器宽度自动换行排列。
- 可以设置 `flow` 属性决定布局方向：  
  - `Flow.LeftToRight`（默认）：从左到右，行满时换行。  
  - `Flow.TopToBottom`：从上到下，列满时换列。

**示例代码**：

```qml
Flow {
    spacing: 10
    anchors.fill: parent

    Rectangle { width: 50; height: 50; color: "red" }
    Rectangle { width: 50; height: 50; color: "green" }
    Rectangle { width: 50; height: 50; color: "blue" }
    Rectangle { width: 50; height: 50; color: "yellow" }
    Rectangle { width: 50; height: 50; color: "purple" }
}
```

**关键点**：

- `spacing`：设置子项间距。
- `flow`：指定子项排列的方向（如从左到右或从上到下）。

---

### **布局中的通用属性**

- **`anchors`**：可以用来定位整个布局组件，比如居中、填充父组件。
- **`spacing`**：子项之间的间距（适用于 Row、Column、Flow 和 Grid）。
- **动态子项**：可以通过 `Repeater` 动态生成布局内的子项。

---

### **动态子项示例（结合布局组件）**

```qml
Row {
    spacing: 5
    Repeater {
        model: 5
        Rectangle { width: 50; height: 50; color: Qt.rgba(Math.random(), Math.random(), Math.random(), 1) }
    }
}
```

上述代码会动态创建 5 个随机颜色的矩形，并使用 `Row` 布局水平排列它们。

---
