# CS-499 Final Project

``` python
import sys
import tkinter
from tkinter import messagebox
from tkinter import *
import pymongo
from pymongo import MongoClient

#insert function to create and add documents
def insert():
    #checks that the id has not already been used and added to the database
    num= db.employee.count_documents({"id":e1.get()})
    #if there are no documents with the id already, then the values are inserted as a document into the database
    if num==0:
        db.employee.insert_one(
        {
            "id":e1.get(),
            "Name":e2.get(),
            "Age":e3.get(),
            "Designation":e4.get(),
            "Salary": e5.get(),
            "Address":e6.get(),
        })
    
        messagebox.showinfo("Information","Data inserted sucessfully")
    #if there is already a document with that id then it will not add the values and alerts the user the data was not inserted
    else:
        messagebox.showinfo("Information", "Data could not be inserted")
      
#function to display or read documents already in the database
def display():
    global b
    num= 25
    if num==0:
        messagebox.showinfo("Information", "Data requested does not exist and could not be displayed")
    #Displays the collection contents
    else:
        c=['Name','Age','Designation','Salary','Address']
        r=20
        b=['_id','Name','Age','Designation','Salary','Address']
        num=num+20
        for i in range (0,5):
            Label(window,text=c[i].upper(),bg='light blue').grid(row=19, column=i)
        for post in collection.find():
            r=r+1
            post
            if r<=num:
                for l in range(0,5):
                    name=post[c[l]]
                    b[l]=Label(window,text=name.upper(),bg='light blue')
                    b[l].grid(row=r, column=l)    

#function to update documents using the ID
def update():
    #sets the variable to the id input
    myquery= {"id":e1.get()}

    #checks to see if there are any documents with the id provided by the user
    num=db.employee.count_documents(myquery)

    #if there is no document with the provided id then the user is notified that they cannot update that document
    if num==0:
        print("No record found to update")
    #if there is a document with that id then it gets the values from the rest of the fields and uses them to update the document 
    else:
        newvalues= {"$set":{"Name":e2.get(),"Age":e3.get(),"Designation":e4.get(),"Salary":e5.get(),"Address":e6.get()}}
        if collection.update_many(myquery, newvalues):
            messagebox.showinfo("Information", "Updated the data")
	#the program displays an error if the record could not be updated
        else:
            messagebox.showinfo("Information", "Record not updated")
        
#function to delete documents
def delete():
    #this sets the variable to get the id vlaue from the entry box
    myquery= {"id":e1.get()}

    #finds the number of documents with that id value
    docCount=db.employee.count_documents(myquery)
    #if there are no documents with that id then the program notifies the user the record could not be located and therefore deleted
    if docCount==0:
        messagebox.showinfo("Information", "No record exists to delete")
    #If the function is successful it notifies the user with a message box
    else:
        db.employee.remove(myquery)
        messagebox.showinfo("Information", "Record located and deleted successfully")
        
def clear():
    #clears the entry box values
    e1.delete(0,END)
    e2.delete(0,END)
    e3.delete(0,END)
    e4.delete(0,END)
    e5.delete(0,END)
    e6.delete(0,END)
    
#sets up the connection to mongoDB
conn= pymongo.MongoClient('mongodb://localhost:27017/')

#Sets the database and collection
global db
db= conn.local
collection= db.employee

window= tkinter.Tk()

#Sets the label for the window
Label(window, text="Employee data", bg="light blue").grid(row=0, column=1)

#sets the size and title of the window
window.geometry('600x500')
window.title("Final")

#sets background color to light blue
window.configure(background='light blue')

#Creates the labels for the entry boxes
Label(window,text="ID", bg= "light blue").grid(row=5, column=0)
Label(window,text="NAME", bg= "light blue").grid(row=6, column=0)
Label(window,text="AGE", bg= "light blue").grid(row=7, column=0)
Label(window,text="DESIGNATION", bg= "light blue").grid(row=8, column=0)
Label(window,text="SALARY", bg= "light blue").grid(row=9, column=0)
Label(window,text="ADDRESS", bg= "light blue").grid(row=10, column=0)

#creates the entry boxes for the input
e1= Entry(window)
e2= Entry(window)
e3= Entry(window)
e4= Entry(window)
e5= Entry(window)
e6= Entry(window)

#sets the entry boxes' grid location
e1.focus_set()
e1.grid(row=5, column=1)
e2.grid(row=6, column=1)
e3.grid(row=7, column=1)
e4.grid(row=8, column=1)
e5.grid(row=9, column=1)
e6.grid(row=10, column=1)

#Creates the buttons for the functions
insert= Button(window,text="INSERT", fg="Black", command=insert)
insert.grid(row=5,column=5)

display= Button(window,text="DISPLAY", fg="Black", command=display)
display.grid(row=7,column=5)

update= Button(window,text="UPDATE", fg="Black", command=update)
update.grid(row=9,column=5)

search= Button(window,text="SEARCH+DELETE", fg="Black", command=delete)
search.grid(row=7,column=6)

clear= Button(window,text="CLEAR", fg="Black", command=clear)
clear.grid(row=9,column=6)

#tells python to start the Tkinter loop
window.mainloop()
```
