#Name: Ketan Naidu
#Level 2 Computer Science Internal
#Julie's Party Hire Store


#Importing a few modules :)

import tkinter as tk  #The tkinter module provides an easy to implement user interface
from tkinter import ttk
from tkinter import messagebox
import json #The json module allows me to convert lists (or variables) into a JSON format which can be stored externally from the program to save data.


#Predefined variables
row_number = 0
details = []
saved = True


#All functions for buttons and usability are below

def load_details(): #JSON will be used to store data externally 

    global row_number, details #Updates the value of all instances of these variables


    with open('mydata.json', 'r') as json_file: #Opens the JSON file

        details = json.loads(json_file.read()) #All stored data (if there is any) will be put into this list
        
    
        for i in details: #Updates the table
            
            table.insert(parent='', index = row_number, values=i) #Updating treeview with saved list
            row_number += 1


def save_details(event): #This function will be called when the save button is clicked

    global saved

    saved_details = json.dumps(details) #The entire list is being stored
    
    with open('mydata.json', 'w') as json_file:
        
        json_file.write(saved_details) #Adds the saved details to the JSON file
        saved = True
        save_log.configure(text = "Saved", fg='black')


def pop_up(title, text): #There may be a lot of pop up windows so it would be cleaner to keep it as a function
    
    messagebox.showerror(title = title, message = text) #Throws in a popup window



#Checks each individual character to see if there's a number
def number_checker(number):
    
    for i in number: #Checks to see if any of the characters have a number in it
        
        if i.isnumeric():
            
            return True
        
def length_checker(subject): #Checks if a variable or variables are empty or not
    for i in subject:
        if len(i.strip()) == 0:
            return True
    

#Function for submitting details to the table.
def add_to_table(event):

    global saved, row_number, details  #These variables are from outside of this function and will share the same values everywhere else.

    name = False #Booleans for checking if certain requirements are met. Names can only have strings. Numbers can only have integers.
    item = False
    receipt = False

    
    

    #Checks if receipt is valid    
    if number_checker(receipt_box.get()): # The number_checker method which was previously created checks if receipt_box.get() has any numbers
            
        if(int(receipt_box.get()) < 10000000 and int(receipt_box.get()) > 1000000):
            
            if len(details) != 0: #Checks if there is anything in the details list
                
                placeholder = True 

                for i in details:   

                    if placeholder == True:


                        if int(receipt_box.get()) == int(i[2]): #Checks if the receipt number already exists

                            receipt_warning.config(text = "Some row already has this receipt number") #Throws a warning
                            placeholder = False
                            receipt = False #Updates boolean
                        


                        else:

                            receipt_number = receipt_box.get()
                            receipt_warning.config(text = " ")
                            receipt = True

            
            else:

                receipt_number = receipt_box.get()
                receipt_warning.config(text = " ")
                receipt = True
        else: 
                receipt_warning.config(text = "The receipt number should only have 7 characters")
        
    else:
        
        if len(receipt_box.get()) != 0:
            receipt_warning.config(text = "Only enter numbers")
    #Receipt code ends here


    #Checks if name is valid
    if number_checker(name_box.get()):
        name_warning.config(text="There can only be letters in a name", fg = 'red')

    else:

        #Makes sure that only full names are entered in the name.
        number_of_spaces = 0
        for i in name_box.get():
            if i == " ":
                number_of_spaces += 1
        
        if number_of_spaces == 1:
            name = True
            name_warning.config(text = " ")
        else:
            if number_of_spaces > 1:           
                name_warning.config(text="Only enter the first and last name") #Throws a warning
            else: 
                name_warning.config(text="Enter their full name") #Throws a warning
        

    #Name ends here

    #Number of items starts

    if number_checker(number_of_items_box.get()):
        
        if(0 <= int(number_of_items_box.get()) <= 500):
            item = True
            number_of_items_warning.config(text = " ")
        else:
            number_of_items_warning.config(text = "The numbers of items can only be from 0 to 500")
    
    else:
        
        if len(number_of_items_box.get()) == 0: #The length_checker() function doesn't need to be used in some cases.
            item = True
            number_of_items_warning.config(text = " ")

        else:
            number_of_items_warning.config(text = "There can only be numbers")
            
    #Number of items ends here
    #The items text field will accept all values



    #Checks if any of the text fields are empty
    if length_checker([name_box.get(), items.get(), number_of_items_box.get(), receipt_box.get()]):

        log.config(text="Some of your texts are empty... Add something!", fg = 'red')

    else:

        log.config(text=" ")

        if name == True and item == True and receipt == True: 
            #Submitting details on the treeview and the list 
            details.insert(row_number,[row_number, name_box.get(), str(receipt_number), items.get(), number_of_items_box.get()])
            table.insert(parent='', index = row_number, values=details[row_number])
            row_number+=1
            
            #Warns the user that changes are unsaved
            saved = False
            save_log.configure(text = "\n Unsaved \n", fg = 'red')
        
        


