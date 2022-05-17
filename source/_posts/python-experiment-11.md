---
title: Python 实验十一 Tkinter的使用（2）
date: 2021-04-05 17:53:43
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的
掌握界面程序的设计
# 实验内容
## 练习一
### 题目：用户登陆界面程序
编写一个用户登录界面，用户可以登录账户信息，如果账户已经存在，可以直接登录，登录名或者登录密码输入错误会提示，如果账户不存在，提示用户注册，点击注册进去注册页面，输入注册信息，确定后便可以返回登录界面进行登录。
### 代码
```python
import pickle
import tkinter as tk
import tkinter.messagebox

window = tk.Tk()

window.title('登 录')

window.geometry('500x400')

l1 = tk.Label(window,text='用户名：',font= 12).place(x=100,y=82)
l2 = tk.Label(window,text='密  码：',font= 12).place(x=100,y=152)

var_user_name = tk.StringVar()
entry_user_name = tk.Entry(window,textvariable=var_user_name,font= 18,width=30,bd =5)
entry_user_name.place(x=180,y=80)

var_user_password = tk.StringVar()
entry_user_password = tk.Entry(window,textvariable=var_user_password,show='*',font= 18,width=30,bd =5)
entry_user_password.place(x=180,y=150)

def user_login_in():
    user_name = var_user_name.get()
    user_password = var_user_password.get()
    try:
        with open('user_info.pickle','rb') as user_file:
            user_info = pickle.load(user_file)
    except FileNotFoundError:
        with open('user_info.pickle','wb') as user_file:
            user_info = {'admin':'admin'}
            pickle.dump(user_info,user_file)
    if user_name ==' ' or user_password == ' ':
        tk.messagebox.showerror(message='用户名或密码不能为空！')
    elif user_name in user_info:
        if user_password == user_info[user_name]:
            tk.messagebox.showinfo(title='welcome',message='欢迎您：'+user_name)
        else:
            tk.messagebox.showerror(message='密码错误！')
    else:
        is_signup = tk.messagebox.askyesno('欢迎','您还没有注册，是否现在注册')
        if is_signup:
            user_sign_up()
def user_sign_up():
    def registration():
        input_name = new_name.get()
        input_password = new_password.get()
        input_password_confirm =new_password_confirm.get()
        try:
            with open('user_info.pickle','rb') as user_file:
                exist_user_info = pickle.load((user_file))
        except FileNotFoundError:
            exist_user_info = {}
        if input_name in exist_user_info:
            tk.messagebox.showerror('错误','用户名已存在！')
        elif input_password =='' or input_name =='':
            tk.messagebox.showerror('错误','用户名或密码不能为空！')
        elif input_password != input_password_confirm:
            tk.messagebox.showerror('错误','密码前后不一致！')
        else:
            exist_user_info[input_name] = input_password
            with open('user_info.pickle','wb') as user_file:
                pickle.dump(exist_user_info,user_file)
            tk.messagebox.showinfo('欢迎','注册成功！')
            window_sign_up.destroy()
    window_sign_up = tk.Toplevel(window)
    window_sign_up.geometry('350x200')
    window_sign_up.title('注册')

    new_name = tk.StringVar()
    tk.Label(window_sign_up,text='用户名：').place(x=10,y=10)
    tk.Entry(window_sign_up,textvariable=new_name).place(x=150,y=10)

    new_password = tk.StringVar()
    tk.Label(window_sign_up,text='请输入密码：').place(x=10,y=50)
    tk.Entry(window_sign_up,textvariable=new_password,show='*').place(x=150,y=50)

    new_password_confirm = tk.StringVar()
    tk.Label(window_sign_up,text='请再次确认密码：').place(x=10,y=90)
    tk.Entry(window_sign_up,textvariable=new_password_confirm,show='*').place(x=150,y=90)

    bt_confirm_sign_up = tk.Button(window_sign_up,text='确认注册',command=registration)
    bt_confirm_sign_up.place(x=150,y=130)
def user_sign_out():
    window.destroy()
bt_login = tk.Button(window,text='登    录',width=30,font= 18,bg='DarkTurquoise',activebackground='Turquoise',command=user_login_in)
bt_login.place(x=150,y=210)

bt_register = tk.Button(window,text='注册',width=25,command= user_sign_up)
bt_register.place(x=150,y=270)

bt_exit = tk.Button(window,text='退出',width=5,command = user_sign_out)
bt_exit.place(x=350,y=270)
window.mainloop()
```
## 练习二
### 题目：tkinter 版猜数游戏
使用 Python 标准库 tkinter 编写 GUI 版本的猜数游戏。 每次猜数之前要启动游戏并设置猜数范围和最大猜测次数等参数， 退出游戏时显示战绩（共玩几次， 猜对几次） 信息。
### 代码
```python
import tkinter as tk
import random

number = random.randint(0,1024)
running = True
num = 0
num_max = 1024
num_min = 0

def eBtnClose(event):
    root.destory()

def eBtnGuess(evnet):
    global num_max
    global num_min
    global num
    global running
    if running:
        val_a = int(entry_a.get())
        if val_a ==number:
            labelqval("恭喜你答对啦！")
            num +=1
            running =False
            numGuess()
        elif val_a <number:
            if val_a >num_min:
                num_min = val_a
                num +=1
                label_tip_min.config(label_tip_min,text=num_min)
            labelqval("小了哦")
        else:
            if val_a < num_max:
                num_max = val_a
                num +=1
                label_tip_max.config(label_tip_max,text=num_max)
            labelqval("大了哦")
    else:
        labelqval('你已经答对了')
def numGuess():
    if num ==1:
        labelqval('居然一次就答对了')
    elif num<10:
        labelqval('十次以内就答对了，继续加油！尝试次数为：'+str(num))
    elif num<50:
        labelqval('很不错！尝试次数为：'+str(num))
def labelqval(vText):
    label_val_q.config(label_val_q,text=vText)

root = tk.Tk(className="猜数游戏")
root.geometry("400x50")
line_a_tip = tk.Frame(root)
label_tip_max = tk.Label(line_a_tip,text=num_max)
label_tip_min = tk.Label(line_a_tip,text=num_min)
label_tip_max.pack(side="top",fill="x")
label_tip_min.pack(side="bottom",fill="x")
line_a_tip.pack(side="left",fill="y")
line_question = tk.Frame(root)
label_val_q = tk.Label(line_question,width="80")
label_val_q.pack(side="left")
line_question.pack(side="top",fill="x")
line_input = tk.Frame(root)
entry_a =tk.Entry(line_input,width="40")
btnGuess = tk.Button(line_input,text = "猜")
entry_a.pack(side="left")
entry_a.bind('<Return>',eBtnGuess)
btnGuess.bind(('<Button-1>'),eBtnGuess)
btnGuess.pack(side="left")
line_input.pack(side="top",fill="x")
line_btn = tk.Frame(root)
btnClose = tk.Button(line_btn,text="关闭")
btnClose.bind('<Button-1>',eBtnClose)
btnClose.pack(side="left")
line_btn.pack(side="top")
labelqval("请输入0到1024之间任意整数：")
entry_a.focus_set()
root.mainloop()
```
