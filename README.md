from PyQt6.QtWidgets import (
    QApplication, QLabel, QLineEdit,
    QPushButton, QVBoxLayout, QWidget, QMessageBox
)
import sys

class MainWindow(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("待辦事項")
        self.resize(400, 300)

        # 元件：標籤、輸入框、按鈕
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
        self.delete_tasks_button.clicked.connect(self.delete_tasks_text)
        self.complete_tasks_button.clicked.connect(self.complete_tasks_text)
        self.add_task_button.clicked.connect(self.add_task_text)
        self.quit_system_button.clicked.connect(self.quit_system_text)
       
    def add_task_text(self):
        self.label = QLabel("請新增任務：", self)
        self.task_name_input_field = QLineEdit(self)
        self.task_name_input_field.setPlaceholderText("在這裡輸入任務名稱...")
        self.task_discribe_input_field = QLineEdit(self)
        self.task_discribe_input_field.setPlaceholderText("請輸入任務描述")
        self.task_deadline_input_field = QLineEdit(self)
        self.task_deadline_input_field.setPlaceholderText("請輸入完成期限（格式: YYYY-MM-DD")
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
        text = self.task_name_input_field.text()
        text1 = self.task_discribe_input_field.text()
        text2 = self.task_deadline_input_field.text()
        if text.strip():
                self.label.setText("f"成功新增：{text}"")
                new_task = {
                            "title": text,
                            "description": text1 or '',
                            "due_date": text2 or '',
                            }
                            pending_tasks.append(new_task)
        else:
                QMessageBox.warning(self, "警告", "輸入欄位為空！")

# 主程式入口
if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec())

    
pending_tasks = []   # 待完成的任務清單
completed_tasks = [] # 已完成的任務清單
def show_tasks_text(self):
    print("\n=== 任務清單 ===")
    print("1. 未完成的任務：")
    print("2. 已完成的任務：")
    choice2 = input("請選擇任務清單（輸入數字）：").strip()
    if choice2 == "1":
        print("\n=== 未完成的任務 ===")
        if not pending_tasks:
            print("\n  目前沒有任何任務！\n")
        else:
            for idx, task in enumerate(pending_tasks, start=1):
                print(f"  {idx}. {task['title']} ({task['description'][:40]})") # 描述部份最多顯示 40 個字元
    if choice2 == "2":
        print("\n=== 已完成的任務 ===")
        if not completed_tasks:
            print("\n  目前沒有任何任務！\n")
        else:
            for idx, task in enumerate(completed_tasks, start=1):
                print(f"  {idx}. {task['title']} ({task['description'][:40]})") # 描述部份最多顯示 40 個字元
    print()
def delete_task_text(self):
    print("\n=== 刪除任務 ===")
    task_type = int(input("請選擇任務類型（1: 未完成, 2: 已完成）：").strip())
    if  task_type==1:
        print("\n=== 未完成的任務 ===")
        if not pending_tasks:
            print("\n  目前沒有任何任務！\n")
            return
        else:
            for idx, task in enumerate(pending_tasks, start=1):
                print(f"  {idx}. {task['title']} ({task['description'][:40]})") # 描述部份最多顯示 40 個字元
                task_list = pending_tasks 
    elif task_type==2:
        print("\n=== 已完成的任務 ===")
        if not completed_tasks:
            print("\n  目前沒有任何任務！\n")
            return
        else:
            for idx, task in enumerate(completed_tasks, start=1):
                print(f"  {idx}. {task['title']} ({task['description'][:40]})") # 描述部份最多顯示 40 個字元
                task_list = completed_tasks
    else:
        print("\n無效的選擇！請輸入 1 或 2。\n")
        return
    try:
        task_idx = int(input("請輸入要刪除的任務編號：")) - 1
        if 0 <= task_idx < len(task_list):
            task = task_list.pop(task_idx)
            print(f"\n成功刪除任務：{task['title']}\n")
        else:
            print("\n無效的編號！請重新選擇。\n")
    except ValueError:
        print("\n輸入無效！請輸入數字。\n")
def complete_task_text(self):
    if not pending_tasks:
        print("\n目前沒有未完成的任務！\n")
        return
    else:    
        print("\n=== 未完成的任務 ===")
        for idx, task in enumerate(pending_tasks, start=1):
            print(f"  {idx}. {task['title']} ({task['description'][:40]})") # 描述部份最多顯示 40 個字元
        task_idx = int(input("請輸入要完成的任務編號：")) - 1
        if 0 <= task_idx < len(pending_tasks):
            task = pending_tasks.pop(task_idx)
            completed_tasks.append(task)
            print(f"\n成功完成任務：{task['title']}\n")
        else:
            print("\n無效的編號！請重新選擇。\n")

def main():
    while True:
        print("\n=== To-Do List 管理系統 ===")
        print("1. 顯示任務清單")
        print("2. 新增任務")
        print("3. 完成任務")
        print("4. 刪除任務")
        print("5. 離開系統")
        
        choice = input("請選擇功能（輸入數字）：").strip()
        
        if choice == "1":
            show_tasks()
        elif choice == "2":
            add_task()
        elif choice == "3":
            complete_task()
        elif choice == "4":
            delete_task()
        elif choice == "5":
            print("感謝使用，再見！")
            break
        else:
            print("無效的選擇，請重新輸入。\n")

# 啟動主程式
main()

