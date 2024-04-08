INTRODUCTION
This case study is about ABC SDN BHD (ABC) which is a stockist that has been employed by SHARIFAH FOOD SDN BHD to manage and recruit sales agents and dropships. The number of agents and dropships have increased along with the growth of the business. As a result, ABC found difficulty in managing all the stockist processes. Thus, ABC requires a system built to keep track of the product inventory, purchases, orders as well as commission payment for agent and dropship. We have analysed the situation in this case and discovered user requirements for the system. Then, we categorize the user requirements according to the entity needed in the system. We have also made several assumptions so that the system can work flawlessly. 
CUSTOMER
1.	All customers have information consisting of attributes cus_id, cus_name, cus_email, cus_address, cus_phone_num and cus_acc_num.
2.	A customer can be a registered customer.
3.	A registered_customer is either a dropship or agent.
4.	A registered_customer has information consisting of attributes rc_id, rc_fb_name, rc_ic and rc_type.
5.	One customer can buy one or more products.
6.	One customer can possess zero or many postage.
7.	One customer can hold zero or many delivery_location.
8.	One customer can pay one to many sales.
9.	The primary key of customer is cus_id.
COMMISSION
1.	Commission consists of attributes which include com_rate and com_type.
2.	One commission will be included in one or more sales.
3.	One commission will be gained by zero to many registered customers which is either an agent or dropship. 
4.	One commission will be taken by zero to many marketing staff.
5.	The primary key of commission is the combination of com_rate and com_type.
SALES
1.	Sales consists of attributes such as invoice_num, sales_ date, payment_type and quantity_sold.
2.	One sale can give one to two commissions.
3.	One sale will be made by one marketing staff.
4.	One sale has one to many products.
5.	One sale consists of zero to one postage.
6.	One sale provides zero to one delivery location.
7.	One sale will be paid by one customer.
8.	The primary key of sale is invoice_num.
PRODUCT
1.	Product consists of attributes which include product_id, weight and price.
2.	One product can be bought by zero to many customers.
3.	One product can be sold by zero to many marketing staff.
4.	One product can be increased in quantity by one to many stock intakes.
5.	One product can be included in zero to many sales.
6.	The product will be categorized into a Normal_product or Promo_package.
7.	Normal_product contains an attribute which is product_name.
8.	Promo_package contains an attribute which is pack_name.
9.	Each normal product can be found in zero to four promotion packages.
10.	Each promotion package includes six to ten products.
11.	The primary key of product inventory is product_id.
MARKETING STAFF
1.	The marketing staff entity contains attributes of staff_id, staff_name, staff_ic, staff_email, staff_phone_num and staff_fb_name.
2.	One marketing staff can sell zero to many products.
3.	One marketing staff can make zero to many sales.
4.	One marketing staff can take zero to many commissions.
5.	The primary key of marketing_staff entity is staff_id.
DELIVERY_LOCATION
1.	The delivery location contains attributes of loca_id, loca_name and charge.
2.	One delivery location can be in zero to many sales.
3.	One delivery location can be held by zero to many customers.
4.	The primary key of delivery location is loca_id.
POSTAGE
1.	The postage contains attributes of region_id , region, weight, SST and basic fee.
2.	One postage can be provided in zero to many sales.
3.	One postage can be possessed by zero to many customers. 
4.	The primary key of the postage is the region_id.
STOCK_INTAKE
1.	Stock intake consists of attributes which are purchase_order_id, quantity_bought and buy_date.
2.	One stock_intake can increase the quantity of one to many products.
3.	The primary key of the stock intake is purchase_order_id.


