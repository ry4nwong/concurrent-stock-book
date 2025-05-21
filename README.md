# Stock Trading Engine w/ Concurrency

This project is a Python implementation of a multithreaded real-time stock trading engine for matching stock buys with stock sells. It simulates a live market by matching buy and sell orders across up to 1024 stock tickers using a custom-built order book, and avoids using built-in dictionaries or maps by relying on parallel array structures. This was aimed to focus on performance, concurrency, and fairness in order matching.

The trading engine exposes two main functions: `addOrder()` to queue incoming orders, and `matchOrder()` to pair compatible buy and sell orders using a price-time priority approach. Orders are sorted to ensure the most favorable trades are processed first.

Thread safety is enforced using fine-grained locks to prevent race conditions during ticker insertion and order matching. A simulation script launches multiple threads to generate random trading activity, stress-testing the system under realistic concurrent conditions.

This project emphasizes custom data handling, concurrency control, and efficient matching logic—offering a simplified but strong foundation for building a trading platform.

## Features

- Order Matching Logic: Supports efficient matching between buy and sell orders for each ticker using a two-pointer strategy, achieving O(n) time complexity.
- Custom Data Structure: Uses parallel arrays to manage the ticker-to-order list mappings, avoiding built-in map or dictionary structures.
- Thread-Safe Execution: Synchronization is handled using fine-grained locks to maintain correctness during concurrent order submissions and matches.
- Realistic Simulation: A randomized order simulation runs on multiple threads to mimic a high-volume trading environment.

## System Design

The code is split into 5 object-oriented components:

- `stock_order.py`: Defines the StockOrder object, which encapsulates the order type, ticker, quantity, price, and timestamp.

- `stock_map.py`: Implements a custom map structure to associate tickers with their corresponding order lists.

- `order_list.py`: Manages insertion and matching logic for buy/sell orders, sorted by price and time priority.

- `stock_book.py`: Acts as the main engine interface, exposing addOrder() and matchOrder() functions.

- `simulation.py`: Runs a 10-second simulation using 10 threads to stress test the trading engine with random orders.

## Concurrency Strategy

A shared lock protects modifications to the ticker map when inserting new tickers. Each ticker's order lists (buy/sell) have dedicated locks to ensure atomic matching and insertion. Match logic avoids race conditions by ensuring updates to order books happen in a thread-safe block.

## Matching Algorithm

Buy orders are sorted in descending order of price, while sell orders are sorted in ascending order. The matching engine walks through both lists and matches the best available prices using a greedy two-pointer approach. Once a match is made, the quantities are adjusted or removed depending on the result.

