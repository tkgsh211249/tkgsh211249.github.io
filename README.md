# tkgsh211249.github.io
import numpy as np  # 陣列處理
import random
import matplotlib.pyplot as plt  # 圖像顯示
import time  # 增加時間延遲讓遊戲更流暢

# 生成兩個不同的賓果板（1到25的隨機排列）
player_board = np.random.permutation(25).reshape(5, 5) + 1
computer_board = np.random.permutation(25).reshape(5, 5) + 1

player_marks = np.zeros((5, 5), dtype=bool)
computer_marks = np.zeros((5, 5), dtype=bool)

# 繪製賓果板
def plot_board(board, marks, title, color):
    plt.figure(figsize=(6, 6))
    plt.imshow(np.ones((5, 5, 3)), extent=(0, 5, 0, 5))
    plt.title(title, fontsize=16, color=color)

    for i in range(5):
        for j in range(5):
            if marks[i, j]:
                plt.text(j + 0.5, 5 - i - 0.5, 'X', ha='center', va='center', fontsize=20, color=color, fontweight='bold')
                #設定文字的座標j + 0.5 是文本的橫座標，5 - i - 0.5 是文本的縱座標。在該國值上進行了簡單的數學運算（加法），可能有助於調整文字的位置。
                #ha;va 分別是水平和垂直對齊方式，取值 'center' 顯示文字在縱座標中心對齊。
                #設定字號 顏色 字體粗細
                #font字體




            else:
                plt.text(j + 0.5, 5 - i - 0.5, str(board[i, j]), ha='center', va='center', fontsize=15, color='gray')

    plt.xticks([])
    plt.yticks([])
    plt.grid(False)
    plt.show()

# 檢查賓果線
def check_bingo(marks):
    count = 0
    count += sum(np.all(marks, axis=1))  # 檢查橫排
    count += sum(np.all(marks, axis=0))  # 檢查直排
    if np.all(np.diag(marks)):  # 檢查左上到右下
        count += 1
    if np.all(np.diag(np.fliplr(marks))):  # 檢查右上到左下
        count += 1
    return count

# 找最佳選擇（電腦策略）
def find_best_move(board, marks):
    """ 讓電腦變聰明：優先完成自己 3 條線，其次阻止玩家贏，最後隨機選 """
    possible_moves = board[~marks]

    # 1️⃣ 電腦優先完成自己 3 條線
    for number in possible_moves:
        temp_marks = marks.copy()
        temp_marks |= (board == number)
        if check_bingo(temp_marks) >= 3:
            return number

    # 2️⃣ 電腦阻止玩家贏
    for number in possible_moves:
        temp_marks = player_marks.copy()
        temp_marks |= (player_board == number)
        if check_bingo(temp_marks) >= 3:
            return number

    # 3️⃣ 沒有關鍵數字時，隨機選擇
    return random.choice(possible_moves)

# 遊戲開始
print("✨ Let's play Bingo! 🎮")

# 隨機決定誰先開始
turn = random.choice(["player", "computer"])
print(f"🎲 {turn.upper()} 先開始！")

# 顯示初始賓果板
print("🎯 你的賓果板:")
plot_board(player_board, player_marks, "玩家的賓果板", 'blue')

print("🤖 電腦的賓果板:")
plot_board(computer_board, computer_marks, "電腦的賓果板", 'red')

