#-----------------------------CONNECTING MYSQL----------------------------
from tkinter import *
from tkinter import ttk
from PIL import ImageTk,Image
import tkinter.messagebox as mb   
import mysql.connector as f

root=Tk()              #to generate a window    
root.geometry("300x300")
root.title("Login")  #to change the title to login


con=f.connect(host='localhost',user='root',password='12345',database='shop')
cur=con.cursor()
cur.execute('create table if not exists sweet(ORDER_NO int,NAME_OF_CUSTOMER varchar(20),PHONE_NO bigint,PASTRY_TYPE varchar(20),DELIVERY_DATE date,PLACE_OF_DELIVERY varchar(20),COST bigint)')

#------------------------------------LOGIN-------------------------------

root.config(bg='pink')
title=Label(root,font='bold',text='SWEETPAN',borderwidth=10,relief=RIDGE)
title.pack() 
Label(root,text='Username',font=('Calibri',10),bg='pink').place(x=20,y=70)
id2=Entry(root)
id2.place(x=150,y=70)
Label(root,text='Password',font=('Calibri',10),bg='pink').place(x=20,y=110)    
id4=Entry(root)
id4.place(x=150,y=110)
id4.config(show='*')  #to hide the password
photo2=Image.open("C:\\Users\\Anil Paul\\Downloads\\pic.png")
photo2=photo2.resize((100,80))
photo2=ImageTk.PhotoImage(photo2)
Label(root,image=photo2).place(x=25,y=180)
a=Button(root,text='Login',bg='pink',command=login)   
a.place(x=215,y=180)
a2=Button(root,text='Logout',bg='pink',command=logout)
a2.place(x=210,y=225)
root.mainloop()

#------------------------------------MENU-------------------------------------------

def login():
    name=id2.get()
    password=id4.get()
    if name=='sweetpan' and password=='2004':
        mb.showinfo("",'Login Successful!!')
        isu=Toplevel(root)
        isu.title('Choice')
        isu.config(bg='pink')
        isu.geometry('350x250')
        smh=Label(isu,font='bold',text='Choose:',borderwidth=5,relief=RIDGE)
        smh.pack()
        Button(isu,text='Orders',bg='pink',width=15,command=tig).place(x=120,y=90)
        Button(isu,text='Products',bg='pink',width=15,command=pro).place(x=120,y=120)
        Button(isu,text='Report',bg='pink',width=15,command=report).place(x=120,y=150)
        isu.mainloop()
    else:
        mb.showinfo("","Username and Password didn't match")


def tig():
    new=Toplevel(root)   #to open another window to proceed      
    new.title('Noshary System')
    photo=Image.open("C:\\Users\\Anil Paul\\Downloads\\download.png")
    photo=photo.resize((200,150))
    photo=ImageTk.PhotoImage(photo)
    Label(new,text='Bliss in every bite.....').grid(row=1,sticky=N)
    Label(new,image=photo).grid(row=2,sticky=N,pady=15)
    Button(new,text='New Order',bg='pink',width=15,command=order).grid(row=3,sticky=N)
    Button(new,text='Update Order',bg='pink',width=20,command=update).grid(row=4,sticky=N)
    Button(new,text='Delete Order',bg='pink',width=15,command=delete).grid(row=5,sticky=N)            
    Button(new,text='Display Orders',bg='pink',width=20,command=display).grid(row=7,sticky=N)
    Button(new,text='Search Order',bg='pink',width=20,command=search).grid(row=6,sticky=N)
    new.mainloop()   #tells python to run the tkinter window.

#---------------------------INSERTING ORDERS-------------------------------



def order():
    global i
    i=Toplevel(root)
    i.title('Order')
    i.geometry('600x350')
    i.resizable(width=0,height=0)
    frame=Frame(i,bg='pink')
    frame.pack(expand=True,fill='both') #expand=widget to fill the space        
    title=Label(frame,font='bold',text="Order Placement" ,borderwidth=10,relief=RIDGE)
    title.pack()   #to organise the label into a block
    global i1,i2,i3,i4,i5,i6,i7
    qq='select max(ORDER_NO)+1 from sweet'
    cur.execute(qq)
    oo=cur.fetchone()
    hh=oo[0]
    Label(frame,text='Order Number',borderwidth=7).place(x=10,y=60)
    i1=Entry(frame,borderwidth=7)
    i1.place(x=140,y=60)
    i1.insert(0,hh)
    
    Label(frame,text='Name',borderwidth=7).place(x=10,y=100)
    i2=Entry(frame,borderwidth=7)
    i2.place(x=140,y=100)
    Label(frame,text='Phone Number',borderwidth=7).place(x=10,y=140)
    i3=Entry(frame,borderwidth=7)
    i3.place(x=140,y=140)
    Label(frame,text='Type of Pastry',borderwidth=7).place(x=10,y=180)
    i4=Entry(frame,borderwidth=7)
    i4.place(x=140,y=180)
    Label(frame,text='Date of Delivery',borderwidth=7).place(x=10,y=220)
    i5=Entry(frame,borderwidth=7)
    i5.place(x=140,y=220)
    Label(frame,text='Place of Delivery',borderwidth=7).place(x=10,y=260)
    i6=Entry(frame,borderwidth=7)
    i6.place(x=140,y=260)
    Label(frame,text='Cost',borderwidth=7).place(x=10,y=300)
    i7=Entry(frame,borderwidth=7)
    i7.place(x=140,y=300)
    Button(frame,text='Add',width=15,command=add).place(x=500,y=140)
    i.mainloop()

