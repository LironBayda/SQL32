# SQL32
Answer to the theoretical question 
1. Many to many relationship is a relationship where multiple records in one table are associated with multiple records in second table/ for example: courses and student each student can be enrolled in several courses and each course can has multiple students. 
2. We should create third join table that join between them.

3. The meaning of creating key that consists  of two field in the table is that the two of them together will be the key- we can put the same value in individual key but we cant put the same combination of values 
4. yes
5. integrated security is Windows authentication it mean that SQL need the user need the user name and the password of windows. When false, User ID and Password are specified in the connection. When true, the current Windows account credentials are used for authentication.
6.the different is that  sqlite connection is to a file on the computer and mmsql connection is to server.





write in progresql



/* 1 */
select  CUSTOMER_ID, C."AGE",C."NAME",c."ADDRESS",
       count(PRODUCT_ID)
           as  numOfPro
from "ORDERS"
    join "Customer" C
        on C."ID" = "ORDERS".CUSTOMER_ID
group by( CUSTOMER_ID, C."AGE",C."NAME",c."ADDRESS")
order by  numOfPro desc
limit  1 ;

/* 2 */
select  CUSTOMER_ID, C."AGE",C."NAME",c."ADDRESS",
       sum(p."PRICE")
           as  sumOfPrice
from "ORDERS"
join "Customer" C
    on C."ID" = "ORDERS".CUSTOMER_ID
    join "Products" P
        on P."ID" = "ORDERS".PRODUCT_ID
group by( CUSTOMER_ID, C."AGE",C."NAME",c."ADDRESS")
order by  sumOfPrice
desc
limit  1 ;

/* 3 */
SELECT "ID", "NAME", "ADDRESS", "AGE", customer_id
FROM "Customer"
LEFT JOIN "ORDERS"
ON "Customer"."ID" = "ORDERS".CUSTOMER_ID
where customer_id is  null;

/* 4 */
SELECT  p."NAME","Customer"."NAME", "ADDRESS", "AGE", customer_id
FROM "ORDERS"
LEFT JOIN "Customer"
    ON "Customer"."ID" = "ORDERS".CUSTOMER_ID
LEFT JOIN "Products" P
    ON P."ID" = "ORDERS".PRODUCT_ID
where   p."NAME" is null or "Customer"."NAME" is null

/*5*/
select  CUSTOMER_ID, C."AGE",C."NAME",c."ADDRESS",
       sum(p."PRICE")
           as  sumOfPrice,
       count("ORDERS".PRODUCT_ID)
from "ORDERS"
join "Customer" C
    on C."ID" = "ORDERS".CUSTOMER_ID
    join "Products" P
        on P."ID" = "ORDERS".PRODUCT_ID
group by( CUSTOMER_ID, C."AGE",C."NAME",c."ADDRESS")


/*6*/
select  CUSTOMER_ID,
       avg(p."PRICE")
           as  AvgOfPrice
from "ORDERS"
join "Customer" C
    on C."ID" = "ORDERS".CUSTOMER_ID
    join "Products" P
        on P."ID" = "ORDERS".PRODUCT_ID
group by("ORDERS".CUSTOMER_ID)


/*7*/
with temp7 as (select  CUSTOMER_ID,
       avg(p."PRICE")
           as  AvgOfPrice
from "ORDERS"
join "Customer" C
    on C."ID" = "ORDERS".CUSTOMER_ID
    join "Products" P
        on P."ID" = "ORDERS".PRODUCT_ID
group by("ORDERS".CUSTOMER_ID))

select CUSTOMER_ID, AvgOfPrice, c."NAME",c."ADDRESS",c."AGE" from temp7
join "Customer"  c on temp7.CUSTOMER_ID=c."ID"
where AvgOfPrice>
(select avg(AvgOfPrice) from temp7);



/*8*/

with temp8 as (select  CUSTOMER_ID,
       sum(p."PRICE")
           as  sumOfPrice
from "ORDERS"
join "Customer" C
    on C."ID" = "ORDERS".CUSTOMER_ID
    join "Products" P
        on P."ID" = "ORDERS".PRODUCT_ID
group by("ORDERS".CUSTOMER_ID))

select sum(sumofprice) as sum_of_all_orders_of_all_coustumer from temp8;





/*9*/