# 遊戲回合
while True:
    if turn == "player":
        # 玩家輸入數字（確保未重複輸入）
        while True:
            try:
                number = int(input("請輸入數字 1~25: "))
                if number < 1 or number > 25:
                    raise ValueError("⚠️ 請輸入 1 到 25 之間的數字")
                if player_marks[player_board == number].any():
                    raise ValueError("⚠️ 這個數字已經被選過了，請選擇其他數字")
                break
            except ValueError as e:
                print(e)

        # 玩家標記數字
        player_marks |= (player_board == number)
        computer_marks |= (computer_board == number)

        # 顯示更新後的賓果板
        print("🎯 你的賓果板:")
        plot_board(player_board, player_marks, "玩家的賓果板", 'blue')

        print("🤖 電腦的賓果板:")
        plot_board(computer_board, computer_marks, "電腦的賓果板", 'red')

        # 檢查是否勝利
        if check_bingo(player_marks) >= 3:
            print("🎉 恭喜你完成 3 條線，贏了！")
            break
        if check_bingo(computer_marks) >= 3:
            print("🤖 電腦完成了 3 條線，贏了！再來挑戰我吧！")
            break

        # 換電腦回合
        turn = "computer"

    else:
        print("🤖 電腦思考中...")
        time.sleep(1)  # 模擬電腦思考時間

        # 電腦選擇最佳數字
        computer_choice = find_best_move(computer_board, computer_marks)
        print(f"🤖 電腦選擇了: {computer_choice}")

        # 電腦標記數字
        computer_marks |= (computer_board == computer_choice)
        player_marks |= (player_board == computer_choice)

        # 顯示更新後的賓果板
        print("🎯 你的賓果板:")
        plot_board(player_board, player_marks, "玩家的賓果板", 'blue')

        print("🤖 電腦的賓果板:")
        plot_board(computer_board, computer_marks, "電腦的賓果板", 'red')

        # 檢查是否勝利
        if check_bingo(player_marks) >= 3:
            print("🎉 恭喜你完成 3 條線，贏了！")
            break
        if check_bingo(computer_marks) >= 3:
            print("🤖 電腦完成了 3 條線，贏了！再來挑戰我吧！")
            break

        # 換玩家回合
        turn = "player"
@app.route("/")
def hello():
    return "Hello, World!"
import numpy as np  # 陣列處理
import random
import matplotlib.pyplot as plt  # 圖像顯示
import time  # 增加時間延遲讓遊戲更流暢

# 生成兩個不同的賓果板（1到25的隨機排列）
player_board = np.random.permutation(25).reshape(5, 5) + 1
computer_board = np.random.permutation(25).reshape(5, 5) + 1

player_marks = np.zeros((5, 5), dtype=bool)
computer_marks = np.zeros((5, 5), dtype=bool)

# 繪製賓果板
def plot_board(board, marks, title, color):
    plt.figure(figsize=(6, 6))
    plt.imshow(np.ones((5, 5, 3)), extent=(0, 5, 0, 5))
    plt.title(title, fontsize=16, color=color)

    for i in range(5):
        for j in range(5):
            if marks[i, j]:
                plt.text(j + 0.5, 5 - i - 0.5, 'X', ha='center', va='center', fontsize=20, color=color, fontweight='bold')
                #設定文字的座標j + 0.5 是文本的橫座標，5 - i - 0.5 是文本的縱座標。在該國值上進行了簡單的數學運算（加法），可能有助於調整文字的位置。
                #ha;va 分別是水平和垂直對齊方式，取值 'center' 顯示文字在縱座標中心對齊。
                #設定字號 顏色 字體粗細
                #font字體




            else:
                plt.text(j + 0.5, 5 - i - 0.5, str(board[i, j]), ha='center', va='center', fontsize=15, color='gray')

    plt.xticks([])
    plt.yticks([])
    plt.grid(False)
    plt.show()

# 檢查賓果線
def check_bingo(marks):
    count = 0
    count += sum(np.all(marks, axis=1))  # 檢查橫排
    count += sum(np.all(marks, axis=0))  # 檢查直排
    if np.all(np.diag(marks)):  # 檢查左上到右下
        count += 1
    if np.all(np.diag(np.fliplr(marks))):  # 檢查右上到左下
        count += 1
    return count

# 找最佳選擇（電腦策略）
def find_best_move(board, marks):
    """ 讓電腦變聰明：優先完成自己 3 條線，其次阻止玩家贏，最後隨機選 """
    possible_moves = board[~marks]

    # 1️⃣ 電腦優先完成自己 3 條線
    for number in possible_moves:
        temp_marks = marks.copy()
        temp_marks |= (board == number)
        if check_bingo(temp_marks) >= 3:
            return number

    # 2️⃣ 電腦阻止玩家贏
    for number in possible_moves:
        temp_marks = player_marks.copy()
        temp_marks |= (player_board == number)
        if check_bingo(temp_marks) >= 3:
            return number

    # 3️⃣ 沒有關鍵數字時，隨機選擇
    return random.choice(possible_moves)