#Removes a row
def remove_row(event):

    global row_number, details, saved
   

    selected_item = table.selection()[0] #Selected row

    selected_details = table.item(selected_item) #Storing selected item's details including row values
    

    deleted_list = details[selected_details.get('values')[0]] #Getting a reference to the selected row from within the detail's list

    del details[selected_details.get('values')[0]] #Deleting row from the list

    #Changing the IDs
    for i in details:
        if int(i[0]) > int(deleted_list[0]):
            i[0] -= 1

    table.delete(selected_item) #Deleting row from treeview

    #Will add the new list here later

    table_row_values = table.get_children() #Getting all ID values

    row_placeholder = 0 
    for i in table_row_values: #For loop for updating the table with new list
        table.item(i, values=details[row_placeholder])
        row_placeholder+=1

    row_number -= 1
    
   
    saved = False
    save_log.configure(text = "\n Unsaved \n", fg = 'red')

#Clears table
def clear_table(event):
    
    global row_number, details, saved

    details = []
    row_number = 0 

    table_row_values = table.get_children() #Getting all ID values
    #Deletes the table IDs
    for i in table_row_values:
        table.delete(i)
    

    saved = False
    save_log.configure(text = "\n Unsaved \n", fg = 'red')
   


#Enables the user to edit rows
def edit_row(event):

    global saved

    name = False
    receipt = False
    item = False

    #Some of the following code is reused from the add_to_table() function but some minor changes prevent it from being used as a function

    if number_checker(receipt_box.get()): # The number_checker method which was previously created checks if receipt_box.get() has any numbers
            
        if(int(receipt_box.get()) < 10000000 and int(receipt_box.get()) > 1000000):
            
            if len(details) != 0:
                
                placeholder = True
                
                for i in details:   
                    

                    if placeholder == True:


                        if int(receipt_box.get()) == int(i[2]):

                            receipt_warning.config(text = "This row already has this receipt number")
                            placeholder = False
                            receipt = False

                        else:

                            receipt_warning.config(text = " ")
                            receipt = True

            
            else:

                receipt_warning.config(text = " ")
                receipt = True
        else: 
                receipt_warning.config(text = "The receipt number should only have 7 characters")

               
        
    else:
        
        if len(receipt_box.get()) != 0:
            receipt_warning.config(text = "Only enter numbers")
    

    if number_checker(name_box.get()):
        name_warning.config(text="There can only be letters in a name", fg = 'red')

    else:

        name = True
        name_warning.config(text = " ")

    if number_checker(number_of_items_box.get()):
        
        if(0 <= int(number_of_items_box.get()) <= 500):
            item = True
            number_of_items_warning.config(text = " ")
        else:
            number_of_items_warning.config(text = "The numbers of items can only be from 0 to 500")
    
    else:
        
        if len(number_of_items_box.get()) == 0:
            item = True
            number_of_items_warning.config(text = " ")

        else:
            number_of_items_warning.config(text = "There can only be numbers")

    if length_checker([name_box.get(), items.get(), number_of_items_box.get(), receipt_box.get()]):

        log.config(text="Some of your texts are empty... Add something!", fg = 'red')

    else:
    
        if item == True and name == True and receipt == True:

            selected_item = table.selection()[0] #Gets selected row
            selected_details = table.item(selected_item) #Gets reference to selected row
            row = details[selected_details.get('values')[0]] #Gets details of the selected row.
            print("testing" + str(row[2]))


            details[row[0]] = [row[0], name_box.get(), receipt_box.get(), items.get(), number_of_items_box.get()]
            table.item(selected_item, values=details[row[0]]) #Updates selected row
            
 
            saved = False
            save_log.configure(text = "\n Unsaved \n", fg = 'red')



