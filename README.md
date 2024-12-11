from PyQt6.QtWidgets import (
    QApplication, QLabel, QLineEdit,
    QPushButton, QVBoxLayout, QWidget, QMessageBox
)
import sys

# 初始化任務列表
pending_tasks = []
completed_tasks = []

class MainWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("待辦事項")
        self.resize(400, 300)

        # 設定元件：標籤、按鈕
        self.label = QLabel("請選擇功能：", self)
        self.show_tasks_button = QPushButton("顯示任務", self)
        self.delete_tasks_button = QPushButton("刪除任務", self)
        self.complete_tasks_button = QPushButton("完成任務", self)
        self.add_task_button = QPushButton("新增任務", self)
        self.quit_system_button = QPushButton("離開系統", self)

        # 設定佈局
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.show_tasks_button)
        layout.addWidget(self.add_task_button)
        layout.addWidget(self.complete_tasks_button)
        layout.addWidget(self.delete_tasks_button)
        layout.addWidget(self.quit_system_button)
        self.setLayout(layout)

        # 事件連接
        self.show_tasks_button.clicked.connect(self.show_tasks_text)
        self.add_task_button.clicked.connect(self.add_task_text)
        
    
    def add_task_text(self):
        # 切換到新增任務的畫面
        self.label.setText("請新增任務：")
        self.task_name_input_field = QLineEdit(self)
        self.task_name_input_field.setPlaceholderText("在這裡輸入任務名稱...")
        self.task_discribe_input_field = QLineEdit(self)
        self.task_discribe_input_field.setPlaceholderText("請輸入任務描述")
        self.task_deadline_input_field = QLineEdit(self)
        self.task_deadline_input_field.setPlaceholderText("請輸入完成期限（格式: YYYY-MM-DD）")
        self.complete_input_button = QPushButton("輸入完成", self)
        self.return_button = QPushButton("回主畫面", self)

        # 設定佈局
        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.task_name_input_field)
        layout.addWidget(self.task_discribe_input_field)
        layout.addWidget(self.task_deadline_input_field)
        layout.addWidget(self.complete_input_button)
        layout.addWidget(self.return_button)
        self.setLayout(layout)

        # 事件連接
        self.complete_input_button.clicked.connect(self.complete_input_text)
        self.return_button.clicked.connect(self.return_text)

    def complete_input_text(self):
        # 新增任務
        text = self.task_name_input_field.text()
        text1 = self.task_discribe_input_field.text()
        text2 = self.task_deadline_input_field.text()

        if text.strip():  # 檢查是否有輸入任務名稱
            self.label.setText(f"成功新增：{text}")
            new_task = {
                "title": text,
                "description": text1 or '',
                "due_date": text2 or '',
            }
            pending_tasks.append(new_task)
        else:
            QMessageBox.warning(self, "警告", "輸入欄位為空！")

    def show_tasks_text(self):
        # 顯示任務畫面
        self.label.setText("=== 任務清單 ===")
        self.show_doing_tasks_button = QPushButton("未完成任務清單", self)
        self.show_done_tasks_button = QPushButton("已完成任務清單", self)
        self.return_button = QPushButton("回主畫面", self)

        layout = QVBoxLayout()
        layout.addWidget(self.label)
        layout.addWidget(self.show_doing_tasks_button)
        layout.addWidget(self.show_done_tasks_button)
        layout.addWidget(self.return_button)
        self.setLayout(layout)

        # 事件連接
        self.show_doing_tasks_button.clicked.connect(self.show_doing_tasks_text)
        self.show_done_tasks_button.clicked.connect(self.show_done_tasks_text)
        self.return_button.clicked.connect(self.return_text)

    def show_doing_tasks_text(self):
        if not pending_tasks:
            self.label.setText("=== 未完成的任務 ===\n目前沒有任何任務！")
        else:
            task_list = "\n".join([f"{idx}. {task['title']} ({task['description'][:40]})"
                                  for idx, task in enumerate(pending_tasks, start=1)])
            self.label.setText(f"=== 未完成的任務 ===\n{task_list}")

    def show_done_tasks_text(self):
        if not completed_tasks:
            self.label.setText("=== 已完成的任務 ===\n目前沒有任何任務！")
        else:
            task_list = "\n".join([f"{idx}. {task['title']} ({task['description'][:40]})"
                                  for idx, task in enumerate(completed_tasks, start=1)])
            self.label.setText(f"=== 已完成的任務 ===\n{task_list}")

    def return_text(self):
        # 返回主畫面
        self.setWindowTitle("待辦事項")
        self.resize(400, 300)
        self.label.setText("請選擇功能：")

    def quit_system_text(self):
        # 離開系統
        QApplication.quit()

# 主程式入口
if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec())

    



