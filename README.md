## EX3 Implementation of GSP Algorithm In Python
### NAME: VIGNESH V
### REG NO: 212223110062
### DATE: 21.11.2026
### AIM: 
To implement GSP Algorithm In Python.
### Description:
The Generalized Sequential Pattern (GSP) algorithm is a data mining technique used for discovering frequent patterns within a sequence database. It operates by identifying sequences that frequently occur together. GSP works by employing a depth-first search strategy to explore and extract frequent patterns efficiently.
### Steps:
1. <strong>Database Scanning:</strong> GSP scans the sequence database to determine the support of each item in the dataset.
2. <strong>Candidate Generation:</strong> It generates a set of candidate sequences using frequent items found in the previous step.
3. <strong>Pattern Growth:</strong> It extends the candidate sequences by merging them to form longer patterns, checking their support against a user-defined minimum support threshold.
4. <strong>Repeat:</strong> The process continues until no new sequences meet the minimum support threshold.
<p align="justify">
GSP finds application in various domains such as market basket analysis, web usage mining, bioinformatics, and more. For instance, in retail, GSP can identify common purchasing patterns, helping businesses understand customer behavior for targeted marketing or inventory management.
</p>

### Procedure:
<p align="justify">
1. From collections import defaultdict, from itertools import combinations: Imports necessary libraries/modules. defaultdict is
used to create a dictionary with default values and combinations generates all possible combinations of a sequence.</p>
<p align="justify">
2. generate_candidates(dataset, k): Function to generate candidate k-item sequences from a dataset. It loops through each sequence in the
dataset and finds combinations of length k for each sequence, updating their counts in a dictionary.</p>
<p align="justify">
3. gsp(dataset, min_support): Function that implements the Generalized Sequential Pattern (GSP) algorithm. It iterates through increasing
sequence lengths (k) until no new frequent patterns are found. It calls generate_candidates() to find patterns of varying lengths.</p>
<p align="justify">
4. Example dataset for each category: Defines example sequences for top wear, bottom wear, and party wear categories.</p>
<p align="justify">
5. Minimum support threshold: Sets the minimum support count required for a pattern to be considered frequent.</p>
<p align="justify">
6. Perform GSP algorithm for each category: Applies the GSP algorithm for each category using the defined example datasets and the
minimum support threshold.</p>
<p align="justify">
7. Output the frequent sequential patterns for each category: Prints the frequent sequential patterns 
    along with their support counts
for each wear category.</p>
<p align="justify">
8. Visulaize the sequence patterns using matplotlib.
</p>
### Program:

