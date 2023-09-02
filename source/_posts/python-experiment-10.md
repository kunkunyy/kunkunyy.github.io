---
title: Python 实验十 Tkinter的使用（1）
date: 2021-04-05 17:49:55
tags:
- python-experiment
- experiment
categories:
- experiment
---

# 实验目的
1. 掌握 tkinter 的使用
2. 熟悉可视化界面的设计方法
# 实验内容
### 题目
完成以下代码，熟悉 tkinter 各个部件的使用
### 代码
```python
#(1) 创建主窗口及 Label 部件（标签）创建使用
import tkinter as tk
# SY11-1
window = tk.Tk()
window.title('First Example')
window.geometry('500x300')
l = tk.Label(window,text ='你好！欢迎使用Python tkinter!',bg = 'AliceBlue',font = ('Arial',12), width=30, height=2)
l.pack()
window.mainloop()

#（2）Button 窗口部件
import tkinter as tk

window = tk.Tk()
window.title('First Example')
window.geometry('500x300')
var = tk.StringVar()
l = tk.Label(window,textvariable = var,bg = 'AliceBlue',
    font = ('Arial',12), width=30, height=2)
l.pack()
on_hit = False
def touch():
    global on_hit
    if on_hit == False:
        on_hit = True
        var.set('你点击了确认按钮')
    else:
        on_hit = False
        var.set('')
b = tk.Button(window,text='确认',font=('Arial',12),width = 10,
              height=1,command = touch)
b.pack()
window.mainloop()

#（3）Entry 窗口部件
import tkinter as tk

window = tk.Tk()
window.title('SY11-3')
window.geometry('500x300')
e1 = tk.Entry(window,show='*',font=('Arial',14))
e2 = tk.Entry(window,show=None,font=('Arial',14))
e1.pack()
e2.pack()
window.mainloop()

#（4）Text 窗口部件
import tkinter as tk

window = tk.Tk()
window.title('SY11-4')
window.geometry('500x300')
e = tk.Entry(window,show = None)
e.pack()
def insert_point():
    var = e.get()
    t.insert('insert',var)
def insert_end():
    var = e.get()
    t.insert('end',var)
b1 = tk.Button(window,text='insert point',width = 10,
               height = 2,command = insert_point)
b1.pack()
b2 = tk.Button(window,text='insert end',width = 10,
               height = 2,command = insert_end)
b2.pack()
t = tk.Text(window,height = 3)
t.pack()
window.mainloop()

#（5）Canvas 窗口部件
import tkinter as tk # 使用 Tkinter 前需要先导入

window = tk.Tk()
window.title('My Window')
window.geometry('500x300')
canvas = tk.Canvas(window, bg='AliceBlue', height=200, width=500)
image_file = tk.PhotoImage(file='123.gif')
image = canvas.create_image(250, 0, anchor='n',image=image_file)
x0, y0, x1, y1 = 100, 100, 150, 150
line = canvas.create_line(x0-50, y0-50, x1-50, y1-50)
oval = canvas.create_oval(x0+120, y0+50, x1+120, y1+50, fill='yellow')
arc = canvas.create_arc(x0, y0+50, x1, y1+50, start=0, extent=180)
rect = canvas.create_rectangle(330, 30, 330+20, 30+20)
canvas.pack()
def moveit():
    canvas.move(rect, 2, 2)
b = tk.Button(window, text='move item',command=moveit).pack()
window.mainloop()

#（6）messageBox 窗口部件
import tkinter as tk
import tkinter.messagebox
window = tk.Tk()
window.title('My Window')
window.geometry('500x300')
def hit_me():
 tkinter.messagebox.showinfo(title='Hi', message='你好！')
 # tkinter.messagebox.showwarning(title='Hi', message='有警告！') # 提出
 # tkinter.messagebox.showerror(title='Hi', message='出错了！') # 提出错
 # print(tkinter.messagebox.askquestion(title='Hi', message='你好！')) # 询问选择对话窗 return 'yes', 'no'
 # print(tkinter.messagebox.askyesno(title='Hi', message='你好！')) # return'True', 'False'
 # print(tkinter.messagebox.askokcancel(title='Hi', message='你好！')) # return'True', 'False'
tk.Button(window, text='hit me', bg='green', font=('Arial', 14), command=hit_me).pack()
window.mainloop()

#（7）窗口部件三种放置方式 pack/grid/place
#7-1
import tkinter as tk
window = tk.Tk()
window.title('My Window')
window.geometry('500x300')
for i in range(3):
    for j in range(3):
        tk.Label(window, text=1).grid(row=i, column=j, padx=10, pady=10,ipadx=10, ipady=10)
window.mainloop()

#7-2
import tkinter as tk
window = tk.Tk()
window.title('My Window')
window.geometry('500x300')
tk.Label(window, text='P', fg='red').pack(side='top') # 上
tk.Label(window, text='P', fg='red').pack(side='bottom') # 下
tk.Label(window, text='P', fg='red').pack(side='left') # 左
tk.Label(window, text='P', fg='red').pack(side='right') # 右
window.mainloop()

#7-3
import tkinter as tk
window = tk.Tk()
window.title('My Window')
window.geometry('500x300')
tk.Label(window, text='Pl', font=('Arial', 20), ).place(x=50, y=100, anchor='nw')
window.mainloop()
```

