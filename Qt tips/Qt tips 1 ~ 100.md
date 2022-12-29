#### 1. QTableWidget键盘搜素

QTableWidget 有个 keyboardSearch 的虚函数，在表格上打字会自动锁定到相应单元格，可重写该函数去除。

```
protected:
    void keyboardSearch(const QString &search);

void QCTableWidget ::keyboardSearch(const QString &search)
{
    Q_UNUSED(search);
}
```



#### 2. QComboBox 悬浮状态异常

QComboBox 设置鼠标悬浮状态，在选择 item 后，有几率不显示常规状态而显示悬浮状态。

选择 item 后，会调用自身 hidePopup 函数，这个函数是个虚函数，在关闭下拉菜单时调用 Qt::WA_UnderMouse 属性即可

```
protected:
    void hidePopup();
    
void QCComboBox::hidePopup()
{
    QComboBox::hidePopup();
    setAttribute(Qt::WA_UnderMouse, false);
}
```

