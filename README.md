import tkinter
from tkinter import *
from tkinter import messagebox
from tkinter import ttk

screen=Tk()


screen.geometry("%dx%d+%d+%d" % (1000,600,700,600))
screen.title("دانشگاه گيلان")
screen.iconbitmap("img/uni1.ico")
screen.resizable(False,False)

#تعريف توابع

def prt():
    print("click shod")

UserAll=[]
def Register(user):
    if int (user ["age"])>=17 :
        if Exist(user):
            messagebox.showerror("کد ملي تکراري","اين کاربر قبلا ثبت شده")
            print(Exist(user))
            return False
        else:
            UserAll.append(user)
            messagebox.showinfo("انجام شد", "خوش آمديد ثبت نام انجام شد")
            return True

    else:
        messagebox.showinfo("ثبت نشد","خطا سن")
        return False;


def Exist(Value):
    for item in  UserAll:
        if Value["cod"]==item["cod"]:
            return True
    return False

def onclickRegester():
    name=Name.get()
    family=Family.get()
    cod=Cod.get()
    sex=Sex.get()
    age=Age.get()
    pedar=Pedar.get()
    reshte=Reshte.get()
    maghta=Maghta.get()
    sal=Sal.get()
    shahr=Shahr.get()
    us = {"Name": name, "family": family,"cod":cod,"sex":sex,"age": age,"pedar":pedar,"reshte":reshte,"maghta":maghta,"sal":sal,"shahr":shahr}
    result = Register(us)
    if result == True:
        listitem = [Name, Family, Cod,Sex,Age,Pedar,Reshte,Maghta,Sal,Shahr]
        insertData(listitem)
        cleare(listitem)
        txtName.focus_set()


def onclickSearch():
    query = txtSearch.get()
    result = Search(query)
    print(result)
    cleantbl()
    Load(result)

def Search(value):
    Listsec=[]
    for item in UserAll:
        if item["Name"]==value or item["family"]==value or item["cod"]==value:
            Listsec.append(item)
    return Listsec

def Load(value):
    for item in value:
        tbl.insert('', "end", text="1", values=[item["shahr"], item["sal"], item["maghta"],item["reshte"], item["pedar"], item["sex"],item["age"],item["cod"],item["family"], item["Name"]])

def cleantbl():
    for item in tbl.get_children():
        sel = (str(item),)
        tbl.delete(sel)

def cleare(listval):
    for item in listval:
        item.set(" ")

#ديتا داخل باکس ها برود داخل جدول

def insertData(value):
    tbl.insert('',"end",text="1",value=[value[9].get(),value[8].get(),value[7].get(),value[6].get(),value[5].get(),value[4].get(),value[3].get(),value[2].get(),value[1].get(),value[0].get()])

#
def Getselection(e):
    selection_row=tbl.selection()
    if selection_row != ():
        listItem = [Name, Family, Cod, Sex, Age, Pedar, Reshte, Maghta, Sal, Shahr]
        cleare(listItem)
        Name.set(tbl.item(selection_row)["values"][9])
        Family.set(tbl.item(selection_row)["values"][8])
        Cod.set(tbl.item(selection_row)["values"][7])
        Sex.set(tbl.item(selection_row)["values"][6])
        Age.set(tbl.item(selection_row)["values"][5])
        Pedar.set(tbl.item(selection_row)["values"][4])
        Reshte.set(tbl.item(selection_row)["values"][3])
        Maghta.set(tbl.item(selection_row)["values"][2])
        Sal.set(tbl.item(selection_row)["values"][1])
        Shahr.set(tbl.item(selection_row)["values"][0])


def onclickDelete():
    result=messagebox.askquestion("هشدار","مطمئن به حذف داده هستيد")
    if result =="yes":
        Delete()

def Delete():
    select_row=tbl.selection()
    if select_row!=():
        SelectItem=tbl.item(select_row)["values"]
        dic={'Name': SelectItem[9], 'family': SelectItem[8],'cod':str(SelectItem[7]),'sex':SelectItem[6],'age': str(SelectItem[5]),'pedar':SelectItem[4],'reshte':SelectItem[3],'maghta':SelectItem[2],'sal':str(SelectItem[1]),'shahr':SelectItem[0]}
        tbl.delete(select_row)
        UserAll.remove(dic)

def onclickEdit():
    select_row = tbl.selection()
    SelectItem = tbl.item(select_row)["values"]
    dic = {'Name': SelectItem[9], 'family': SelectItem[8],'cod':str(SelectItem[7]),'sex':SelectItem[6],'age': str(SelectItem[5]),'pedar':SelectItem[4],'reshte':SelectItem[3],'maghta':SelectItem[2],'sal':str(SelectItem[1]),'shahr':SelectItem[0]}
    indexUs=UserAll.index(dic)
    UserAll[indexUs]={'Name': SelectItem[9], 'family': SelectItem[8],'cod':str(SelectItem[7]),'sex':SelectItem[6],'age': str(SelectItem[5]),'pedar':SelectItem[4],'reshte':SelectItem[3],'maghta':SelectItem[2],'sal':str(SelectItem[1]),'shahr':SelectItem[0]}
    if select_row != ():
        tbl.item(select_row,values=[Shahr.get(),str(Sal.get()),Maghta.get(),Reshte.get(),Pedar.get(),str(Age.get()),Sex.get(),str(Cod.get()),Family.get(),Name.get()])




