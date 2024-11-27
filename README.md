# first-report
#### 系統需求與資料結構設計
- [ ] 設計系統的基本功能需求
- [ ] 設計單個任務的資料結構
- [ ] 設計儲存多個任務的清單結構
- [ ] 設計數據儲存結構

#### 程式實作進度
- [ ] 設計查看所有任務功能
- [ ] 設計新增任務功能
- [ ] 設計完成任務功能
- [ ] 設計刪除任務功能
- [ ] 設計篩選任務功能（根據優先順序或完成期限）
- [ ] 設計數據儲存功能（儲存到檔案中）
- [ ] 設計數據讀取功能（從檔案讀取數據）

#### 測試進度
- [ ] 測試新增任務
- [ ] 測試查看任務
- [ ] 測試完成任務
- [ ] 測試刪除任務
- [ ] 測試過期任務的篩選功能
pending_tasks = []   # 待完成的任務清單
completed_tasks = [] # 已完成的任務清單
def add_task():
    title = input("請輸入任務名稱：").strip()
    if not title:
        print("任務名稱不可為空！")
        return
    
    description = input("請輸入任務描述（可選）：").strip()
    due_date = input("請輸入完成期限（格式: YYYY-MM-DD，可選）：").strip()

    # 建立新任務
    new_task = {
        "title": title,
        "description": description or None,
        "due_date": due_date or None,
    }
    pending_tasks.append(new_task)
    print(f"\n成功新增任務：{new_task['title']}\n")
def show_tasks():
    print("\n=== 任務清單 ===")
    print("未完成的任務：")
    if not pending_tasks:
        print("\n  目前沒有任何任務！\n")
    else:
        for idx, task in enumerate(pending_tasks, start=1):
            print(f"  {idx}. {task['title']} ({task['description'][:40]})") # 描述部份最多顯示 40 個字元
    
    print("\n已完成的任務：")
    if not completed_tasks:
        print("\n  目前沒有任何任務！\n")
    else:
        for idx, task in enumerate(completed_tasks, start=1):
            print(f"  {idx}. {task['title']} ({task['description'][:40]})") # 描述部份最多顯示 40 個字元
    print()

def main():
    while True:
        print("\n=== To-Do List 管理系統 ===")
        print("1. 顯示任務清單")
        print("2. 新增任務")
        print("3. 離開系統")
        
        choice = input("請選擇功能（輸入數字）：").strip()
        
        if choice == "1":
            show_tasks()
        elif choice == "2":
            add_task()
        elif choice == "3":
            print("感謝使用，再見！")
            break
        else:
            print("無效的選擇，請重新輸入。\n")

# 啟動主程式
main()



