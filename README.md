# mysql
/* print details of shipments (sales) where amounts are > 2,000 and boxes are < 100*/
 SELECT *
FROM sales
WHERE amount > 2000 AND boxes < 100;

/* how many shipments (sales) each of the sales persons had in the month of january 2022*/
SELECT p.salesperson, COUNT(*) AS shipment
FROM sales s
JOIN people p
ON s.spid = p.spid
WHERE saledate BETWEEN '2022-01-01' AND '2022-01-31'
GROUP BY p.salesperson;


/*which product sold more boxes in the first seven days of february 2022? milk bars or eclairs*/
SELECT pr.product, SUM(boxes) AS 'total boxes'
FROM sales s
JOIN products pr
ON s.pid = pr.pid
WHERE pr.product IN ('milk bars', 'eclairs')
and s.saledate between '2022-02-01' and '2022-02-07'
GROUP BY pr.product;

/* what are the names of salesperson who had at least one sale in the first 7 days of january 2022*/
select distinct p.salesperson
from people p
join sales s
on p.spid = s.spid
where s.saledate between '2022-01-01' and '2022-01-07'
group by p.salesperson;


/* which salesperson did not make any sale in the first 7 days of january 2022*/
select p.salesperson
from people p
where p.spid not in
	( select DISTINCT s.spid
	   from sales s
	    WHERE s.saledate BETWEEN '2022-01-01' AND '2022-01-07'
	)
	

/*how many times we shipped more than 1000 boxes in each month*/	
select year(saledate) 'year', month(saledate) 'month', COUNT(*) AS 'total shipment'
from sales
where boxes > 1000
group by year(saledate), month(saledate)
order by YEAR(saledate), MONTH(saledate);


/*india or australia? who buys more chocolate boxes on a monthly basis*/
SELECT YEAR(saledate) 'year', MONTH(saledate) 'month', COUNT(*) AS 'total shipment',
sum(case when g.geo= 'india' then 'yes' 
      else 'no' end as 'india countries')
 SUM(CASE WHEN g.geo= 'australia' THEN 'yes' 
      ELSE 'no' END AS 'australia countries' )
FROM sales s
join geo g
on s.geoid = g.geoid
GROUP BY YEAR(saledate), MONTH(saledate)
ORDER BY YEAR(saledate), MONTH(saledate);

/* sales by salesperson, number of boxes and amount */
SELECT p.salesperson, COUNT(boxes), SUM(boxes), SUM(amount)
FROM people p
JOIN sales s
ON p.spid = s.spid
GROUP BY p.salesperson
ORDER BY SUM(amount) DESC;

/* sales by salesperson, number of boxes and amount */
SELECT p.salesperson, COUNT(boxes), SUM(boxes), SUM(amount)
FROM people p
JOIN sales s
ON p.spid = s.spid
GROUP BY p.salesperson
ORDER BY SUM(amount) DESC;

/* print details of shipments (sales) where amounts are > 2,000 and boxes are < 100*/
SELECT *
FROM sales
WHERE amount > 2000 AND boxes < 1000;

/* how many shipments (sales) each of the sales persons had in the month of january 2022*/
SELECT p.salesperson, COUNT(*) 'shipment'
FROM people p
JOIN sales s
ON s.spid = p.spid
WHERE s.saledate BETWEEN '2022-01-01'  AND '2022-01-31'
GROUP BY p.salesperson;

/*which product sells more boxes?  milk bars or eclairs*/
select product, sum(boxes)
from products pr
join sales s
on pr.pid = s.pid
where pr.product in ('milk bars' , 'eclairs')
group by product;

/*which product sold more boxes in january 2022? milk bars or eclairs*/
SELECT pr.product, SUM(boxes)
FROM products pr
JOIN sales s
ON pr.pid = s.pid
WHERE pr.product IN ('milk bars' , 'eclairs') and s.saledate BETWEEN '2022-01-01'  AND '2022-01-31';
GROUP BY product;

/*which shipments had under 100 customers & under 100 boxes. did any of them occur on wednesday*/
select *,
	case when weekday(saledate) = 3 then 'wednesday shipment'
	else 'otherday shipment' end as 'shipment days'
from sales
where customers < 100 and boxes < 100;