ASSUMPTIONS
1.	All products for sales are in Malaysia only.
2.	Customers have to use Malaysia phone numbers to make an order.
3.	Delivery services are only provided in the Klang Valley area only.
4.	Postage services are not provided outside Malaysia.
5.	The postage and delivery for the order received from the client by agent will be done by the agent himself. 
6.	ABC Sdn Bhd will not bill for shipping or delivery if the customer comes and purchases the item from the store. 
7.	ABC Sdn Bhd will be alerted by the system when the agent’s restocking is less than RM 2000 for continuously 3 months. 
8.	The system will operate in Windows OS, Android, IOS and others.
9.	Customers can only order using Whatsapp or Facebook through marketing staff.
10.	There are only four promotional packages during every holiday and festival season.
11.	Marketing staff will receive the commissions at the end of the month.
12.	Every commission will be given to agent and dropship in the form of a discount on the sales.
13.	For the case that marketing staff will sell the products to the agent or dropship, the full price of the sales will be used as the price to deduct the commission of both marketing staff and agent or dropship.
14.	Sales dates will be equal to the payment dates.
15.	Every sale will be made with one marketing staff.
16.	Marketing staff, agent and dropship may fail to make even one sale.
17.	The rate of commission of promotional package will be different from the rate of commission of normal product for marketing staff, agent and dropship

