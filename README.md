# Individual Assignment III :PL/SQL Records, Collections and GOTO Statements Demonstration
**Name** : Firmin Iradukunda 
**ID** : 27316 
**Course** : Database Development with PL/SQL (INSY 8311) 
**Instructor** : Mr Eric Maniraguha

### Assignment Objective : 
Demonstrate understanding of PL/SQL Records, Collections and GOTO Statements by solving a realistic problem, implementing them , and documenting results in a professional GitHub repository.
## 1. Problem statement
A bookshop wants to keep track of its books and quickly identify which titles are out of stock. Each book has an ID, Title, and Number of Copies. The system should:

**Store** multiple books in memory using **records and collections.**

Loop through the collection to display each book’s details.
If a book has zero copies, the program should immediately jump to a special handler using a  **GOTO statement to flag it as “OUT OF STOCK.”**

This problem is small, clear, and demonstrates exactly how PL/SQL records, collections, and GOTO can work together in a practical scenario.

## 2. Demonstartion
### 1. Define a **Record Type**
```sql
TYPE book_rec IS  RECORD ( 
book_id NUMBER, 
title VARCHAR2(100), 
copies NUMBER 
);
```
    
** Here, each book has ** :
    
* `book_id` → unique identifier
        
* `title` → book name
        
* `copies` → how many copies are available

### 2. Define a **Collection Type**
```sql  
TYPE book_table IS  TABLE  OF  book_rec; 
books book_table :=  book_table();
```
* `book_table` is a table (nested collection) of `book_rec`.
    
* `books` is the variable we’ll use to store all our book records.

### 3. Add Sample Data
```sql  
books.EXTEND(3);

books(1).book_id := 1; books(1).title := 'Database Systems'; books(1).copies := 5;
books(2).book_id := 2; books(2).title := 'PL/SQL Basics'; books(2).copies := 0;
books(3).book_id := 3; books(3).title := 'Algorithms'; books(3).copies := 2;

```
* `EXTEND(3)` → makes room for 3 records in the collection.
    
* We then assign values to each book’s fields.
    
* Notice book 2 has `copies = 0` (this will trigger our GOTO).

### 4. Process the Collection

```sql  
FOR  i IN  1..books.COUNT LOOP IF books(i).copies =  0  THEN  
GOTO OUT_OF_STOCK; 
END  IF; 

DBMS_OUTPUT.PUT_LINE('Book: '  ||  books(i).title ||  ' | Copies: '  ||  books(i).copies); CONTINUE; 

<<OUT_OF_STOCK>>  
DBMS_OUTPUT.PUT_LINE(' ALERT: '  ||  books(i).title ||  ' is OUT OF STOCK!'); 
END  LOOP;
```
* `FOR i IN 1..books.COUNT LOOP` → loop through each book in the collection.
* `IF books(i).copies = 0 THEN GOTO OUT_OF_STOCK;`
If a book has no copies, jump to the label `OUT_OF_STOCK`.
* `DBMS_OUTPUT.PUT_LINE(...)` → prints book info if copies are available.
    
* `<<OUT_OF_STOCK>>` → label where the program jumps if the book is out of stock.
    
* `CONTINUE` → ensures the loop moves to the next book after printing.

## 3. Implementation

![Demo Screenshot](https://github.com/iradukundafirmin/plsql_records_collect_goto_demo_iradukunda_firmin/blob/main/Records_collection%20screenshots.JPG?raw=true)
### Output
![Demo Screenshot](https://github.com/iradukundafirmin/plsql_records_collect_goto_demo_iradukunda_firmin/blob/main/Records_collection%20screenshots%202.JPG?raw=true])

## 4. Conclusion

### Key Concepts Demonstrated
*  **Record**: Groups book details together.
    
* **Collection**: Holds multiple records (like a mini in-memory table).
    
*  **GOTO**: Provides explicit control flow to handle special cases (out of stock).