def help_function(*event): #Brief tutorial for new users
    
    pop_up(title = "Tutorial", #The text below has been formatted this way so I can alter/see it more easily
                            text = "\n Tutorial: \n"
                            "\n - Enter the necessary details (name, item, number of items and receipt number) in the text boxes to your left. \n \n"
                            + " - Warnings will show up if invalid entries or potential mistakes occur. \n \n" 
                            + " - There are buttons below the text boxes. You will be able submit details to the table on your right. \n \n"
                            + " - If you wish to edit or delete rows on table then you must CLICK on the row you want to edit or delete before clicking the button. \n \n"
                            + " - A save button exists to save all the details on your table \n \n"
                            + " - You may adjust text sizes, text colours and background colours \n \n"
                            + " - User input for submitting and removing rows exists if you prefer using the keyboard more. Press enter to submit or press backspace to delete (make sure a row is selected). \n \n"
                            + " - Enjoy!")


        

#A separate window for the user to creatr adjustments
def preferences_window(event):

    #A list of colours for the comboboxes
    colours = ["black", "white", "red", "brown", "orange", "yellow", "green", "blue", "purple", "pink"]

    
    #Submit function
    def submit(event):

        for i in colours: 
            #Checks to see if the user picked valid colours
            if text_colour.get() != i or text_background != i:
                submit_log.config(text = "You haven't picked valid inputs")
            
                


            #Checks for empty text boxes
        if length_checker([text_colour.get(), text_background.get(), text_size.get()]):
            submit_log.config(text = "Some of your texts are empty") 
        
        else: #Updates all labels
            name_label.config(font = (int(text_size.get())), bg = text_background.get(), fg = text_colour.get())
            items_label.config(font = (int(text_size.get())), bg = text_background.get(), fg = text_colour.get())
            number_of_items_label.config(font = (int(text_size.get())), bg = text_background.get(), fg = text_colour.get())
            receipt_label.config(font = (int(text_size.get())), bg = text_background.get(), fg = text_colour.get())
            submit_log.config(text = " ")
    
        



    #New window
    preferences = tk.Tk()
    preferences.title("Preferences")
        
    #Text colour widgets
    text_colour_frame = tk.Frame(preferences)
    tk.Label(text_colour_frame, text = "Text colour").pack(side="left")
    text_colour = ttk.Combobox(text_colour_frame, values = colours)
    text_colour.pack(side="right")
    text_colour_frame.pack()

    #Text background widgets
    text_background_frame = tk.Frame(preferences)
    tk.Label(text_background_frame, text = "Text background").pack(side="left")
    text_background = ttk.Combobox(text_background_frame, values = colours)
    text_background.pack(side="right")
    text_background_frame.pack()

    #Text size widgets
    text_size_frame = tk.Frame(preferences)
    tk.Label(text_size_frame, text = "Text size").pack(side="left")
    text_size = tk.Entry(text_size_frame)
    text_size.pack(side="right")
    text_size_frame.pack()


    
    #Message log incase of misuse
    submit_log = tk.Label(preferences, fg = 'red')
    submit_log.pack(side="bottom")
        
    submit_preferences = tk.Button(preferences, text = "Submit")
    submit_preferences.bind("<ButtonPress>", submit)
    submit_preferences.pack(side="right")

    #User input: Press enter to submit
    preferences.bind("<Return>", submit)

        
        

def close_window(): #Exit button
    global saved
    
    if saved == True:
        root.destroy() #Destroys window
    
    if saved == False:
        pop_up("Warning!", "You have a few unsaved changes")

#All the tkinter labels, entries, treeviews, buttons and more are below
help_function()

root = tk.Tk() #This is the window
root.title("Julie's Party Hire Store Details") #Title of window
root.attributes('-fullscreen', True) #Makes window full screen


#Title
title_image = tk.PhotoImage(file="Title.png") #Gets reference of image
title_image = title_image.zoom(10) #Adjusts size of image
title_image = title_image.subsample(15) #Adjusts size of image


title = tk.Label(root, image=title_image) #Tkinter text label
title.pack() #Adds label to window
title.place(x=0, y=20) #Places label


#Name
name_frame = tk.Frame(root) #Makes widgets slightly more organized

name_warning = tk.Label(name_frame, fg = 'red')
name_warning.pack(side='bottom')

name_label = tk.Label(name_frame, text = "Full Name: ", font=25)
name_label.pack(side="left")

name_box = tk.Entry(name_frame, width=50) #User input can go in here
name_box.pack(side="right")


name_frame.pack(side="left")
name_frame.place(x=30, y=130)


#Number of items
number_items_frame = tk.Frame(root)

