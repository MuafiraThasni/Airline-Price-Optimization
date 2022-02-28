# Airline-Revue-Optimization
#### Pricing Optimization Project Inspired from Kaggle Micro Challenge

The objective is to optimize generated revenues using dynamic pricing by defining a pricing algorithm able to predict and optimize daily prices in response to a changing daily demand. Think about a transportation, hospitality or entertainment industry selling a fixed amount of tickets for a defined event, flight or time-bound service.

For each event, the pricing function will be run once per day to set that day's ticket price. The seats you don't sell today will be available to sell tomorrow, unless the date of event is reached (for example, the flight leaves that day).

Key problem assumptions:

* Demand is represented by a uniform distribution between 100 and 200. So, for any day in the future, it is equally likely to be each value between 100 and 200 (the demand has equal probabilities to occur).
* You only learn the demand level for each day at the time you need to define price for that day.
* Ticket quantities are capped at the number of seats available.
* Tickets which have not been sold at the date of the event are lost.
* The quantity of tickets you sell on a given day is defined by the simple equation: tickets sold = demand - price (on that day). So you won't sell any tickets if priced at the demand level (walk-away price limit) and you will sell all available tickets if you propose a price equals to the demand less your number of tickets (provided the demand is large enough).
* The sale starts N days before the event with a defined number of tickets to sell. The objective is to maximize revenues generated selling the tickets.

## Initial observations

- Since unsold tickets are lost, the algorithm must check and sell all remaining tickets the day before the last. This will be achieved by setting price = demand - tickets left. In case the demand on the day before the last is below the number of tickets left, a portion will be lost. 

- Revenues = Price x Quantities sold.
  - Quantities sold = demand - price
  - `Revenues = price x (demand - price)`. Similarly, `Revenues = tickets_sold x (demand - tickets_sold)`. Revenues are maximized when the function's derivative is 0.
  - dRevenues/dprice = demand - 2 x price so the optimal price maximizing Revenues is `price* = demand / 2`.
  - Given revenue formula, we find back that we do not generate revenues if price = 0 or if price = demand.
  
-  At optimal price, ticket_sold* = demand - price* = demand - demand/2 = demand/2.
    - This optimal price is only relevant when the number of available tickets is at least half of the demand.
    - The price should not be lower than demand - remaining number of tickets
    - Below is the revenue as a function of price or tickets sold. It assumes demand = 150 and max tickets = 100. The optimum is found at 150 / 2 = 75.

## Optimization Algorithms
* **Brute-Force algorithm :** precomputes all possible best prices given the number of days left and the number of tickets left accross all possible demand levels. It gave an average reveneu of 7574 euros. 
* **Modified BF algorithm with dynamic programming :** To deliver further improvement, Brute-Force algorithm can be improved by usinf a refned version. This approach is called as dynamic programming. Dynamic programming starts by solving an optimization problem in a very limited scenario, and then creates an iterative rule to expand to larger problem. In our case, we first solve for the optimal price when you only have 1 day to sell tickets before the flight. Then we continually step back to longer time-horizons one day at a time. Further improvement is obtained, as Average revenue across all flights became €7596
* **Dynamic Programming BF with Bellman equation for modeling : ** 