ENTITY RELATIONSHIP MODELLING (ERD) FOR SHARIFAH READY TO EAT SALES SYSTEM
![image](https://github.com/yeejing0822/foodDropshipManagementDB/assets/86753374/94f185dc-be3f-4dda-9eca-9aed10c1bdd2)

1.	Query 1
   
To check the total sales price of a sale made by the marketing staff and the total price that is deducted with the commissions of the marketing staff as well as the agent or dropship as a customer. This is also able to help to check whether the performance of particular marketing staff is good or not.

COLUMN staff_name HEADING 'STAFF NAME'
COLUMN LPAD(sd.invoice_num,7) HEADING 'INVOICE|NUMBER' 
COLUMN LPAD('RM'||TO_CHAR(SUM(sales_price*quantity_sold),'9999.99'),11) HEADING 'TOTAL SALES|PRICE' 
COLUMN LPAD('RM'||TO_CHAR(SUM(quantity_sold*commission_rate*sales_price),'9999.99'),11) HEADING 'DISCOUNTED|PRICE'
COLUMN SALES_GAIN HEADING 'TOTAL SALES|GAIN'
SELECT staff_name , LPAD(sd.invoice_num,7), LPAD('RM'||TO_CHAR(SUM(sales_price*quantity_sold),'9999.99'),11), 
LPAD('RM'||TO_CHAR(SUM(quantity_sold*commission_rate*sales_price),'9999.99'),11), 
LPAD('RM'||TO_CHAR(SUM(sales_price*quantity_sold-quantity_sold*commission_rate*sales_price),'9999.99'),11) AS SALES_GAIN
FROM SALES S, SALES_DETAIL SD, MARKETING_STAFF MS
WHERE  ms.staff_id = s.staff_id 
AND s.invoice_num = sd.invoice_num
GROUP BY sd.invoice_num, staff_name
ORDER BY staff_name; 

![image](https://github.com/yeejing0822/foodDropshipManagementDB/assets/86753374/b4bcb950-cdae-4926-8dee-fca87fa241c5)


2.	Query 2
   
To check the amount of money that has been consumed each month to restock inventory in that month. To provide information for the financial department to do budgeting.

COLUMN TO_CHAR(buy_date,'MM-YYYY') HEADING 'BUY STOCK DATE|(MONTH)' FORMAT A15
COLUMN LPAD('RM'||TO_CHAR(SUM(intake_price*quantity_bought),'9990.99'),10) HEADING 'TOTAL COST|(MONTH)'
SELECT TO_CHAR(buy_date,'MM-YYYY'), LPAD('RM'||TO_CHAR(SUM(intake_price*quantity_bought),'9990.99'),10)
FROM INTAKE_DETAIL ID,STOCK_INTAKE SI
WHERE id.purchase_order_id = si.purchase_order_id
GROUP BY TO_CHAR(buy_date,'MM-YYYY')
ORDER BY TO_CHAR(buy_date,'MM-YYYY');

![image](https://github.com/yeejing0822/foodDropshipManagementDB/assets/86753374/c69f159e-d556-4137-bf88-8cdcc4795b63)

3.	Query 3
   
To show a list of registered customers and their respective total purchases of a particular month. This can determine if the agents have fulfilled the requirement of restocking items RM2000 per month or not.

COLUMN cus_name HEADING 'CUSTOMER NAME'
COLUMN TO_CHAR(sales_date,'MM-YYYY') HEADING 'SALES DATE|PER MONTH' FORMAT A10
COLUMN LPAD('RM'||TO_CHAR(SUM(sales_price*quantity_sold),'99990.99'),15) HEADING 'TOTAL PURCHASE|PER MONTH' 
SELECT cus_name, TO_CHAR(sales_date,'MM-YYYY'),LPAD('RM'||TO_CHAR(SUM(sales_price*quantity_sold),'99990.99'),15)
FROM CUSTOMER C,REGISTERED_CUSTOMER RC, PHONE_NUMBER PN,SALES S, SALES_DETAIL SD
WHERE c.cus_id = rc.cus_id
AND c.cus_id = pn.cus_id 
AND s.phone_id = pn.phone_id
AND s.invoice_num = sd.invoice_num
AND rc_type = 'A'
GROUP BY cus_name, TO_CHAR(sales_date,'MM-YYYY')
ORDER BY cus_name;

![image](https://github.com/yeejing0822/foodDropshipManagementDB/assets/86753374/24809cdc-7cae-400a-8b7d-b9c94ff0c25d)

4.	Query 4
   
To determine the quantity of current stock available for each product. Total quantity sold shows the total amount of each item sold since the beginning of the business. This can check which product excluding promotional packages is the best-selling product since the beginning of the business.
   
COLUMN PRODUCT_NAME HEADING 'PRODUCT NAME'
COLUMN (Q.QUANTITY-A.TOTAL) HEADING 'CURRENT STOCK|QUANTITY'
COLUMN TOTAL HEADING 'TOTAL QUANTITY|SOLD'
COLUMN QUANTITY HEADING 'TOTAL QUANTITY|BOUGHT'
SELECT A.PRODUCT_NAME, (Q.QUANTITY-A.TOTAL), A.TOTAL, Q.QUANTITY FROM
(
	SELECT PRODUCT_NAME, SUM(sumcol) as Total
	FROM	
	(
		SELECT PRODUCT_NAME, SUM(QUANTITY_SOLD) as sumcol
		FROM NORMAL_PRODUCT, SALES_DETAIL
 		WHERE PRODUCT_ID = ITEM_ID
		GROUP BY PRODUCT_NAME
 		union
 		SELECT product_name, sum(product_quantity*quantity_sold) as sumcol
 		from (pack_detail inner join normal_product on pack_detail.product_id = normal_product.product_id) inner join 
		(sales_detail inner join promo_package on promo_package.pack_id = item_id) on pack_detail.pack_id = promo_package.pack_id
		GROUP BY PRODUCT_NAME
	)
	GROUP BY PRODUCT_NAME
) A,
(
	SELECT PRODUCT_NAME, SUM(QUANTITY_BOUGHT) AS QUANTITY
	FROM INTAKE_DETAIL, NORMAL_PRODUCT
	WHERE NORMAL_PRODUCT.PRODUCT_ID = INTAKE_DETAIL.PRODUCT_ID
	GROUP BY PRODUCT_NAME	
) Q
WHERE A.PRODUCT_NAME = Q.PRODUCT_NAME;

![image](https://github.com/yeejing0822/foodDropshipManagementDB/assets/86753374/1d3d91e1-cf86-4036-ac54-b48c5aeb5a0b)

5.	Query 5
   
This query can help to check the performance growth of sales of the company in sales and the performance growth of sales gained of the company in percentage every month. It is compared with the sales of the previous month.

COLUMN LPAD(a.SALES_DATE,10) HEADING 'SALES DATE|IN MONTH'
COLUMN LPAD(a.TOTAL_SALES_MADE,11) HEADING 'TOTAL SALES|MADE'
COLUMN LPAD(NVL(a.NUM_SALES,0),11) HEADING 'PERFORMANCE|GROWTH|(SALES)'
COLUMN LPAD('RM'||b.TOTAL_SALES_GAIN,11) HEADING 'TOTAL SALES|GAIN'
COLUMN LPAD(TO_CHAR(NVL(b.PERFORMANCE,0)*100,'90.99'),12) HEADING 'PREFORMANCE|GROWTH|(%)'
SELECT LPAD(a.SALES_DATE,10),LPAD(a.TOTAL_SALES_MADE,11), LPAD(NVL(a.NUM_SALES,0),11), LPAD('RM'||b.TOTAL_SALES_GAIN,11), LPAD(TO_CHAR(NVL(b.PERFORMANCE,0)*100,'90.99'),12)
FROM ( 
		SELECT TO_CHAR(sales_date,'MM-YYYY') AS SALES_DATE, COUNT(TO_CHAR(sales_date,'MM-YYYY')) AS TOTAL_SALES_MADE, 
		COUNT(TO_CHAR(sales_date,'MM-YYYY'))-LAG(COUNT(TO_CHAR(sales_date,'MM-YYYY'))) 
		OVER (ORDER BY TO_CHAR(sales_date,'MM-YYYY')) AS NUM_SALES
		FROM SALES S 
		GROUP BY TO_CHAR(sales_date,'MM-YYYY')
		) a,
	(
		SELECT TO_CHAR(sales_date,'MM-YYYY') AS SALES_DATE, SUM(TO_CHAR(sales_price*quantity_sold-quantity_sold*commission_rate*sales_price, '9999.99')) AS TOTAL_SALES_GAIN,
		(SUM(TO_CHAR(sales_price*quantity_sold-quantity_sold*commission_rate*sales_price, '9999.99'))-
		LAG(SUM(TO_CHAR(sales_price*quantity_sold-quantity_sold*commission_rate*sales_price, '9999.99'))) OVER (ORDER BY TO_CHAR(sales_date,'MM-YYYY')))
		/LAG(SUM(TO_CHAR(sales_price*quantity_sold-quantity_sold*commission_rate*sales_price, '9999.99'))) OVER (ORDER BY TO_CHAR(sales_date,'MM-YYYY')) AS PERFORMANCE
		FROM SALES S, SALES_DETAIL SD
		WHERE s.invoice_num = sd.invoice_num
		GROUP BY TO_CHAR(sales_date,'MM-YYYY')
		) b
WHERE a.SALES_DATE = b.SALES_DATE
ORDER BY a.SALES_DATE; 

![image](https://github.com/yeejing0822/foodDropshipManagementDB/assets/86753374/385a55b4-b086-404b-8d58-de1170c4bcae)

6.	Query 6

To check if a particular purchase has arrived at the customer’s destination via postage. To determine the time taken in days for a particular purchase to arrive at the customer's destination via postage. The start date of the shipment and the staff are shown so that the concerning staff can take action if a particular purchase has not arrived after a long time. 

COLUMN NUM HEADING 'INVOICE NUMBER'
COLUMN A HEADING 'STARTED ON'
COLUMN B HEADING 'ARRIVED ON'
COLUMN DATEDIFF HEADING 'TIME TAKEN|(DAY)'
COLUMN R HEADING 'REGION'
SELECT 
	RPAD(SALES.INVOICE_NUM,15) AS NUM, RPAD(REGION_NAME, 15) AS R,
	RPAD(TO_CHAR(SHIP_START_DATE, 'DD-MON-YYYY'),15) AS A, RPAD(NVL(TO_CHAR(SHIP_END_DATE, 'DD-MON-YYYY'), 'NOT ARRIVED YET'),18) AS B,
	RPAD(NVL(TO_CHAR(SHIP_END_DATE- SHIP_START_DATE), '-'),12) AS DATEDIFF, STAFF_NAME
FROM SHIPMENT_DETAIL INNER JOIN POSTAGE ON SHIPMENT_ID = REGION_ID, MARKETING_STAFF INNER JOIN SALES ON MARKETING_STAFF.STAFF_ID = SALES.STAFF_ID
WHERE SHIPMENT_DETAIL.INVOICE_NUM = SALES.INVOICE_NUM 
ORDER BY SHIP_START_DATE;

![image](https://github.com/yeejing0822/foodDropshipManagementDB/assets/86753374/be45751b-342c-4216-8b17-752a0b38c24a)
