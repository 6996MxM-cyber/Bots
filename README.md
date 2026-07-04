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
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
        
Автоматизированный торговый бот для Binance Futures
Описание

Данный проект представляет собой полностью автоматизированную систему торговли криптовалютными фьючерсами на Binance, разработанную на Python.

Бот круглосуточно отслеживает торговые сигналы, автоматически открывает позиции, рассчитывает размер сделки в соответствии с заданным уровнем риска, выставляет защитные ордера Take Profit и Stop Loss, контролирует их наличие на бирже и сопровождает открытые позиции до полного закрытия.

Проект предназначен для непрерывной автономной работы на VPS или выделенном сервере без участия пользователя.

Основные возможности
Обработка торговых сигналов в реальном времени

Бот постоянно отслеживает файл с торговыми сигналами и моментально реагирует на появление новых записей.

Реализовано:

автоматическое отслеживание изменений файла;
обработка только новых сигналов;
проверка корректности данных;
фильтрация устаревших сигналов;
поддержка LONG и SHORT позиций.
Автоматическое управление рисками

Перед открытием каждой сделки бот автоматически:

получает актуальный баланс Binance Futures;
рассчитывает допустимый риск на сделку;
определяет размер позиции исходя из расстояния до стоп-лосса;
учитывает минимально допустимый размер позиции;
округляет объем согласно требованиям Binance.

Таким образом каждая сделка имеет одинаковый риск независимо от цены актива.

Автоматическое открытие сделок

После получения торгового сигнала бот выполняет полный цикл открытия позиции:

определяет направление сделки;
выбирает необходимый торговый инструмент;
устанавливает кредитное плечо;
отправляет рыночный ордер;
получает фактическую цену исполнения;
рассчитывает уровни Take Profit и Stop Loss;
выставляет защитные ордера.

Все операции выполняются автоматически.

Надежная система защитных ордеров

После открытия позиции бот создает:

Stop Market ордер;
Take Profit ордер.

После создания производится обязательная проверка их существования на стороне Binance.

Если один из ордеров не был создан или был отклонен биржей, система автоматически:

выполняет повторные попытки создания;
проверяет результат каждой попытки;
восстанавливает отсутствующие защитные ордера.

Это значительно снижает вероятность появления незащищенной позиции.

Постоянный контроль открытых сделок

Бот непрерывно контролирует все открытые позиции.

Для каждой сделки проверяется:

наличие Take Profit;
наличие Stop Loss;
корректность данных на бирже;
синхронизация локального состояния с Binance.

При обнаружении несоответствий выполняется автоматическое восстановление или корректное завершение сопровождения сделки.

Автоматическое закрытие по времени

Каждый торговый сигнал содержит максимальное время удержания позиции.

После истечения указанного времени бот автоматически:

закрывает позицию рыночным ордером;
отменяет Take Profit;
отменяет Stop Loss;
удаляет сделку из журнала сопровождения.
Синхронизация с Binance

Для повышения надежности реализованы:

синхронизация времени с сервером Binance;
автоматическая корректировка временного смещения;
обработка ошибок timestamp;
повторная отправка запросов после синхронизации времени;
HMAC SHA256 подпись всех приватных запросов.
Восстановление после перезапуска

Все открытые позиции сохраняются в локальном журнале.

Для каждой сделки записываются:

цена входа;
объем позиции;
идентификатор Take Profit;
идентификатор Stop Loss;
параметры сигнала;
время автоматического закрытия.

После перезапуска программы сопровождение уже открытых сделок продолжается автоматически.

Проверка требований биржи

Перед отправкой любого ордера бот автоматически получает параметры торгового инструмента и учитывает:

минимальный размер позиции;
шаг изменения объема;
минимальный шаг цены;
ограничения Binance Futures.

Все цены и объемы автоматически округляются согласно требованиям биржи.

Отказоустойчивость

Проект рассчитан на длительную непрерывную работу.

Реализованы механизмы:

повторной отправки запросов;
автоматического восстановления защитных ордеров;
обработки сетевых ошибок;
восстановления после ошибок времени Binance;
проверки целостности журналов;
кэширования баланса;
непрерывного мониторинга открытых сделок.
Алгоритм работы
Торговая стратегия
        │
        ▼
Генерация сигнала
        │
        ▼
Запись сигнала в CSV
        │
        ▼
Watchdog обнаруживает изменение файла
        │
        ▼
Проверка корректности сигнала
        │
        ▼
Расчет риска
        │
        ▼
Расчет размера позиции
        │
        ▼
Открытие рыночной сделки
        │
        ▼
Получение фактической цены входа
        │
        ▼
Расчет уровней TP и SL
        │
        ▼
Создание защитных ордеров
        │
        ▼
Проверка успешного создания
        │
        ▼
Сохранение информации о сделке
        │
        ▼
Постоянный мониторинг позиции
        │
        ├── Исполнен TP
        ├── Исполнен SL
        └── Истекло время удержания
                     │
                     ▼
              Закрытие позиции
Ключевые возможности
Полностью автоматическая торговля на Binance Futures.
Автоматический расчет объема позиции по уровню риска.
Динамический расчет Take Profit и Stop Loss.
Контроль существования защитных ордеров.
Автоматическое восстановление отсутствующих TP и SL.
Закрытие сделок по времени.
Синхронизация времени с серверами Binance.
Восстановление состояния после перезапуска программы.
Хранение информации обо всех открытых сделках.
Работа в автономном режиме 24/7.
Повышенная отказоустойчивость и защита от типичных ошибок API Binance.