# تعريف منو

menubar=Menu(screen)
userMenu=Menu(menubar,tearoff=0)
userMenu.add_command(label="ثبت نام کاربر جديد",command=prt)
userMenu.add_command(label="ورود با شماره دانشجويي",command=prt)

menubar.add_cascade(label="ورود کاربر",menu=userMenu)


setingmenu=Menu(screen)
userMenu=Menu(setingmenu,tearoff=0)
setingmenu.add_command(label="پرداخت شهريه",command=prt)
setingmenu.add_command(label="درخواست وام",command=prt)
setingmenu.add_separator()
setingmenu.add_command(label="خروج",command=prt,foreground="red",background="green")

menubar.add_cascade(label="امور مالي",menu=setingmenu)

screen.config(menu=menubar)

##تعريف ليبل

title=Label(screen,text="ثبت نام دانشجويان ورودي ترم يک",width=100,height=1,font="nazanin 10 bold",bg="green",fg="white",padx=10,pady=10,anchor="n").place(x=80,y=2)
#lab

lableName=Label(screen,text="نام").place(x=900,y=50)
lableFamily=Label(screen,text="نام خانوادگي").place(x=900,y=80)
lablecod=Label(screen,text="کد ملي").place(x=900,y=110)
lablesex=Label(screen,text="جنسيت").place(x=920,y=140)
lableage=Label(screen,text="سن").place(x=900,y=170)
lablepedar=Label(screen,text="نام پدر").place(x=900,y=200)
lablereshte=Label(screen,text="رشته تحصيلي").place(x=900,y=230)
lablemaghta=Label(screen,text="مقطع تحصيلي ").place(x=922,y=260)
lablesal=Label(screen,text="سال ورود").place(x=900,y=290)
lableshahr=Label(screen,text="شهر محل سکونت").place(x=900,y=320)


#input

Name=StringVar()
Family=StringVar()
Cod=StringVar()
Sex=StringVar()
Age=StringVar()
Pedar=StringVar()
Reshte=StringVar()
Maghta=StringVar()
Sal=StringVar()
Shahr=StringVar()

#####

txtName=Entry(screen,textvariable=Name,justify="right")
txtName.place(x=770,y=50)
txtFamily=Entry(screen,textvariable=Family,justify="right")
txtFamily.place(x=770,y=80)
txtCod=Entry(screen,textvariable=Cod,justify="right")
txtCod.place(x=770,y=110)
comboTxt=ttk.Combobox(screen,state="readonly",textvariable=Sex,justify="right")
valuesCombo=["زن","مرد"]
comboTxt["value"]=valuesCombo
comboTxt.place(x=770,y=140)
txtAge=Entry(screen,textvariable=Age,justify="right")
txtAge.place(x=770,y=170)
txtPedar=Entry(screen,textvariable=Pedar,justify="right")
txtPedar.place(x=770,y=200)
txtReshte=Entry(screen,textvariable=Reshte,justify="right")
txtReshte.place(x=770,y=230)
comboTxt=ttk.Combobox(screen,state="readonly",textvariable=Maghta,justify="right")
valuesCombo=["کارداني","کارشناسي","کارشناسي ارشد","دکتري"]
comboTxt["value"]=valuesCombo
comboTxt.place(x=770,y=260)
txtSal=Entry(screen,textvariable=Sal,justify="right")
txtSal.place(x=770,y=290)
txtShahr=Entry(screen,textvariable=Shahr,justify="right")
txtShahr.place(x=770,y=320)
#ايجاد باکس سرچ


txtSearch=Entry(screen)
txtSearch.place(x=70,y=70)

#دکمه ها

btn1=Button(screen,text="ثبت نام",command=onclickRegester).place(x=800,y=350)
btn2=Button(screen,text="search",command=onclickSearch).place(x=20,y=67)
btn3=Button(screen,text="حذف",bg="red",fg="black",command=onclickDelete).place(x=860,y=350)
btn4=Button(screen,text="ويرايش",bg="green",fg="black",command=onclickEdit).place(x=900,y=350)

#ساخت جدول

tbl = ttk.Treeview(screen, columns=("c1", "c2", "c3","c4","c5","c6","c7","c8","c9","c10"), show="headings", height=20)

# تنظيم ستون‌ها

tbl.column("c1", width=100)
tbl.heading("c1", text="شهر محل سکونت")

tbl.column("c2", width=70)
tbl.heading("c2", text="سال ورود")

tbl.column("c3", width=100)
tbl.heading("c3", text="مقطع تحصيلي")

tbl.column("c4", width=100)
tbl.heading("c4", text="رشته تحصيلي")

tbl.column("c5", width=50)
tbl.heading("c5", text="نام پدر")

tbl.column("c6", width=70)
tbl.heading("c6", text="سن")

tbl.column("c7", width=50)
tbl.heading("c7", text="جنسيت")

tbl.column("c8", width=70)
tbl.heading("c8", text="کد ملي")

tbl.column("c9", width=70)
tbl.heading("c9", text="نام خانوادگي")

tbl.column("c10", width=50)
tbl.heading("c10", text="نام")



tbl.bind("<Button-1>",Getselection)

tbl.place(x=10, y=380)
tbl.insert('',"end",text="1",value=["","","","","","","","","",""])

screen.mainloop()


