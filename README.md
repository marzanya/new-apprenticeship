# new-apprenticeship

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# read amazon excel file into python

obj = pd.read_csv('C:/Users/daesk/OneDrive/Documents/amazon.csv')


# This will create loop for generating recommendation of books based on the budget.
def asking():
    budget = int(input("How much money do you want to spend on? :"))
    budget_book = obj.loc[obj['Price'] == budget]
    s = 0
    for i in budget_book['Price']:
        print("We recommend the book called", '"' + budget_book.iloc[s:s+1, 0].to_string(index=False) + '"' + ".", "It it wrote by", budget_book.iloc[s:s+1, 1].to_string(index=False) + ".",
        "\nBook rating is", budget_book.iloc[s:s+1, 2].to_string(index=False), "and published in", budget_book.iloc[s:s+1, 5].to_string(index=False)+".")
        s = s + 1
    ask = input("Would you like to try again?. Press Y to try again or any other ket to exit the program")
    if ask == 'Y':
        asking()
    else:
        print("Thank you, have a nice day!")

asking()


# check if there are any missing values in our data
print(obj.isnull().sum())
# assume only customer who actually bought the book can leave review and we use that number to
# generate how much acutally book made. i added new colum totla to reflect that price
obj["Total"] = obj["Reviews"] * obj["Price"]
print(obj.sort_values(by='User Rating', ascending=False).head(5).to_string())


s = obj.groupby(["Price", "Reviews", "Name"]).sum()
print(s.to_string())

