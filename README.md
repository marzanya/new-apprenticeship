import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# read amazon excel file into python
obj = pd.read_csv('C:/Users/daesk/OneDrive/Documents/amazon.csv')

# check if there are any missing values in our data
print(obj.isnull().sum())

# assume only customer who actually bought the book can leave review and we use that number to generate how many books actually solde. I added new colum "Total" to reflect that.
obj["Total"] = obj["Reviews"] * obj["Price"]

# This will create loop for generating recommendation of books based on the budget and user rating

def asking():
    budget = (input("How much money do you want to spend on? :"))
    try:
        budget = int(budget)
    except ValueError:
        print("Please, Use Only Numbers!")
    budget_book = obj.loc[obj['Price'] == budget].sort_values(by="User Rating", ascending=False)
    s = 0
    for i in budget_book["User Rating"].sort_values(ascending=False).head(5):
        print("We recommend the book called", '"' + budget_book.iloc[s:s+1, 0].to_string(index=False) + '"' + ".", "It it wrote by", budget_book.iloc[s:s+1, 1].to_string(index=False) + ".",
        "\nBook rating is", budget_book.iloc[s:s+1, 2].to_string(index=False), "and published in", budget_book.iloc[s:s+1, 5].to_string(index=False)+".")
        s = s + 1
    ask = input("Would you like to try again?. Press Y to try again or any other ket to exit the program\n")
    if ask.upper() == 'Y':
        asking()
    else:
        print("Thank you, have a nice day!")

start = input("Hi, Would you like to try our book search that will create a list of recommended book based on your budge and also, user ratings.\nPress y or Y to start the program or any other key to exit. Thank you\n")
if start.upper() == 'Y':
    asking()
else:
    print("Thank you, have a nice day")


# This will generate books in catagories based on rating and also, how many non-fiction or fiction book exists
def bookreview(x):
    if x >= 4.5:
        return 'Books that has rating equal or more than 4.5'
    elif x >= 4:
        return 'Books that has rating equal or more than 4'
    elif x >= 3.5:
        return 'Books that has rating equal or more than 3.5'
    elif x >= 3:
        return 'Books that has rating equal or more than 3'
    else:
        return 'Books that has rating less than 3'
def genre(x):
    if x == 'Non Fiction':
        return 'Non-Fiction'
    else:
        return  'Fiction'

print(obj['User Rating'].apply(bookreview).value_counts().to_string())
print(obj['Genre'].apply(genre).value_counts().to_string())

#graph
xs = obj["Year"].tolist()
ys =obj["Total"].tolist()
plt.xlabel("Year")
plt.title("Book Sales")
plt.ylabel("Total Sale")
plt.bar(xs, ys, width = 0.7, color='green')
plt.show()
