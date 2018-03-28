Collecting data with streams
===

Grouping transactions by currency in imperative style
---

```java
Map<Currency, List<Transaction>> transactionsByCurrencies =
                                                  new HashMap<>();
for (Transaction transaction : transactions) {
    Currency currency = transaction.getCurrency();
    List<Transaction> transactionsForCurrency = transactionsByCurrencies.get(currency);
    if (transactionsForCurrency == null) {
        transactionsForCurrency = new ArrayList<>();
        transactionsByCurrencies.put(currency, transactionsForCurrency);
    }
     transactionsForCurrency.add(transaction);
}
```

Java8:

```java

Map<Currency, List<Transaction>> transactionsByCurrencies =
        transactions.stream().collect(groupingBy(Transaction::getCurrency));
```

Collectors in a nutshell
------

<PAGE-154>
