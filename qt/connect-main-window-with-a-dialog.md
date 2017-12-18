# Connect main window with a dialog

## Create a dialog

dialog는 new를 통해 생성한다. Qt는 알아서 Memory를 관리하므로 Memory Leak을 걱정할 필요는 없다. dialog 종료 시 결과를 받아오기 위해서는 `finished(int)` signal을 slot에 연결한다.

```cc
Dialog* dialog = new Dialog;
dialog->show();
QObject::connect(dialog, SIGNAL(finished(int)), this, SLOT(on_dialog_finished(int)));
```

## Close the dialog

dialog 종료 시 결과를 받아올 수 있다. `OK` 버튼을 누른 경우 `QDialog::Accepted`를, `Cancel` 버튼을 누른 경우 `QDialog::Rejected`를 반환한다.

```cc
void MainWindow::on_dialog_finished(int result)
{
    switch (result)
    {
    case QDialog::Accepted:
        qDebug() << "Accepted";
        break;

    case QDialog::Rejected:
        qDebug() << "Rejected";
        break;
    }
}
```