number_of_items_warning = tk.Label(number_items_frame, fg = 'red')
number_of_items_warning.pack(side="bottom")

number_of_items_label = tk.Label(number_items_frame, text = "Number Of Items: ", font=25)
number_of_items_label.pack(side="left")


number_of_items_box = tk.Entry(number_items_frame, width=50)
number_of_items_box.pack(side="right")


number_items_frame.pack(side="left")
number_items_frame.place(x=30, y=230)


#Items
items_frame = tk.Frame(root)

items_label = tk.Label(items_frame, text = "Item: ", font = 50)
items_label.pack(side="left")

items = ttk.Combobox(items_frame, values = ["Party Hire Stuff", "Other stuff"]) #Multiselect widget                             
items.pack(side="right")

items_frame.pack(side="left")
items_frame.place(x=30, y=330)


#Receipt
receipt_frame = tk.Frame(root)

receipt_warning = tk.Label(receipt_frame, fg = 'red')
receipt_warning.pack(side="bottom")

receipt_label = tk.Label(receipt_frame, text = "Receipt number: ", font = 25)
receipt_label.pack(side="left")


receipt_box = tk.Entry(receipt_frame, width=30)
receipt_box.pack(side="right")


receipt_frame.pack(side="left")
receipt_frame.place(x=30, y=430)

#Buttons

#Submit button
submit = tk.Button(root, text = "Submit", font = 50) #Creates button
submit.bind("<ButtonPress>", add_to_table) #Binds function to a button. When the button is pressed, the function will play
submit.pack()
submit.place(x=30, y=525)

#Remove button
remove_button = tk.Button(root, text = "Remove", font = 50)
remove_button.bind('<ButtonPress>', remove_row)
remove_button.pack()
remove_button.place(x=130,y=525)

#Save button
save = tk.Button(root, text = "Save", font = 50)
save.bind("<ButtonPress>", save_details)
save.pack()
save.place(x = 30, y = 590)

#Edit button
edit = tk.Button(root, text = "Edit", font = 50)
edit.bind("<ButtonPress>", edit_row)
edit.pack()
edit.place(x=240, y= 525)

#Clear button
clear = tk.Button(root, text = "Clear", font = 50)
clear.bind("<ButtonPress>", clear_table)
clear.pack()
clear.place(x = 310, y = 525)

#Frame for the table

frame_table = tk.Frame(root)

#Frame for important buttons/widgets

important = tk.Frame(frame_table)

#A widget for logging the state of save
save_log = tk.Label (important, text = "\n Saved \n")
save_log.pack(side='bottom')

#Help button
help_button = tk.Button(important, text = "Help", font = ("Arial", 10, "bold"))
help_button.bind("<ButtonPress>", help_function)
help_button.pack(side='left')

#Exit Button
exit_button = tk.Button(important, text = "Exit", font = ("Arial", 10, "bold"), command = close_window)
exit_button.pack(side='right')

#Preferences button
preferences_button = tk.Button(important, text = "Preferences", font = ("Arial", 10, "bold"))
preferences_button.bind("<ButtonPress>", preferences_window)
preferences_button.pack()


important.pack(side="top")

#Tkinter scrollbar
table_scrollbar = ttk.Scrollbar(frame_table)
table_scrollbar.pack(side="right", fill = 'y')

#Tkinter table/treeview
table = ttk.Treeview(frame_table, columns = ["ID", "Name","Receipt Number", "Item", "Number Of Items"], height=20, yscrollcommand = table_scrollbar.set)
table.column('#0', width=0, stretch=False) #Gets rid of invisible column (shrinks it to an unnoticable level)
table.column('#01', width=50) #Sets width of the rows
table.column('#02', width=150)
table.column('#03', width=150)
table.column('#04', width=150)
table.column('#05', width=150)
table.heading('ID', text = "ID")
table.heading('Name', text = "Full Name") #Adds headings
table.heading('Receipt Number', text = "Receipt Number")
table.heading('Item', text = "Item")
table.heading('Number Of Items', text = "Number Of Items")



table.pack()


table_scrollbar.config(command = table.yview) #Adds scrollbar to the side of the treeview

#Logs errors
log = tk.Label(frame_table)
log.pack(side='bottom')

frame_table.pack()
frame_table.place(x=600,y=100)


load_details()#Loads all details

#User input

root.bind('<Return>', add_to_table) #Press enter to submit details
root.bind('<BackSpace>', remove_row) #Press backspace to remove rows (make sure a row is selected)


root.mainloop() #Runs window