```python
from collections import defaultdict
from itertools import combinations
# Function to generate candidate k-item sequences
def generate_candidates(dataset, k):


    /WRITE YOUR CODE HERE/


#Function to perform GSP algorithm
def gsp(dataset, min_support):


  /WRITE YOUR CODE HERE/


#Example dataset for each category
top_wear_data = [
 ["blouse", "t-shirt", "tank_top"],
 ["hoodie", "sweater", "top"],["hoodie"],["hoodie","sweater"]
 #Add more sequences for top wear
]
bottom_wear_data = [
 ["jeans", "trousers", "shorts"],
 ["leggings", "skirt", "chinos"],
 # Add more sequences for bottom wear
]
party_wear_data = [
 ["cocktail_dress", "evening_gown", "blazer"],
 ["party_dress", "formal_dress", "suit"],
 ["party_dress", "formal_dress", "suit"],
 ["party_dress", "formal_dress", "suit"],
 ["party_dress", "formal_dress", "suit"],
 ["party_dress"],["party_dress"],
 # Add more sequences for party wear
]
#Minimum support threshold
min_support = 2
#Perform GSP algorithm for each category
top_wear_result = gsp(top_wear_data, min_support)
bottom_wear_result = gsp(bottom_wear_data, min_support)
party_wear_result = gsp(party_wear_data, min_support)
#Output the frequent sequential patterns for each category
print("Frequent Sequential Patterns - Top Wear:")
if top_wear_result:
 for pattern, support in top_wear_result.items():
 print(f"Pattern: {pattern}, Support: {support}")
else:
 print("No frequent sequential patterns found in Top Wear.")
print("\nFrequent Sequential Patterns - Bottom Wear:")
if bottom_wear_result:
 for pattern, support in bottom_wear_result.items():
 print(f"Pattern: {pattern}, Support: {support}")
else:
 print("No frequent sequential patterns found in Bottom Wear.")
print("\nFrequent Sequential Patterns - Party Wear:")
if party_wear_result:
 for pattern, support in party_wear_result.items():
 print(f"Pattern: {pattern}, Support: {support}")
else:
 print("No frequent sequential patterns found in Party Wear.")
```
### Complete CODE:
```
# -*- coding: utf-8 -*-
"""
Spyder Editor

This is a temporary script file.
"""


from collections import defaultdict
import matplotlib.pyplot as plt


# PART 1: Check whether candidate is a subsequence
def is_subsequence(candidate, sequence):
    i = 0

    for item in sequence:
        if i < len(candidate) and candidate[i] == item:
            i += 1

    return i == len(candidate)


# Generate frequent 1-sequences
def generate_L1(database, min_support):
    counts = defaultdict(int)

    for seq in database:
        for item in set(seq):
            counts[(item,)] += 1

    return {k: v for k, v in counts.items()
            if v >= min_support}


# PART 2: Generate candidate sequences
def generate_candidates(prev_patterns):
    candidates = set()
    patterns = list(prev_patterns.keys())

    for p1 in patterns:
        for p2 in patterns:

            # Join two patterns
            if p1[1:] == p2[:-1]:
                candidate = p1 + (p2[-1],)

                candidates.add(candidate)

    return candidates


# Count support of candidates
def count_support(database, candidates, min_support):
    support = defaultdict(int)

    for c in candidates:
        for seq in database:
            if is_subsequence(c, seq):
                support[c] += 1

    return {k: v for k, v in support.items()
            if v >= min_support}


# PART 3: Complete GSP algorithm
def gsp(database, min_support):

    result = {}

    # Generate frequent 1-sequences
    current_patterns = generate_L1(database, min_support)

    result.update(current_patterns)

    # Generate longer patterns
    while current_patterns:

        candidates = generate_candidates(current_patterns)

        if not candidates:
            break

        current_patterns = count_support(
            database, candidates, min_support
        )

        result.update(current_patterns)

    return result


# Example dataset for each category

top_wear_data = [
    ["blouse", "t-shirt", "tank_top"],
    ["hoodie", "sweater", "top"],
    ["hoodie"],
    ["hoodie", "sweater"]
]


bottom_wear_data = [
    ["jeans", "trousers", "shorts"],
    ["jeans", "trousers", "shorts"],
    ["leggings", "skirt", "chinos"]
]


party_wear_data = [
    ["cocktail_dress", "evening_gown", "blazer"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress"],
    ["party_dress"]
]


# Minimum support threshold
min_support = 2


# Perform GSP algorithm
top_wear_result = gsp(top_wear_data, min_support)

bottom_wear_result = gsp(bottom_wear_data, min_support)

party_wear_result = gsp(party_wear_data, min_support)


# Display frequent sequential patterns

print("Frequent Sequential Patterns - Top Wear:")

if top_wear_result:
    for pattern, support in sorted(top_wear_result.items()):
        print(f"Pattern: {pattern}, Support: {support}")
else:
    print("No frequent sequential patterns found in Top Wear.")


print("\nFrequent Sequential Patterns - Bottom Wear:")

if bottom_wear_result:
    for pattern, support in sorted(bottom_wear_result.items()):
        print(f"Pattern: {pattern}, Support: {support}")
else:
    print("No frequent sequential patterns found in Bottom Wear.")


print("\nFrequent Sequential Patterns - Party Wear:")

if party_wear_result:
    for pattern, support in sorted(party_wear_result.items()):
        print(f"Pattern: {pattern}, Support: {support}")
else:
    print("No frequent sequential patterns found in Party Wear.")


# PART 4: Visualization with support counts

def visualize_patterns_line(result, category):

    if result:
        patterns = list(result.keys())
        support = list(result.values())

        plt.figure(figsize=(10, 6))

        plt.plot(
            [str(pattern) for pattern in patterns],
            support,
            marker='o',
            linestyle='-',
            color='blue'
        )

        # Display support count on each point
        for i, value in enumerate(support):
            plt.text(i, value, str(value),
                     ha='center', va='bottom')

        plt.xlabel("Patterns")
        plt.ylabel("Support Count")
        plt.title(f"Frequent Sequential Patterns - {category}")

        plt.xticks(rotation=90)
        plt.tight_layout()
        plt.show()

    else:
        print(f"No frequent sequential patterns found in {category}.")


# Visualize results for each category

visualize_patterns_line(top_wear_result, "Top Wear")

visualize_patterns_line(bottom_wear_result, "Bottom Wear")

visualize_patterns_line(party_wear_result, "Party Wear")

```
### Output:
![alt text](image.png)

### Visualization:
```python
import matplotlib.pyplot as plt

# Function to visualize frequent sequential patterns with a line plot
def visualize_patterns_line(result, category):
    if result:
        patterns = list(result.keys())
        support = list(result.values())

        plt.figure(figsize=(10, 6))
        plt.plot([str(pattern) for pattern in patterns], support, marker='o', linestyle='-', color='blue')
        plt.xlabel('Patterns')
        plt.ylabel('Support Count')
        plt.title(f'Frequent Sequential Patterns - {category}')
        plt.xticks(rotation=90)
        plt.tight_layout()
        plt.show()
    else:
        print(f"No frequent sequential patterns found in {category}.")

# Visualize frequent sequential patterns for each category using a line plot
visualize_patterns_line(top_wear_result, 'Top Wear')
visualize_patterns_line(bottom_wear_result, 'Bottom Wear')
visualize_patterns_line(party_wear_result, 'Party Wear')
```
### Output:
![alt text](image-1.png)
![alt text](image-2.png)
![alt text](image-3.png)
![alt text](image-4.png)
![alt text](image-5.png)

### Result:
Thus, the GSP Algorithm In Python was successfully implemented.