def add():
    no=i1.get()
    name=i2.get()
    phone=i3.get()
    type=i4.get()
    date=i5.get()
    place=i6.get()
    cost=i7.get()
    
    query="insert into sweet values({},'{}',{},'{}','{}','{}',{})".format(no,name,phone,type,date,place,cost)
    cur.execute(query)
    con.commit()
    mb.showinfo("",'Data inserted successfully..')
    i.destroy()

#---------------------------UPDATING ORDERS-----------------------------

def update():
    global u 
    u=Toplevel(root)
    u.title('Update')
    u.geometry('600x350')
    u.resizable(width=0,height=0)
    gk=Frame(u,bg='pink') #to group and arrange widgets. 
    gk.pack(expand=True,fill='both')
    gk=Label(gk,font='bold',text='Update Orders',borderwidth=10,relief=RIDGE)
    gk.pack()
    global l1
    Label(u,text='Order Number',borderwidth=7).place(x=10,y=60)
    l1=Entry(u,borderwidth=7)
    l1.place(x=140,y=60)
    Button(u,text='Enter',bg='pink',width=15,command=enter).place(x=450,y=60)
    u.mainloop()

def enter():
    fr=l1.get()
    cur.execute('select * from sweet where ORDER_NO={}'.format(fr))
    k=cur.fetchone()
    n=k[1]
    p=k[2]
    t=k[3]
    d=k[4]
    pl=k[5]
    c=k[6]
    global s1,s2,s3,s4,s5,s6
    Label(u,text='Name',borderwidth=7).place(x=10,y=100)
    s1=Entry(u,borderwidth=7)
    s1.place(x=140,y=100)
    s1.insert(0,n) #to insert the updated record to the specified index
    Label(u,text='Phone Number',borderwidth=7).place(x=10,y=140)
    s2=Entry(u,borderwidth=7)
    s2.place(x=140,y=140)
    s2.insert(0,p) #to overwrite the previous data as any index before first character is treated as 0
    Label(u,text='Type of Pastry',borderwidth=7).place(x=10,y=180)
    s3=Entry(u,borderwidth=7)
    s3.place(x=140,y=180)
    s3.insert(0,t)
    Label(u,text='Date of Delivery',borderwidth=7).place(x=10,y=220)
    s4=Entry(u,borderwidth=7)
    s4.place(x=140,y=220)
    s4.insert(0,d)
    Label(u,text='Place of Delivery',borderwidth=7).place(x=10,y=260)
    s5=Entry(u,borderwidth=7)
    s5.place(x=140,y=260)
    s5.insert(0,pl)
    Label(u,text='Cost',borderwidth=7).place(x=10,y=300)
    s6=Entry(u,borderwidth=7)
    s6.place(x=140,y=300)
    s6.insert(0,c)
    Button(u,text='Update',bg='pink',width=15,command=now).place(x=450,y=100)

def now():
    fr2=l1.get()
    w1=s1.get()
    w2=s2.get()
    w3=s3.get()
    w4=s4.get()
    w5=s5.get()
    w6=s6.get()
    q="update sweet set  NAME_OF_CUSTOMER='{}’ ,PHONE_NO={},PASTRY_TYPE='{}', DELIVERY_DATE='{},' PLACE_OF_DELIVERY='{}',COST={} where ORDER_NO={}".format(w1,w2,w3,w4,w5,w6,fr2)
    cur.execute(q)
    con.commit()
    mb.showinfo("",'Updated Successfully')
    u.destroy()

#--------------------------------DELETING ORDERS-----------------------

