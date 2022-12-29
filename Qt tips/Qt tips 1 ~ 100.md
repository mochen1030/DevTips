#### 1. QTableWidget 键盘搜素

QTableWidget 有个 keyboardSearch 的虚函数，在表格上打字会自动锁定到相应单元格，可重写该函数去除。

```
protected:
    void keyboardSearch(const QString &search);

void QCTableWidget::keyboardSearch(const QString &search)
{
    Q_UNUSED(search);
}
```



#### 2. QComboBox 悬浮状态异常

QComboBox 设置鼠标悬浮状态，在选择 item 后，有几率不显示常规状态而显示悬浮状态。

选择 item 后，会调用自身 hidePopup 函数，这个函数是个虚函数，在关闭下拉菜单时调用 Qt::WA_UnderMouse 属性即可。

```
protected:
    void hidePopup();
    
void QCComboBox::hidePopup()
{
    QComboBox::hidePopup();
    setAttribute(Qt::WA_UnderMouse, false);
}
```



#### 3. QComboBox 设置 item 高度

暂时没发现 QComboBox 可以通过 QSS 样式表直接设置 item 高度，可以给 QComboBox  重新设置一个 QListView ，然后再通过 QSS 样式表设置 QListView  的 item 高度。

```
ui->comboBox->setView(new QListView());

QComboBox QAbstractItemView 
{
	...
}
QComboBox QAbstractItemView::item
{
	...
	min-height:40px;
}
```



#### 4.  槽函数重载

槽函数有重载时，在使用 Qt5 的方式连接信号槽时需要强类型转换，转换成带参数类型的函数指针。

```
connect(sender, &ClassA::signal, recevier, static_cast<void (ClassB::*)()>(&ClassB::slot));
```



