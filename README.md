# PyQt5 待辦事項應用程式

## 功能說明

1. **主選單功能**：
   - 顯示任務（檢視未完成與已完成的任務）。
   - 新增任務。
   - 標記任務為完成。
   - 刪除任務。
   - 離開應用程式。

1. **任務管理**：
   - 每個任務包含以下屬性：
     - **名稱** (`title`)
     - **描述** (`description`)
     - **完成期限** (`due_date`)
   - 任務分為兩個清單：
     - `pending_tasks`（未完成任務）
     - `completed_tasks`（已完成任務）

1. **任務顯示**：
   - 以列表格式顯示任務。
   - 區分未完成任務與已完成任務。

1. **任務完成**：
   - 使用者可以從未完成清單中選擇任務，標記為完成。

1. **任務刪除**：
   - 使用者可以刪除未完成或已完成清單中的任務。

1. **動態界面**：
   - 根據使用者操作動態更新界面。

---

## 程式架構

### 主類別：`MainWindow`

#### 方法
- **`__init__`**：初始化主視窗。
```python
  def __init__(self):
        super().__init__()
        self.setWindowTitle("待辦事項")
        self.resize(400, 300)
        # 初始化主畫面布局
        self.init_main_layout()
```
- **`init_main_layout`**：初始化主選單布局。
```python  
  def init_main_layout(self):
        """初始化主畫面布局"""
        self.clear_layout()
        self.label = QLabel("請選擇功能：", self)
        self.show_tasks_button = QPushButton("顯示任務", self)
        self.add_task_button = QPushButton("新增任務", self)
        self.complete_tasks_button = QPushButton("完成任務", self)
        self.delete_tasks_button = QPushButton("删除任務", self)
        self.quit_system_button = QPushButton("離開系统", self)
        # 布局設置
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.show_tasks_button)
        layout.addWidget(self.add_task_button)
        layout.addWidget(self.complete_tasks_button)
        layout.addWidget(self.delete_tasks_button)
        layout.addWidget(self.quit_system_button)
        self.setLayout(layout)
        # 信號連接
        self.show_tasks_button.clicked.connect(self.show_tasks)
        self.add_task_button.clicked.connect(self.add_task)
        self.complete_tasks_button.clicked.connect(self.complete_task)
        self.delete_tasks_button.clicked.connect(self.delete_task)
        self.quit_system_button.clicked.connect(QApplication.quit)
```
- **`add_task`**：切換到新增任務介面。
- ```python  
   def add_task(self):
        """新增任務畫面"""
        self.clear_layout()
        self.label = QLabel("請輸入任務信息：", self)
        self.task_name_input = QLineEdit(self)
        self.task_name_input.setPlaceholderText("任務名稱")
        self.task_description_input = QLineEdit(self)
        self.task_description_input.setPlaceholderText("任务描述")
        self.task_deadline_input = QLineEdit(self)
        self.task_deadline_input.setPlaceholderText("完成期限 (YYYY-MM-DD)")
        self.add_button = QPushButton("添加任務", self)
        self.return_button = QPushButton("返回主畫面", self)
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.task_name_input)
        layout.addWidget(self.task_description_input)
        layout.addWidget(self.task_deadline_input)
        layout.addWidget(self.add_button)
        layout.addWidget(self.return_button)
        self.setLayout(layout)
        # 信號連接
        self.add_button.clicked.connect(self.add_task_confirm)
        self.return_button.clicked.connect(self.init_main_layout)
 def add_task_confirm(self):
        """确認添加任務"""
        name = self.task_name_input.text().strip()
        description = self.task_description_input.text().strip()
        deadline = self.task_deadline_input.text().strip()
        if not name:
            QMessageBox.warning(self, "警告", "任務名稱不能為空！")
            return
        new_task = {"title": name, "description": description, "due_date": deadline}
        pending_tasks.append(new_task)
        QMessageBox.information(self, "提示", f"任務 '{name}' 添加成功！")
        self.init_main_layout()
```
- **`show_tasks`**：切換到顯示任務介面。
```python  
  def show_tasks(self):
        """顯示任務介面"""
        self.clear_layout()
        self.label = QLabel("任務清單：", self)
        self.pending_tasks_button = QPushButton("未完成任務", self)
        self.completed_tasks_button = QPushButton("已完成任務", self)
        self.return_button = QPushButton("返回主畫面", self)
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.pending_tasks_button)
        layout.addWidget(self.completed_tasks_button)
        layout.addWidget(self.return_button)
        self.setLayout(layout)
        # 信号连接
        self.pending_tasks_button.clicked.connect(self.show_pending_tasks)
        self.completed_tasks_button.clicked.connect(self.show_completed_tasks)
        self.return_button.clicked.connect(self.init_main_layout)
```
- **`show_pending_tasks`**：顯示未完成任務。
```python 
  def show_pending_tasks(self):
        """顯示未完成任務"""
        self.display_tasks("未完成任務", pending_tasks)
```
- **`show_completed_tasks`**：顯示已完成任務。
```python 
    def show_completed_tasks(self):
        """顯示已完成任務"""
        self.display_tasks("已完成任務", completed_tasks)
```
- **`display_tasks`**：通用任務顯示介面。
- **`complete_task`**：切換到完成任務介面。
- ```python  
  def complete_task(self):
        """完成任務介面"""
        self.modify_task("完成任務", pending_tasks, completed_tasks, "完成")
```
- **`delete_task`**：切換到刪除任務介面。
```python  
 def delete_task(self):
        """完成任務介面"""
        self.modify_task("完成任務", pending_tasks, completed_tasks, "完成")

```
- **`modify_task`**：通用任務修改介面（適用於完成與刪除）。
```python  
  def modify_task(self, action, source_list, target_list, action_name):
        """通用任務修改介面"""
        self.clear_layout()
        task_list = "\n".join(
            [f"{idx + 1}. {task['title']} ({task['description']}) - {task['due_date']}" for idx, task in enumerate(source_list)]
        )
        self.label = QLabel(f"{action}：\n{task_list if task_list else '暫無任務'}", self)
        self.task_index_input = QLineEdit(self)
        self.task_index_input.setPlaceholderText(f"輸入任務編號{action_name}...")
        self.confirm_button = QPushButton(f"確認{action_name}", self)
        self.return_button = QPushButton("返回主畫面", self)
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.task_index_input)
        layout.addWidget(self.confirm_button)
        layout.addWidget(self.return_button)
        self.setLayout(layout)
        def confirm_action():
            try:
                index = int(self.task_index_input.text().strip()) - 1
                if index < 0 or index >= len(source_list):
                    raise ValueError
                task = source_list.pop(index)
                if target_list is not None:
                    target_list.append(task)
                if target_list is None:
                    for task_list in [pending_tasks, completed_tasks]:
                        if task in task_list:
                            task_list.remove(task)
                QMessageBox.information(self, "提示", f"任務 '{task['title']}' {action_name}成功！")
                self.init_main_layout()
            except (ValueError, IndexError):
                QMessageBox.warning(self, "警告", "請輸入有效的編號！")
        self.confirm_button.clicked.connect(confirm_action)
        self.return_button.clicked.connect(self.init_main_layout)
```
- **`clear_layout`**：清空當前介面的所有布局與元件。
```python  
def clear_layout(self):
        """清空當前布局"""
        if self.layout() is not None:
            while self.layout().count():
                child = self.layout().takeAt(0)
                if child.widget() is not None:
                    child.widget().deleteLater()
            QWidget().setLayout(self.layout())
```