with temp9 as(select p."NAME", count("ORDERS".PRODUCT_ID) as buying_number
from "ORDERS"
join "Customer" C
    on C."ID" = "ORDERS".CUSTOMER_ID
    join "Products" P
        on P."ID" = "ORDERS".PRODUCT_ID
group by("ORDERS".PRODUCT_ID,p."NAME")
order by buying_number desc)

select "NAME", buying_number from temp9 where buying_number in
(select  max(temp9.buying_number)
from temp9);

/*10*/

select p."ID",p."NAME",p."PRICE",p."VENDOR", o.PRODUCT_ID from  "Products" p
left join "ORDERS" O on p."ID" = O.PRODUCT_ID
group by p."ID",p."NAME",p."PRICE",p."VENDOR", o.PRODUCT_ID
having O.PRODUCT_ID is null

/* code to create the tables amd insert data*/
CREATE TABLE "Customer" (
	"ID" SERIAL PRIMARY KEY   ,
	"NAME" TEXT NOT NULL,
	"ADDRESS" TEXT NOT NULL,
	"AGE" INT NOT NULL
);


CREATE TABLE "Products" (
	"ID" SERIAL PRIMARY KEY ,
	"NAME" TEXT NOT NULL,
	"PRICE"	INTEGER NOT NULL,
	"CATEGORY" TEXT  NOT NULL,
	"VENDOR" TEXT  NOT NULL
);

CREATE TABLE "ORDERS" (
	PRODUCT_ID INTEGER NOT NULL,
	CUSTOMER_ID INTEGER NOT NULL,
	PRIMARY KEY (PRODUCT_ID, CUSTOMER_ID),
	FOREIGN KEY (PRODUCT_ID) REFERENCES "Products"("ID"),
	FOREIGN KEY (CUSTOMER_ID) REFERENCES "Customer"("ID")
	);

INSERT INTO "Customer" ("NAME", "ADDRESS", "AGE") VALUES ('avi','a street',48);
INSERT INTO "Customer" ("NAME", "ADDRESS", "AGE") VALUES ('aviva','b street',58);
INSERT INTO "Customer" ("NAME", "ADDRESS", "AGE") VALUES ('dana','c street',68);
INSERT INTO "Customer" ("NAME", "ADDRESS", "AGE") VALUES ('ben','d street',44);
INSERT INTO "Customer" ("NAME", "ADDRESS", "AGE") VALUES ('dan','e street',41);
INSERT INTO "Customer" ("NAME", "ADDRESS", "AGE") VALUES ('danny','f street',20);

INSERT INTO "Products" ("NAME", "PRICE", "CATEGORY","VENDOR") VALUES ('a',345,'food','fad');
INSERT INTO "Products" ("NAME", "PRICE", "CATEGORY","VENDOR") VALUES ('b',4,'food','fad');
INSERT INTO "Products" ("NAME", "PRICE", "CATEGORY","VENDOR") VALUES ('c',55,'food','sony');
INSERT INTO "Products" ("NAME", "PRICE", "CATEGORY","VENDOR") VALUES ('d',345875,'food','sony');
INSERT INTO "Products" ("NAME", "PRICE", "CATEGORY","VENDOR") VALUES ('e',45,'home','sony');
INSERT INTO "Products" ("NAME", "PRICE", "CATEGORY","VENDOR") VALUES ('f',654,'home','batili');
INSERT INTO "Products" ("NAME", "PRICE", "CATEGORY","VENDOR") VALUES ('g',12,'home','batili');
INSERT INTO "Products" ("NAME", "PRICE", "CATEGORY","VENDOR") VALUES ('h',34,'home','batili');
INSERT INTO "Products" ("NAME", "PRICE", "CATEGORY","VENDOR") VALUES ('j',567,'home','fad');


INSERT INTO "ORDERS" values (1,2);
INSERT INTO "ORDERS" values (1,3);
INSERT INTO "ORDERS" values (1,1);
INSERT INTO "ORDERS" values (1,4);
INSERT INTO "ORDERS" values (2,2);
INSERT INTO "ORDERS" values (4,2);
INSERT INTO "ORDERS" values (4,1);
INSERT INTO "ORDERS" values (4,3);
INSERT INTO "ORDERS" values (5,1);
INSERT INTO "ORDERS" values (5,2);
INSERT INTO "ORDERS" values (5,3);
INSERT INTO "ORDERS" values (5,4);
INSERT INTO "ORDERS" values (6,3);
INSERT INTO "ORDERS" values (6,2);
INSERT INTO "ORDERS" values (7,2);
