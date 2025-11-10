
**NAMES : Turikumana jean Claude**

**ST ID :26989**

**on 10/11/2025**

### **define a problem that demonstrates PL/SQL Collections, Records, and GOTO statements.**






### **The problem is to create a PL/SQL program that can store and manage many related data items together. The program should be able to keep several values in one group, store different types of information about one item, and control the flow of the program using jumps when needed.**

This problem shows how **PL/SQL** can manage complex data and control the program’s flow effectively.

* **Collections** represent the ability to store and handle many pieces of similar data together. This means the program can process large amounts of information in an organized and flexible way, instead of working with one value at a time.

* **Records** show how different types of information that belong to one item can be grouped together. This helps in keeping data complete, connected, and easy to understand — like keeping all details about one thing in one place.

* **GOTO statements** demonstrate control and direction in a program. They help the program jump to a specific part when something unusual happens, such as an error, making the process more controlled and reliable.


-- Turn on output before running:

-- SET SERVEROUTPUT ON;

DECLARE

    --------------------------------------------------------------------
    -- 1️⃣ COLLECTION: Used to store many product prices for one customer
    --------------------------------------------------------------------
    
    TYPE Product_List IS TABLE OF NUMBER;  -- A nested table (collection)
    v_prices Product_List := Product_List(12000, 18000, 20000, 5000);

    --------------------------------------------------------------------
    -- 2️⃣ RECORD: Used to keep all customer information in one variable
    --------------------------------------------------------------------
    
    TYPE Customer_Record IS RECORD (
        customer_id    NUMBER,
        customer_name  VARCHAR2(30),
        total_amount   NUMBER,
        discount       NUMBER,
        final_amount   NUMBER
    );
    v_customer Customer_Record;  -- variable of record type

    --------------------------------------------------------------------
    -- 3️⃣ CONTROL VARIABLES
    --------------------------------------------------------------------
    
    v_sum NUMBER := 0;
    v_discount_rate CONSTANT NUMBER := 0.10; -- 10% discount
    v_limit CONSTANT NUMBER := 50000;        -- discount limit

BEGIN

    --------------------------------------------------------------------
    -- Initialize customer data
    --------------------------------------------------------------------
    
    v_customer.customer_id := 101;
    v_customer.customer_name := 'Alice';
    v_customer.total_amount := 0;
    v_customer.discount := 0;
    v_customer.final_amount := 0;
    
    --------------------------------------------------------------------
    -- Check if collection has data; if not, jump to error handling
    --------------------------------------------------------------------
    
    IF v_prices.COUNT = 0 THEN
        DBMS_OUTPUT.PUT_LINE('No product prices found!');
        GOTO handle_error;
    END IF;
    
    --------------------------------------------------------------------
    -- Sum all product prices
    --------------------------------------------------------------------
    
    FOR i IN 1 .. v_prices.COUNT LOOP
        IF v_prices(i) <= 0 THEN
            DBMS_OUTPUT.PUT_LINE('Invalid price found: ' || v_prices(i));
            GOTO handle_error;  -- jump to error-handling section
        END IF;
        v_sum := v_sum + v_prices(i);
    END LOOP;

    v_customer.total_amount := v_sum;
    
    --------------------------------------------------------------------
    -- Apply discount if total ≥ 50,000
    --------------------------------------------------------------------  
    
    IF v_customer.total_amount >= v_limit THEN
        v_customer.discount := v_customer.total_amount * v_discount_rate;
    ELSE
        v_customer.discount := 0;
    END IF;
    
    --------------------------------------------------------------------
    -- Calculate final amount
    --------------------------------------------------------------------
    
    v_customer.final_amount := v_customer.total_amount - v_customer.discount;
    
    --------------------------------------------------------------------
    -- Display customer details
    --------------------------------------------------------------------
    
    DBMS_OUTPUT.PUT_LINE('--- CUSTOMER ORDER DETAILS ---');
    DBMS_OUTPUT.PUT_LINE('Customer ID: ' || v_customer.customer_id);
    DBMS_OUTPUT.PUT_LINE('Customer Name: ' || v_customer.customer_name);
    DBMS_OUTPUT.PUT_LINE('Total Amount: ' || v_customer.total_amount);
    DBMS_OUTPUT.PUT_LINE('Discount: ' || v_customer.discount);
    DBMS_OUTPUT.PUT_LINE('Final Amount: ' || v_customer.final_amount);

    GOTO finish;  -- jump to end section

    --------------------------------------------------------------------
    -- 🧩 GOTO ERROR HANDLING SECTION
    --------------------------------------------------------------------
    
    <<handle_error>>
    DBMS_OUTPUT.PUT_LINE('Error found! Please check your data again.');
    GOTO finish;

    --------------------------------------------------------------------
    -- 🏁 FINISH SECTION
    --------------------------------------------------------------------
    
    <<finish>>
    DBMS_OUTPUT.PUT_LINE('--- Program Completed ---');

END;
/