# 遊戲開始
print("✨ Let's play Bingo! 🎮")

# 隨機決定誰先開始
turn = random.choice(["player", "computer"])
print(f"🎲 {turn.upper()} 先開始！")

# 顯示初始賓果板
print("🎯 你的賓果板:")
plot_board(player_board, player_marks, "玩家的賓果板", 'blue')

print("🤖 電腦的賓果板:")
plot_board(computer_board, computer_marks, "電腦的賓果板", 'red')

# 遊戲回合
while True:
    if turn == "player":
        # 玩家輸入數字（確保未重複輸入）
        while True:
            try:
                number = int(input("請輸入數字 1~25: "))
                if number < 1 or number > 25:
                    raise ValueError("⚠️ 請輸入 1 到 25 之間的數字")
                if player_marks[player_board == number].any():
                    raise ValueError("⚠️ 這個數字已經被選過了，請選擇其他數字")
                break
            except ValueError as e:
                print(e)

        # 玩家標記數字
        player_marks |= (player_board == number)
        computer_marks |= (computer_board == number)

        # 顯示更新後的賓果板
        print("🎯 你的賓果板:")
        plot_board(player_board, player_marks, "玩家的賓果板", 'blue')

        print("🤖 電腦的賓果板:")
        plot_board(computer_board, computer_marks, "電腦的賓果板", 'red')

        # 檢查是否勝利
        if check_bingo(player_marks) >= 3:
            print("🎉 恭喜你完成 3 條線，贏了！")
            break
        if check_bingo(computer_marks) >= 3:
            print("🤖 電腦完成了 3 條線，贏了！再來挑戰我吧！")
            break

        # 換電腦回合
        turn = "computer"

    else:
        print("🤖 電腦思考中...")
        time.sleep(1)  # 模擬電腦思考時間

        # 電腦選擇最佳數字
        computer_choice = find_best_move(computer_board, computer_marks)
        print(f"🤖 電腦選擇了: {computer_choice}")

        # 電腦標記數字
        computer_marks |= (computer_board == computer_choice)
        player_marks |= (player_board == computer_choice)

        # 顯示更新後的賓果板
        print("🎯 你的賓果板:")
        plot_board(player_board, player_marks, "玩家的賓果板", 'blue')

        print("🤖 電腦的賓果板:")
        plot_board(computer_board, computer_marks, "電腦的賓果板", 'red')

        # 檢查是否勝利
        if check_bingo(player_marks) >= 3:
            print("🎉 恭喜你完成 3 條線，贏了！")
            break
        if check_bingo(computer_marks) >= 3:
            print("🤖 電腦完成了 3 條線，贏了！再來挑戰我吧！")
            break

        # 換玩家回合
        turn = "player"
@app.route("/")
def hello():
    return "Hello, World!"import numpy as np  # 陣列處理
import random
import matplotlib.pyplot as plt  # 圖像顯示
import time  # 增加時間延遲讓遊戲更流暢

# 生成兩個不同的賓果板（1到25的隨機排列）
player_board = np.random.permutation(25).reshape(5, 5) + 1
computer_board = np.random.permutation(25).reshape(5, 5) + 1

player_marks = np.zeros((5, 5), dtype=bool)
computer_marks = np.zeros((5, 5), dtype=bool)

# 繪製賓果板
def plot_board(board, marks, title, color):
    plt.figure(figsize=(6, 6))
    plt.imshow(np.ones((5, 5, 3)), extent=(0, 5, 0, 5))
    plt.title(title, fontsize=16, color=color)

    for i in range(5):
        for j in range(5):
            if marks[i, j]:
                plt.text(j + 0.5, 5 - i - 0.5, 'X', ha='center', va='center', fontsize=20, color=color, fontweight='bold')
                #設定文字的座標j + 0.5 是文本的橫座標，5 - i - 0.5 是文本的縱座標。在該國值上進行了簡單的數學運算（加法），可能有助於調整文字的位置。
                #ha;va 分別是水平和垂直對齊方式，取值 'center' 顯示文字在縱座標中心對齊。
                #設定字號 顏色 字體粗細
                #font字體




            else:
                plt.text(j + 0.5, 5 - i - 0.5, str(board[i, j]), ha='center', va='center', fontsize=15, color='gray')

    plt.xticks([])
    plt.yticks([])
    plt.grid(False)
    plt.show()

