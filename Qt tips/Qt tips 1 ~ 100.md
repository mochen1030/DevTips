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

