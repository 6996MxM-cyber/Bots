Binance Futures Automated Trading Bot
Overview

This project is a fully automated cryptocurrency trading bot for Binance Futures written in Python.

The bot continuously monitors trading signals from an external strategy, automatically opens futures positions, calculates position size based on account risk, places protective Take Profit and Stop Loss orders, manages open positions, and ensures that every trade is properly protected.

The system is designed for 24/7 autonomous operation on a VPS or dedicated server.

Main Features
Real-time signal processing
Watches a CSV file for newly generated trading signals.
Instantly processes only newly added records.
Ignores duplicate, invalid or outdated signals.
Supports LONG and SHORT positions.
Risk Management

Every trade is calculated automatically.

The bot:

retrieves current Futures account balance;
calculates the maximum acceptable risk per trade;
determines position size according to stop-loss distance;
respects exchange minimum quantity requirements;
automatically rounds quantity to Binance LOT_SIZE filters.

This allows every trade to risk a fixed percentage of the account regardless of market price.

Automatic Trade Execution

After receiving a valid signal the bot:

Determines the trading direction.
Selects the correct Futures symbol.
Sets leverage.
Opens a market order.
Retrieves the actual execution price.
Calculates Take Profit and Stop Loss prices.
Creates protection orders.

No manual intervention is required.

Intelligent Protection Orders

Immediately after opening a position the bot creates:

Stop Market order
Take Profit order

The system verifies that both orders actually exist on Binance.

If either order was not created successfully:

retries creation several times;
verifies exchange response;
restores missing protection automatically.

This minimizes the probability of unprotected positions.

Trade Monitoring

The bot continuously monitors every open trade.

It automatically checks:

existence of TP orders;
existence of SL orders;
synchronization with Binance;
missing or cancelled protection.

If one protective order disappears unexpectedly, the remaining order is cancelled and the trade is removed from monitoring to prevent inconsistent states.

Time-based Exit

Each signal contains its own holding period.

When this time expires the bot:

closes the position with a market order;
cancels TP;
cancels SL;
removes the trade from the local database.
Binance Synchronization

The bot continuously synchronizes with Binance.

Implemented features include:

automatic server time synchronization;
timestamp offset correction;
recvWindow handling;
automatic retry after timestamp errors;
REST API authentication;
HMAC SHA256 request signing.
Persistent State

All open trades are stored locally.

The bot saves:

entry price;
quantity;
TP order ID;
SL algorithm ID;
signal information;
expiration time.

If the application restarts, it restores monitoring from disk without losing existing positions.

Exchange Validation

The bot automatically loads Binance exchange filters.

Supported validations include:

LOT_SIZE
PRICE_FILTER
Tick Size
Step Size

Every quantity and price is adjusted to exchange requirements before order submission.

Fault Tolerance

Designed for long-term autonomous execution.

Implemented mechanisms:

retry logic;
exchange verification;
automatic protection recovery;
timestamp synchronization;
corrupted file handling;
balance caching;
continuous monitoring loop.
Workflow
Trading Strategy
        │
        ▼
Signal Generator
        │
        ▼
CSV File
        │
        ▼
Watchdog detects new signal
        │
        ▼
Signal Validation
        │
        ▼
Risk Calculation
        │
        ▼
Market Order
        │
        ▼
Retrieve Actual Entry Price
        │
        ▼
Calculate TP / SL
        │
        ▼
Create Protection Orders
        │
        ▼
Verify Protection
        │
        ▼
Store Trade Information
        │
        ▼
Continuous Monitoring
        │
        ├── TP executed
        ├── SL executed
        └── Hold time expired
                 │
                 ▼
        Close Position
Technologies
Python
Binance Futures API
REST API
HMAC Authentication
Watchdog
Pandas
Requests
CSV Processing
Decimal Precision
Risk Management
Automated Trade Execution
Key Capabilities
Automated Futures trading
Position sizing by account risk
Dynamic Take Profit / Stop Loss
Continuous order verification
Automatic recovery of missing protection
Time-based exits
Binance server synchronization
Persistent trade storage
24/7 autonomous operation
Fault-tolerant architecture