---

## 程式範例（主程序入口）

```python
# 主程序入口
if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec())

```python
import sys
from PyQt5.QtWidgets import (
    QApplication, QWidget, QMessageBox, QLabel,
    QLineEdit, QPushButton, QVBoxLayout
)
# 初始化任務列表
pending_tasks = []
completed_tasks = []
class MainWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("待辦事項")
        self.resize(400, 300)
        # 初始化主畫面布局
        self.init_main_layout()
    def init_main_layout(self):
        """初始化主畫面布局"""
        self.clear_layout()
        self.label = QLabel("請選擇功能：", self)
        self.show_tasks_button = QPushButton("顯示任務", self)
        self.add_task_button = QPushButton("新增任務", self)
        self.complete_tasks_button = QPushButton("完成任務", self)
        self.delete_tasks_button = QPushButton("删除任務", self)
        self.quit_system_button = QPushButton("離開系统", self)
        # 布局設置
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.show_tasks_button)
        layout.addWidget(self.add_task_button)
        layout.addWidget(self.complete_tasks_button)
        layout.addWidget(self.delete_tasks_button)
        layout.addWidget(self.quit_system_button)
        self.setLayout(layout)
        # 信號連接
        self.show_tasks_button.clicked.connect(self.show_tasks)
        self.add_task_button.clicked.connect(self.add_task)
        self.complete_tasks_button.clicked.connect(self.complete_task)
        self.delete_tasks_button.clicked.connect(self.delete_task)
        self.quit_system_button.clicked.connect(QApplication.quit)

    def add_task(self):
        """新增任務畫面"""
        self.clear_layout()
        self.label = QLabel("請輸入任務信息：", self)
        self.task_name_input = QLineEdit(self)
        self.task_name_input.setPlaceholderText("任務名稱")
        self.task_description_input = QLineEdit(self)
        self.task_description_input.setPlaceholderText("任务描述")
        self.task_deadline_input = QLineEdit(self)
        self.task_deadline_input.setPlaceholderText("完成期限 (YYYY-MM-DD)")
        self.add_button = QPushButton("添加任務", self)
        self.return_button = QPushButton("返回主畫面", self)
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.task_name_input)
        layout.addWidget(self.task_description_input)
        layout.addWidget(self.task_deadline_input)
        layout.addWidget(self.add_button)
        layout.addWidget(self.return_button)
        self.setLayout(layout)
        # 信號連接
        self.add_button.clicked.connect(self.add_task_confirm)
        self.return_button.clicked.connect(self.init_main_layout)
    def add_task_confirm(self):
        """确認添加任務"""
        name = self.task_name_input.text().strip()
        description = self.task_description_input.text().strip()
        deadline = self.task_deadline_input.text().strip()
        if not name:
            QMessageBox.warning(self, "警告", "任務名稱不能為空！")
            return
        new_task = {"title": name, "description": description, "due_date": deadline}
        pending_tasks.append(new_task)
        QMessageBox.information(self, "提示", f"任務 '{name}' 添加成功！")
        self.init_main_layout()
    def show_tasks(self):
        """顯示任務介面"""
        self.clear_layout()
        self.label = QLabel("任務清單：", self)
        self.pending_tasks_button = QPushButton("未完成任務", self)
        self.completed_tasks_button = QPushButton("已完成任務", self)
        self.return_button = QPushButton("返回主畫面", self)
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.pending_tasks_button)
        layout.addWidget(self.completed_tasks_button)
        layout.addWidget(self.return_button)
        self.setLayout(layout)
        # 信号连接
        self.pending_tasks_button.clicked.connect(self.show_pending_tasks)
        self.completed_tasks_button.clicked.connect(self.show_completed_tasks)
        self.return_button.clicked.connect(self.init_main_layout)
    def show_pending_tasks(self):
        """顯示未完成任務"""
        self.display_tasks("未完成任務", pending_tasks)
    def show_completed_tasks(self):
        """顯示已完成任務"""
        self.display_tasks("已完成任務", completed_tasks)
    def display_tasks(self, title, tasks):
        """通用任務顯示介面"""
        self.clear_layout()
        task_list = "\n".join(
            [f"{idx + 1}. {task['title']} ({task['description']}) - {task['due_date']}" for idx, task in enumerate(tasks)]
        )
        self.label = QLabel(f"{title}：\n{task_list if task_list else '暂無任務'}", self)
        self.return_button = QPushButton("返回任務清單", self)
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.return_button)
        self.setLayout(layout)
        # 信號連接
        self.return_button.clicked.connect(self.show_tasks)
    def complete_task(self):
        """完成任務介面"""
        self.modify_task("完成任務", pending_tasks, completed_tasks, "完成")

    def delete_task(self):
         """删除任務介面"""
         self.modify_task("刪除任務", pending_tasks + completed_tasks, [], "删除")
    def modify_task(self, action, source_list, target_list, action_name):
        """通用任務修改介面"""
        self.clear_layout()
        task_list = "\n".join(
            [f"{idx + 1}. {task['title']} ({task['description']}) - {task['due_date']}" for idx, task in enumerate(source_list)]
        )
        self.label = QLabel(f"{action}：\n{task_list if task_list else '暫無任務'}", self)
        self.task_index_input = QLineEdit(self)
        self.task_index_input.setPlaceholderText(f"輸入任務編號{action_name}...")
        self.confirm_button = QPushButton(f"確認{action_name}", self)
        self.return_button = QPushButton("返回主畫面", self)
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.task_index_input)
        layout.addWidget(self.confirm_button)
        layout.addWidget(self.return_button)
        self.setLayout(layout)
        def confirm_action():
            try:
                index = int(self.task_index_input.text().strip()) - 1
                if index < 0 or index >= len(source_list):
                    raise ValueError
                task = source_list.pop(index)
                if target_list is not None:
                    target_list.append(task)
                if target_list is None:
                    for task_list in [pending_tasks, completed_tasks]:
                        if task in task_list:
                            task_list.remove(task)
                QMessageBox.information(self, "提示", f"任務 '{task['title']}' {action_name}成功！")
                self.init_main_layout()
            except (ValueError, IndexError):
                QMessageBox.warning(self, "警告", "請輸入有效的編號！")
        self.confirm_button.clicked.connect(confirm_action)
        self.return_button.clicked.connect(self.init_main_layout)
    def clear_layout(self):
        """清空當前布局"""
        if self.layout() is not None:
            while self.layout().count():
                child = self.layout().takeAt(0)
                if child.widget() is not None:
                    child.widget().deleteLater()
            QWidget().setLayout(self.layout())
# 主程序入口
if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec())
```