def delete():
    a=Toplevel(root)
    a.title('Delete')
    a.geometry('600x300')
    a.resizable(width=0,height=0)
    frame9=Frame(a,bg='pink')
    frame9.pack(expand=True,fill='both')
    title5=Label(frame9,font='bold',text='Delete Record',borderwidth=10,
    relief=RIDGE)
    title5.pack()
    global p1
    Label(frame9,text='Order Number',borderwidth=7).place(x=10,y=60)
    p1=Entry(frame9,borderwidth=7)
    p1.place(x=140,y=60)
    Button(a,text='Delete',bg='pink',width=15,command=dlt).place(x=450,y=100)
    a.mainloop()

def dlt():
    f=p1.get()
    q3="delete from sweet where ORDER_NO={}".format(f)
    cur.execute(q3)
    con.commit()
    mb.showinfo("",'Deleted successfully')

#--------------------------------SEARCHING ORDERS--------------------------

def search():
    global c
    c=Toplevel(root)
    c.title('Search')
    c.geometry('600x350')
    c.resizable(width=0,height=0)
    op=Frame(c,bg='pink')
    op.pack(expand=True,fill='both')
    ma=Label(op,font='bold',text='Search Record',borderwidth=10,relief=RIDGE)
    ma.pack()
    Label(c,text='Order Number',borderwidth=7).place(x=10,y=60)
    global say1	
    say1=Entry(c,borderwidth=7)
    say1.place(x=140,y=60)
    Button(c,text='Show',bg='pink',width=15,command=show).place(x=450,y=60)
    c.mainloop()

def show():
    g1=say1.get()
    cur.execute('select * from sweet where ORDER_NO={}'.format(g1))
    cc1=cur.fetchone()
    cc2=cc1[1]
    cc3=cc1[2]
    cc4=cc1[3]
    cc5=cc1[4]
    cc6=cc1[5]
    cc7=cc1[6]
    Label(c,text='Name',borderwidth=7).place(x=10,y=100)
    v1=Entry(c,borderwidth=7)
    v1.place(x=140,y=100)
    v1.insert(0,cc2) #to insert the updated record to the specified index
    Label(c,text='Phone Number',borderwidth=7).place(x=10,y=140)
    v2=Entry(c,borderwidth=7)
    v2.place(x=140,y=140)
    v2.insert(0,cc3)
    Label(c,text='Type of Pastry',borderwidth=7).place(x=10,y=180)
    v3=Entry(c,borderwidth=7)
    v3.place(x=140,y=180)
    v3.insert(0,cc4)
    Label(c,text='Date of Delivery',borderwidth=7).place(x=10,y=220)
    v4=Entry(c,borderwidth=7)
    v4.place(x=140,y=220)
    v4.insert(0,cc5)
    Label(c,text='Place of Delivery',borderwidth=7).place(x=10,y=260)
    v5=Entry(c,borderwidth=7)
    v5.place(x=140,y=260)
    v5.insert(0,cc6)
    Label(c,text='Cost',borderwidth=7).place(x=10,y=300)
    v6=Entry(c,borderwidth=7)
    v6.place(x=140,y=300)
    v6.insert(0,cc7)
    Button(c,text='Ok',bg='pink',width=15,command=close).place(x=450,y=100)

def close():
    c.destroy()

#-----------------------------DISPLAYING ORDERS-----------------------------

def display():
    global b
    b=Toplevel(root)
    b.title('Display')
    b.geometry('850x350')
    b.resizable(width=0,height=0)
    b.config(bg='pink')
    sky=Label(b,font='bold',text='Record',borderwidth=10,relief=RIDGE)
    sky.pack()
    cur.execute('select * from sweet')
    tree=ttk.Treeview(b)
    tree['show']='headings' #to suppress the default column heading in mysql 
    tree['columns']=('ORDER_NO','NAME_OF_CUSTOMER','PHONE_NO',
    'PASTRY_TYPE','DELIVERY_DATE','PLACE_OF_DELIVERY','COST')
    tree.column('ORDER_NO',width=100,minwidth=100)
    tree.column('NAME_OF_CUSTOMER',width=150,minwidth=150)
    tree.column('PHONE_NO',width=100,minwidth=100)
    tree.column('PASTRY_TYPE',width=150,minwidth=150)
    tree.column('DELIVERY_DATE',width=100,minwidth=100)
    tree.column('PLACE_OF_DELIVERY',width=150,minwidth=150)
    tree.column('COST',width=60,minwidth=60)
    tree.heading('NAME_OF_CUSTOMER',text='NAME_OF_CUSTOMER')
    tree.heading('ORDER_NO',text='ORDER_NO')
    tree.heading('PHONE_NO',text='PHONE_NO')
    tree.heading('PASTRY_TYPE',text='PASTRY_TYPE')
    tree.heading('DELIVERY_DATE',text='DELIVERY_DATE')
    tree.heading('PLACE_OF_DELIVERY',text='PLACE_OF_DELIVERY')
    tree.heading('COST',text='COST')
    i=0
    for r in cur:
        tree.insert('',i,text='',values=(r[0],r[1],r[2],r[3],r[4],r[5],r[6]))
        i=i+1
    tree.pack()
    b.mainloop()