# 檢查賓果線
def check_bingo(marks):
    count = 0
    count += sum(np.all(marks, axis=1))  # 檢查橫排
    count += sum(np.all(marks, axis=0))  # 檢查直排
    if np.all(np.diag(marks)):  # 檢查左上到右下
        count += 1
    if np.all(np.diag(np.fliplr(marks))):  # 檢查右上到左下
        count += 1
    return count

# 找最佳選擇（電腦策略）
def find_best_move(board, marks):
    """ 讓電腦變聰明：優先完成自己 3 條線，其次阻止玩家贏，最後隨機選 """
    possible_moves = board[~marks]

    # 1️⃣ 電腦優先完成自己 3 條線
    for number in possible_moves:
        temp_marks = marks.copy()
        temp_marks |= (board == number)
        if check_bingo(temp_marks) >= 3:
            return number

    # 2️⃣ 電腦阻止玩家贏
    for number in possible_moves:
        temp_marks = player_marks.copy()
        temp_marks |= (player_board == number)
        if check_bingo(temp_marks) >= 3:
            return number

    # 3️⃣ 沒有關鍵數字時，隨機選擇
    return random.choice(possible_moves)

# 遊戲開始
print("✨ Let's play Bingo! 🎮")

# 隨機決定誰先開始
turn = random.choice(["player", "computer"])
print(f"🎲 {turn.upper()} 先開始！")

# 顯示初始賓果板
print("🎯 你的賓果板:")
plot_board(player_board, player_marks, "玩家的賓果板", 'blue')

print("🤖 電腦的賓果板:")
plot_board(computer_board, computer_marks, "電腦的賓果板", 'red')

# 遊戲回合
while True:
    if turn == "player":
        # 玩家輸入數字（確保未重複輸入）
        while True:
            try:
                number = int(input("請輸入數字 1~25: "))
                if number < 1 or number > 25:
                    raise ValueError("⚠️ 請輸入 1 到 25 之間的數字")
                if player_marks[player_board == number].any():
                    raise ValueError("⚠️ 這個數字已經被選過了，請選擇其他數字")
                break
            except ValueError as e:
                print(e)

        # 玩家標記數字
        player_marks |= (player_board == number)
        computer_marks |= (computer_board == number)

        # 顯示更新後的賓果板
        print("🎯 你的賓果板:")
        plot_board(player_board, player_marks, "玩家的賓果板", 'blue')

        print("🤖 電腦的賓果板:")
        plot_board(computer_board, computer_marks, "電腦的賓果板", 'red')

        # 檢查是否勝利
        if check_bingo(player_marks) >= 3:
            print("🎉 恭喜你完成 3 條線，贏了！")
            break
        if check_bingo(computer_marks) >= 3:
            print("🤖 電腦完成了 3 條線，贏了！再來挑戰我吧！")
            break

        # 換電腦回合
        turn = "computer"

    else:
        print("🤖 電腦思考中...")
        time.sleep(1)  # 模擬電腦思考時間

        # 電腦選擇最佳數字
        computer_choice = find_best_move(computer_board, computer_marks)
        print(f"🤖 電腦選擇了: {computer_choice}")

        # 電腦標記數字
        computer_marks |= (computer_board == computer_choice)
        player_marks |= (player_board == computer_choice)

        # 顯示更新後的賓果板
        print("🎯 你的賓果板:")
        plot_board(player_board, player_marks, "玩家的賓果板", 'blue')

        print("🤖 電腦的賓果板:")
        plot_board(computer_board, computer_marks, "電腦的賓果板", 'red')

        # 檢查是否勝利
        if check_bingo(player_marks) >= 3:
            print("🎉 恭喜你完成 3 條線，贏了！")
            break
        if check_bingo(computer_marks) >= 3:
            print("🤖 電腦完成了 3 條線，贏了！再來挑戰我吧！")
            break

        # 換玩家回合
        turn = "player"
