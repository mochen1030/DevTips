#### 1.Qt 删除布局及其子控件

```
void deleteUiItem(QLayout* layout)
{
    QLayoutItem* item;
    while ((item = layout->takeAt(0)) != nullptr)
    {
        auto widget = item->widget();
        if (widget != nullptr)
        {
            widget->setParent(nullptr);
            delete widget;
        }
        else
        {
            auto layout = item->layout();
            if (layout != nullptr)
            {
                deleteUiItem(layout);
                layout->deleteLater();
            }
        }
        delete item;
    }
}
```