#---------------------------DISPLAYING RECORDS-------------------------------

def pro():
    global q
    q=Toplevel(root)
    q.title('Display')
    q.geometry('400x300')
    q.resizable(width=0,height=0)
    q.config(bg='pink')
    pek=Label(q,font='bold',text='Records',borderwidth=10,relief=RIDGE)
    pek.pack()
    cur.execute('select * from products')
    tree2=ttk.Treeview(q)
    tree2['show']='headings' #to suppress the default column heading in mysql and insert the desired  headings 
    tree2['columns']=('NO','PASTRY','COST')
    tree2.column('NO',width=100,minwidth=100)
    tree2.column('PASTRY',width=150,minwidth=150)
    tree2.column('COST',width=100,minwidth=100)
    tree2.heading('NO',text='NO')
    tree2.heading('PASTRY',text='PASTRY')
    tree2.heading('COST',text='COST')
    i4=0
    for rj in cur:
        tree2.insert('',i4,text='',values=(rj[0],rj[1],rj[2]))
        i4=i4+1
    tree2.pack()
    q.mainloop()

#---------------------------DISPLAYING REPORT-------------------------------

def report():
    global m
    m=Toplevel(root)
    m.title('Sales Report')
    m.geometry('600x400')
    m.resizable(width=0,height=0)
    m.config(bg='pink')
    jun1=Label(m,font='bold',text='Sales Report',borderwidth=10,relief=RIDGE)
    jun1.pack()
    #jun2=Label(m,font='bold',text='January',borderwidth=10,relief=RIDGE)
    #jun2.pack()
    ba=Button(m,text='Jan',bg='pink',width='10',height='2',command=jan)   
    ba.place(x=250,y=90)
    ba2=Button(m,text='Feb',bg='pink',width='10',height='2',command=feb)
    ba2.place(x=250,y=150)
    ba3=Button(m,text='March',bg='pink',width='10',height='2',command=
    march)   
    ba3.place(x=250,y=210)

def jan():
    cur.execute('select * from report')
    tree2=ttk.Treeview(m)
    tree2['show']='headings'      
    tree2 ['columns']=('ITEM','SOLD','TOTAL_COST','REVENUE',
    'PROFIT')
    tree2.column('ITEM',width=100,minwidth=100)
    tree2.column('SOLD',width=80,minwidth=80)
    tree2.column('TOTAL_COST',width=150,minwidth=150)
    tree2.column('REVENUE',width=100,minwidth=100)
    tree2.column('PROFIT',width=100,minwidth=100)
    tree2.heading('ITEM',text='ITEM')
    tree2.heading('SOLD',text='SOLD')
    tree2.heading('TOTAL_COST',text='TOTAL_COST')
    tree2.heading('REVENUE',text='REVENUE')
    tree2.heading('PROFIT',text='PROFIT')
    i4=0
    for rj in cur:
        tree2.insert('',i4,text='',values=(rj[0],rj[1],rj[2],rj[3],rj[4]))
        i4=i4+1
    tree2.pack()
    Label(m,text='Total income: 89,700').place(x=100,y=285)
    m.mainloop()

def feb():
    return jan()

def march():
    cur.execute('select * from march')
    tree2=ttk.Treeview(m)
    tree2['show']='headings'         
    tree2  ['columns']=('ITEM','SOLD','TOTAL_COST','REVENUE',
    'PROFIT')
    tree2.column('ITEM',width=140,minwidth=140)
    tree2.column('SOLD',width=80,minwidth=80)
    tree2.column('TOTAL_COST',width=90,minwidth=90)
    tree2.column('REVENUE',width=100,minwidth=100)
    tree2.column('PROFIT',width=100,minwidth=100)
    tree2.heading('ITEM',text='ITEM')
    tree2.heading('SOLD',text='SOLD')
    tree2.heading('TOTAL_COST',text='TOTAL_COST')
    tree2.heading('REVENUE',text='REVENUE')
    tree2.heading('PROFIT',text='PROFIT')
    i4=0
    for rj in cur:
        tree2.insert('',i4,text='',values=(rj[0],rj[1],rj[2],rj[3],rj[4]))
        i4=i4+1
    tree2.pack()
    Label(m,text='Total income: 86,000').place(x=100,y=300)
    m.mainloop()

#-----------------------------------------LOGOUT------------------------------------
def logout():
    mb.showinfo("",'Thank you!')
    root.destroy()
